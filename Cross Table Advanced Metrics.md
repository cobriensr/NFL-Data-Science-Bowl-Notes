# 1. Player Performance Contextualized

## Success Rate by Formation
**Required Columns:**
- plays.offenseformation
- plays.yardsgained
- plays.down
- plays.yardstogo
- player_play.nflid
- players.position

**Calculation:**
```sql
successful_play = CASE
    WHEN down = 1 AND yardsgained >= (yardstogo * 0.4) THEN 1
    WHEN down = 2 AND yardsgained >= (yardstogo * 0.6) THEN 1
    WHEN down IN (3,4) AND yardsgained >= yardstogo THEN 1
    ELSE 0
END

success_rate = SUM(successful_play) / COUNT(*) 
GROUP BY offenseformation, nflid
```

## Yards Per Route by Route Type
**Required Columns:**
- player_play.wasrunningroute
- player_play.routeran
- player_play.receivingyards
- plays.passresult
- players.position

**Calculation:**
```sql
yards_per_route = SUM(receivingyards) / COUNT(CASE WHEN wasrunningroute = true THEN 1 END)
GROUP BY routeran, nflid
```

## Pressure Rate by Protection Scheme
**Required Columns:**
- plays.offenseformation
- player_play.pressureallowedasblocker
- player_play.timetopressureallowedasblocker
- plays.dropbacktype
- players.position

**Calculation:**
```sql
pressure_rate = SUM(CASE WHEN pressureallowedasblocker = true THEN 1 ELSE 0 END) / 
                COUNT(DISTINCT playid)
GROUP BY offenseformation, dropbacktype
```

## Completion Percentage Under Pressure
**Required Columns:**
- plays.passresult
- player_play.causedpressure
- player_play.timetopressureaspassrusher
- players.position

**Calculation:**
```sql
completion_pct_under_pressure = 
    SUM(CASE WHEN passresult = 'COMPLETE' AND causedpressure = true THEN 1 ELSE 0 END) /
    COUNT(CASE WHEN causedpressure = true THEN 1 END)
```

## Yards After Catch by Route Type
**Required Columns:**
- player_play.routeran
- player_play.yardagegainedafterthecatch
- plays.passresult
- players.position

**Calculation:**
```sql
yac_by_route = AVG(yardagegainedafterthecatch)
GROUP BY routeran
```

# 2. Team Tendencies

## Formation Frequency by Down and Distance
**Required Columns:**
- plays.offenseformation
- plays.down
- plays.yardstogo
- plays.quarter
- plays.gameclock

**Calculation:**
```sql
formation_frequency = COUNT(*) / 
    SUM(COUNT(*)) OVER (PARTITION BY down, 
        CASE WHEN yardstogo <= 3 THEN 'SHORT'
             WHEN yardstogo <= 7 THEN 'MEDIUM'
             ELSE 'LONG' END)
GROUP BY offenseformation, down, yardstogo_category
```

## Blitz Frequency by Situation
**Required Columns:**
- player_play.wasinitialpassrusher
- plays.down
- plays.yardstogo
- plays.absoluteyardlinenumber
- plays.presnaphometeamwinprobability

**Calculation:**
```sql
blitz_frequency = 
    COUNT(DISTINCT CASE WHEN wasinitialpassrusher = true THEN player_play.nflid END) /
    COUNT(DISTINCT plays.playid)
GROUP BY down,
    CASE WHEN yardstogo <= 3 THEN 'SHORT'
         WHEN yardstogo <= 7 THEN 'MEDIUM'
         ELSE 'LONG' END,
    CASE WHEN absoluteyardlinenumber <= 20 THEN 'RED_ZONE'
         WHEN absoluteyardlinenumber <= 50 THEN 'MIDDLE_FIELD'
         ELSE 'BACKED_UP' END
```

## Personnel Grouping Tendencies
**Required Columns:**
- players.position
- player_play.nflid
- plays.offenseformation
- plays.down
- plays.yardstogo

**Calculation:**
```sql
-- First calculate personnel package (e.g., 11, 12, 21 personnel)
personnel_package = CONCAT(
    COUNT(CASE WHEN position = 'RB' THEN 1 END),
    COUNT(CASE WHEN position = 'TE' THEN 1 END)
)

-- Then calculate usage frequency
usage_frequency = COUNT(*) / SUM(COUNT(*)) OVER ()
GROUP BY personnel_package, down, yardstogo
```

# 3. Situational Analysis

## Third Down Conversion Efficiency
**Required Columns:**
- plays.down
- plays.yardstogo
- plays.yardsgained
- plays.possessionteam
- plays.playdescription

**Calculation:**
```sql
conversion_rate = 
    SUM(CASE WHEN yardsgained >= yardstogo THEN 1 ELSE 0 END) /
    COUNT(*)
WHERE down = 3
GROUP BY possessionteam,
    CASE WHEN yardstogo <= 3 THEN 'SHORT'
         WHEN yardstogo <= 7 THEN 'MEDIUM'
         ELSE 'LONG' END
```

## Red Zone Efficiency
**Required Columns:**
- plays.absoluteyardlinenumber
- plays.presnaphomescore
- plays.presnapvisitorscore
- plays.possessionteam
- plays.playdescription

**Calculation:**
```sql
redzone_efficiency = 
    SUM(CASE WHEN playdescription LIKE '%TOUCHDOWN%' THEN 7
             WHEN playdescription LIKE '%FIELD GOAL%' THEN 3
             ELSE 0 END) /
    COUNT(DISTINCT CASE WHEN absoluteyardlinenumber <= 20 THEN plays.gameid || plays.playid END)
GROUP BY possessionteam
```

## Two Minute Drill Efficiency
**Required Columns:**
- plays.quarter
- plays.gameclock
- plays.yardsgained
- plays.possessionteam
- plays.presnaphometeamwinprobability

**Calculation:**
```sql
two_minute_efficiency = 
    AVG(yardsgained)
WHERE (quarter = 2 OR quarter = 4) 
  AND EXTRACT(MINUTES FROM gameclock) <= 2
GROUP BY possessionteam
```

# 4. Advanced Strategy Metrics

## Defensive Adjustment Success
**Required Columns:**
- plays.pff_passcoverage
- plays.expectedpoints
- plays.expectedpointsadded
- player_play.pff_defensivecoverageassignment

**Calculation:**
```sql
defensive_success = 
    AVG(CASE WHEN expectedpointsadded < 0 THEN 1 ELSE 0 END)
GROUP BY pff_passcoverage, defensiveteam
```

## Offensive Tendency Breaking
**Required Columns:**
- plays.offenseformation
- plays.playdescription
- plays.down
- plays.yardstogo
- plays.expectedpointsadded

**Calculation:**
```sql
-- First establish tendency
common_play = MODE() OVER (PARTITION BY down, yardstogo_category, offenseformation)

-- Then measure success when breaking tendency
tendency_break_success = 
    AVG(expectedpointsadded)
WHERE play_type != common_play
GROUP BY possessionteam
```

## Matchup Exploitation Rate
**Required Columns:**
- player_play.pff_primarydefensivecoveragematchupnflid
- player_play.receivingyards
- players.position
- plays.expectedpointsadded

**Calculation:**
```sql
matchup_exploitation = 
    AVG(CASE WHEN receivingyards > 10 AND expectedpointsadded > 0 THEN 1 ELSE 0 END)
GROUP BY pff_primarydefensivecoveragematchupnflid, nflid
```

## Game Script Adaptation
**Required Columns:**
- plays.presnaphometeamwinprobability
- plays.possessionteam
- plays.playdescription
- plays.offenseformation
- plays.expectedpointsadded

**Calculation:**
```sql
adaptation_success = 
    AVG(expectedpointsadded)
GROUP BY 
    possessionteam,
    CASE WHEN presnaphometeamwinprobability > 0.7 THEN 'LEADING_BIG'
         WHEN presnaphometeamwinprobability > 0.5 THEN 'LEADING'
         WHEN presnaphometeamwinprobability > 0.3 THEN 'TRAILING'
         ELSE 'TRAILING_BIG' END
```
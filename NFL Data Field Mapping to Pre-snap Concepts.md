# 1. Offensive Formation Indicators

## A. Quarterback Alignment
### Under Center
```
Relevant Fields:
tracking_week_[N].csv:
- x, y: QB position relative to center
- o: QB orientation
- dir: Direction facing
- frameId: Pre-snap sequence
- event: Snap identification

plays.csv:
- offenseFormation: Formation type
- dropbackType: Indicates under center vs shotgun
```

### Shotgun
```
Same fields as Under Center, plus:
plays.csv:
- dropbackDistance: Distance QB is from center
- isDropback: Dropback play indicator
```

### Pistol
```
Same fields as Shotgun, plus:
tracking_week_[N].csv:
- Additional RB positioning (x, y) relative to QB
```

## B. Running Back Position

### I-Formation
```
tracking_week_[N].csv:
- x, y: RB and FB coordinates
- o: Player orientation
- nflId: Player identification

players.csv:
- position: Identify RB/FB
- nflId: Link to tracking data

plays.csv:
- offenseFormation: Formation identification
- pff_runConceptPrimary: Run scheme
```

### Single Back
```
Same fields as I-Formation, minus FB tracking
```

### Empty Backfield
```
plays.csv:
- offenseFormation: Empty indication
- receiverAlignment: Receiver distribution

tracking_week_[N].csv:
- Absence of RB coordinates in backfield
```

## C. Receiver Alignments

### Tight Splits
```
tracking_week_[N].csv:
- x, y: WR/TE positions
- nflId: Player identification

players.csv:
- position: WR/TE identification

plays.csv:
- receiverAlignment: Split description
- offenseFormation: Formation context
```

### Wide Splits
```
Same fields as Tight Splits, plus:
player_play.csv:
- routeRan: Route type from position
```

# 2. Pre-snap Movement Strategies

## A. Motion Types

### Jet Motion
```
tracking_week_[N].csv:
- s: Player speed
- x, y: Motion path
- dir: Movement direction
- frameId: Motion timing

player_play.csv:
- inMotionAtBallSnap: Motion at snap indicator
- motionSinceLineset: Motion type
```

### Shift Motion
```
player_play.csv:
- shiftSinceLineset: Formation shift indicator
- motionSinceLineset: Motion indicator

tracking_week_[N].csv:
- frameId: Shift timing
- x, y: Position changes
```

### Trading Places
```
tracking_week_[N].csv:
- x, y: Position changes
- frameId: Timing sequence
- nflId: Player tracking

player_play.csv:
- shiftSinceLineset: Position change indicator
```

## B. Purpose of Motion

### Diagnostic
```
plays.csv:
- pff_passCoverage: Coverage faced
- pff_manZone: Coverage type

player_play.csv:
- pff_defensiveCoverageAssignment: Defensive response
```

### Tactical
```
tracking_week_[N].csv:
- Event sequences
- Position changes

plays.csv:
- playAction: Play-action indicator
- yardsGained: Play success
```

# 3. Defensive Alignment Keys

## A. Safety Alignment

### Single-High
```
tracking_week_[N].csv:
- x, y: Safety positions
- o: Safety orientation
- nflId: Player identification

players.csv:
- position: "FS"/"SS" identification

plays.csv:
- pff_passCoverage: Coverage type
```

### Two-High
```
Same fields as Single-High

player_play.csv:
- pff_defensiveCoverageAssignment: Coverage responsibility
```

### Safety Rotation
```
tracking_week_[N].csv:
- x, y: Position changes
- dir: Movement direction
- frameId: Rotation timing
```

## B. Cornerback Technique

### Press Coverage
```
tracking_week_[N].csv:
- x, y: CB position relative to WR
- o: CB orientation

player_play.csv:
- pff_defensiveCoverageAssignment: Coverage type
```

### Off Coverage
```
Same fields as Press Coverage, plus:
plays.csv:
- pff_passCoverage: Coverage scheme
```

## C. Linebacker Depth

### Stacked
```
tracking_week_[N].csv:
- x, y: LB positions
- o: LB orientation

players.csv:
- position: "LB" identification
```

### Walked Up
```
Same fields as Stacked, plus:
plays.csv:
- pff_passCoverage: Coverage impact
```

# 4. Personnel Package Implications

## A. Offensive Personnel

### 11 Personnel
```
players.csv:
- position: Player positions
- nflId: Player identification

tracking_week_[N].csv:
- nflId: Player tracking
- frameId: Personnel timing
```

### 12 Personnel
```
Same fields as 11 Personnel

plays.csv:
- offenseFormation: Formation with personnel
```

### 21 Personnel
```
Same fields as 12 Personnel
```

## B. Defensive Personnel

### Base Defense
```
players.csv:
- position: Defensive positions
- nflId: Player identification

plays.csv:
- defensiveTeam: Team identification
```

### Nickel Package
```
Same fields as Base Defense, plus:
player_play.csv:
- pff_defensiveCoverageAssignment: Coverage responsibilities
```

### Dime Package
```
Same fields as Nickel Package
```

# 5. Situational Indicators

## A. Down and Distance
```
plays.csv:
- down: Down number
- yardsToGo: Distance needed
- absoluteYardlineNumber: Field position
- quarter: Game period
- gameClock: Time remaining
- playClockAtSnap: Play clock
```

## B. Field Position
```
plays.csv:
- yardlineSide: Side of field
- yardlineNumber: Yard line
- absoluteYardlineNumber: Distance from goal

games.csv:
- gameId: Game context
- homeTeamAbbr/visitorTeamAbbr: Team identification
```

# 6. Additional Pre-snap Keys

## A. Offensive Line Setup
```
tracking_week_[N].csv:
- x, y: OL positions
- o: OL orientation
- dir: OL facing

players.csv:
- position: OL position identification
```

## B. Defensive Line Technique
```
tracking_week_[N].csv:
- x, y: DL alignment
- o: DL orientation

player_play.csv:
- wasInitialPassRusher: Rush designation
```

## C. Communication
```
tracking_week_[N].csv:
- frameId: Timing of adjustments
- event: Pre-snap events

plays.csv:
- playClockAtSnap: Time used
```

[[Additional Derived Datapoints Based on Football Knowledge]]
[[Entity Relationship Diagram]]
[[Pre-snap Football Knowledge]]
1. GAMES Table Additional Derivations:

- home_win_flag (boolean based on final scores)
- game_competitive_flag (boolean, perhaps if score difference <= 7 points)
- primetime_game_flag (based on gametimeeastern, e.g., after 7PM EST)
- divisional_game_flag (could be derived if we know team divisions)
- day_of_week (from gamedate)
- playoff_week_flag (based on week number > 18)
- bye_week_teams (teams not playing in a given week)

2. PLAYS Table Additional Derivations:

- drive_number (can be calculated by sequencing plays per game)
- drive_plays_count (how many plays per drive)
- red_zone_play_flag (based on absoluteyardlinenumber <= 20)
- goal_to_go_flag (when yardstogo equals absoluteyardlinenumber)
- scoring_play_flag (when points change between plays)
- two_minute_warning_flag (based on gameclock near 2:00 in 2nd/4th quarters)
- clutch_play_flag (close game in 4th quarter)
- successful_play_flag (based on down/distance gained):
    - 1st down: gained 40% of needed yards
    - 2nd down: gained 60% of needed yards
    - 3rd/4th down: gained 100% of needed yards
- field_position_category (own territory vs opponent territory)
- hash_mark_position (derived from x coordinates)
- play_duration (using gameclock differences)
- tempo_rating (time between plays)

3. PLAYER_PLAY Table Additional Derivations:

- fantasy_points (based on standard scoring)
- ppr_fantasy_points (point per reception scoring)
- half_ppr_fantasy_points
- quarterback_rating (for passing plays)
- yards_per_attempt
- yards_per_completion
- yards_after_contact (for rushing plays)
- broken_tackles (could be derived from tackle data)
- pressure_rate (on passing plays)
- blitz_recognition (based on pressure handling)
- target_share (percentage of team's targets)
- route_running_efficiency (yards gained vs route length)
- blocking_efficiency
- pass_rush_win_rate
- coverage_grade (based on targets/completions allowed)
- missed_tackle_rate

4. PLAYERS Table Additional Derivations:

- age (from birthdate)
- experience_years (from current season - rookie year)
- height_inches (convert from height string)
- bmi (using height and weight)
- position_group (grouping similar positions):
    - Offensive Line (T, G, C)
    - Skill Positions (WR, RB, TE)
    - Front Seven (DE, DT, LB)
    - Secondary (CB, S)
- special_teams_player_flag

5. TRACKING_WEEK_1 Table Additional Derivations:

- total_distance_covered (sum of dis per play)
- average_speed (mean of s per play)
- top_speed (max of s per play)
- acceleration_bursts (count of high a values)
- change_of_direction_count (based on dir changes)
- route_tree_classification (for receivers)
- coverage_shell_position (for defensive backs)
- separation_distance (from nearest defender)
- closing_speed (relative speed to target)
- pursuit_angle (using dir and o)
- time_to_contact (based on relative positions/speeds)
- player_congestion (nearby players count)
- formation_spacing_metrics
- pre_snap_movement_distance
- sprint_burst_count (periods of high speed)

Cross-Table Advanced Metrics: [[Cross Table Advanced Metrics]]

1. Player Performance Contextualized:

- success_rate_by_formation
- yards_per_route_by_route_type
- pressure_rate_by_protection_scheme
- completion_percentage_under_pressure
- yards_after_catch_by_route_type
- tackle_efficiency_by_play_type
- coverage_success_by_receiver_type

2. Team Tendencies:

- formation_frequency_by_down_and_distance
- blitz_frequency_by_situation
- personnel_grouping_tendencies
- play_calling_tendencies_by_field_position
- substitution_patterns
- hurry_up_offense_frequency

3. Situational Analysis:

- third_down_conversion_efficiency
- red_zone_efficiency
- two_minute_drill_efficiency
- goal_line_package_success_rate
- fourth_down_decision_making

4. Advanced Strategy Metrics:

- defensive_adjustment_success
- offensive_tendency_breaking
- matchup_exploitation_rate
- situational_awareness_score
- game_script_adaptation

[[Inferred and Derived Columns]]
[[Entity Relationship Diagram]]
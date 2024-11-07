1. GAMES Table Calculated Columns:

- total_points_scored = homefinalscore + visitorfinalscore
- score_difference = ABS(homefinalscore - visitorfinalscore)
- winning_team = CASE WHEN homefinalscore > visitorfinalscore THEN hometeamabbr ELSE visitorteamabbr END
- margin_of_victory = ABS(homefinalscore - visitorfinalscore)

2. PLAYS Table - playdescription -> extract these additional data points:

##### Formation Information:

- actual_formation (parsed from playdescription)
- formation_mismatch_flag (boolean comparing actual_formation vs offenseformation)
	- ==I noticed that the offense formation in the playdescription column value did not always match the value given in the offenseformation column value==
##### Play Details:

- is_no_huddle (boolean)
	- This can be used to look at what might indicate a tempo/no-huddle offense
- passing_direction (left/right/middle)
	- This can be used in conjunction with defensive plays to look at propensity to throw the ball towards a particular spot in the field
- pass_depth (short/deep)
	- This can be used in conjunction with defensive plays to look at preferred pass length
		- defensive formation
		- particular defensive player in coverage
		- avg yards to go
		- specific down
- yards_after_catch (when mentioned, like "Pass 0, YAC 8")
	- Can be used to look at relationships between receivers, coverage formations and players in coverage
- play_result_type (TOUCHDOWN, INCOMPLETE, FUMBLE, INTERCEPTION)
	- Can be used to provide analysis on successful play ratio when taking into account avg yards to go, specific down, defensive formation and players involved. 

##### Penalty Information:

- penalty_occurred (boolean)
- penalty_team
	- Interesting to see if one team is more heavily penalized than the other over all of the game samples and look at the relationships between teams
- penalty_player
	- Interesting to see if a particular player has more propensity towards a particular type of penalty such as defensive pass interference on a cornerback or roughing the passer from the interior defensive positions or edge rushers
- penalty_type
	- Interesting to see if a particular player has more propensity towards a particular type of penalty such as defensive pass interference on a cornerback or roughing the passer from the interior defensive positions or edge rushers
- penalty_yards
	- Avg penalty yards vs number of penalties committed
- penalty_enforced_between_downs (boolean)

##### Turnover Information:

- turnover_type (FUMBLE/INTERCEPTION)
- turnover_recovered_by_team
- turnover_recovered_by_player
- turnover_reviewed (boolean)
- review_ruling (UPHELD/REVERSED)
	- This is especially interesting to see what might influence calls being reviewed or upheld and what % of calls incur this action to compare to the upheld vs reverse ratio

##### Defensive Information:

- primary_tackler
	- This can help us determine how good a player is in coverage and if there is a particular type of defensive formation that they excel in or if there is a particular matchup that they are better at
- assist_tackler
- defensive_action (tackle, interception, forced fumble)
	- This can help predict the turnover ratio on avg number of plays when taking into account down and distance to go

[[Entity Relationship Diagram]]

![[Pasted image 20241106220447.png]]
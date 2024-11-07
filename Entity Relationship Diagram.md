1. Primary Key/Foreign Key Relationships:

- `GAMES.gameid` is referenced by `PLAYS.gameid`
- `PLAYS.(gameid, playid)` is referenced by `PLAYER_PLAY.(gameid, playid)`
- `PLAYERS.nflid` is referenced by `PLAYER_PLAY.nflid`
- `PLAYS.(gameid, playid)` is referenced by `TRACKING_WEEK_1.(gameid, playid)`
- `PLAYERS.nflid` is referenced by `TRACKING_WEEK_1.nflid`

2. Common Join Columns:

- Between GAMES and PLAYS: `gameid`
- Between PLAYS and PLAYER_PLAY: `gameid` and `playid`
- Between PLAYERS and PLAYER_PLAY: `nflid`
- Between PLAYS and TRACKING_WEEK_1: `gameid` and `playid`
- Between PLAYERS and TRACKING_WEEK_1: `nflid`

3. Cardinalities

![[Pasted image 20241106213235.png]]

![[Pasted image 20241106212640.png]]
+++
title = "Interactive Game Rotation Plots"
date = 2024-04-07
description = "Web app for visualizing player shifts + lineups in any desired game this season"

[taxonomies]
tags = ["game rotations", "shiny"]

[extra]
footnote_backlinks = true
quick_navigation_buttons = true
+++

### Web app here!

Generate (and customize) a rotation plot for a game of choice in the 2023-24 season at [apps.plotandroll.com/game_rotation_app/](https://apps.plotandroll.com/game_rotation_app/)!

### Backstory

I've been [posting](https://twitter.com/d_lavrent/status/1719472374452486650) a plot like this after each Cavs game in the 2023-24 NBA season:
![Example rotation plot](img/nba_lineup_plot_2024-02-27_DAL-CLE_401585460.png "Example rotation plot")*Example rotation plot of the [Max Strus 59-foot buzzer beater](https://www.youtube.com/watch?v=9FjvUOttXMg&ab_channel=NBA) game*

To do this, I used the excellent [SportsDataverse](https://sportsdataverse-py.sportsdataverse.org/) Python package to pull play-by-play information from a game using an ESPN ID (the numeric code you see in an ESPN boxscore, i.e. the `401585460` in [https://www.espn.com/nba/game/_/gameId/401585460/mavericks-cavaliers](https://www.espn.com/nba/game/_/gameId/401585460/mavericks-cavaliers)).

From the play-by-play data, I parsed out substitution times for each player, and counted the game score at each substitution time to get +/-'s for each player shift. This worked pretty well, but I noted sometimes my +/- counts would occasionally be off by a point or two (I'm pretty sure I didn't manage some edge cases correctly like when players get subbed onto the floor during another player's free throws).

(I promise to release the sportsdataverse-based code to GitHub by the way, I need to reorganize some stuff!)


But also, a couple of weeks into the season, I learned about the [nba_api](https://github.com/swar/nba_api) package and was blown away by the volume of data I've always wanted access to. One of the endpoints that this API client lets you get from [nba.com/stats](nba.com/stats) is called [GameRotation](https://github.com/swar/nba_api/blob/master/docs/nba_api/stats/endpoints/gamerotation.md), and, given an game ID (an nba.com one, different from ESPN), yields the following type of information an hour or two after the game is played:

{% wide_container() %}

| GAME_ID  | PLAYER_FIRST | PLAYER_LAST  | IN_TIME_REAL | OUT_TIME_REAL | PT_DIFF |
|-----------|--------------|--------------|--------------|----------|---------|
| 22300832 | Kyrie        | Irving       | 0            | 4930         | 4       |
| 22300832 | Kyrie        | Irving       | 7200         | 12920        | -12     |
| 22300832 | Kyrie        | Irving       | 14400        | 18900        | 5       |

{% end %}

Perfect -- in/out times for each player stint, along with the point differential.

So, I swapped out my manual parsing code with the data from the much more legit nba.com, took a few weeks to learn [Shiny Server](https://shiny.posit.co/py/) for Python, along with automating the accessing and storing of these `GameRotation` dataframes, and made the web app linked above.

Documentation will come on GitHub, I promise -- but for now, please try out the web app, and I'm happy to hear feedback or answer any questions or field suggestions!
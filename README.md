# Point Guard Height and Team Efficiency: Regular Season vs. Playoffs

An econometric analysis of whether the height of an NBA team's starting point guard affects team offensive and defensive efficiency, and whether that relationship changes between the regular season and the playoffs.

Final project for **SAL 384 (Sport Analytics, Syracuse University)**
## Research Question

Taller point guards such as Shai Gilgeous-Alexander, Cade Cunningham, and Luka Doncic have become more common, and the 2025 champion Oklahoma City Thunder leaned into size. Does a starting point guard's height actually predict team efficiency, and does that effect differ in the playoffs?

Prior work (Teramoto & Cross, 2018) found that team height interacts with offensive efficiency in the playoffs, but it averaged height across all five positions. This project isolates **one position**, the point guard, to test whether that position drives the effect.

## Data

- **Source:** [Historical NBA Data and Player Box Scores](https://www.kaggle.com/datasets/eoinamoore/historical-nba-data-and-player-box-scores) (Amoore, 2024), a public Kaggle dataset. I used it instead of the NBA stats API because of repeated access restrictions.
- **Final sample:** 17,722 team-game observations (16,558 regular season, 1,164 playoff) across 7 seasons, 2017-18 through 2024-25, excluding 2021-22, covering all 30 teams.
- **Cleaning:** Kept games with exactly 5 recorded starters, kept only regular season and playoff games (dropped All-Star, Play-In, and NBA Cup), and excluded 2016-17 (duplicated lineup data) and 2021-22 (lineup data almost entirely missing).

## Methods

**Identifying the starting point guard.** Box scores label starters only as guard, forward, or center. For each game with two starting guards, I classified the one with more assists as the point guard (minutes played broke ties). Validation: the identified point guard averaged 3.91 more assists than the other guard, and ties occurred in only 8.5% of games.

**Model.** Pooled OLS, run separately for team offensive rating and team defensive rating:

```
TeamRtg = b0 + b1(PG Height z-score) + b2(Playoff) + b3(PG Height x Playoff)
          + b4(Other Starters' Avg Height) + b5(Opponent Quality) + b6(Home)
          + Season Fixed Effects + error
```

- PG height is standardized within each season, so guards are compared with their contemporaries.
- Other starters' average height separates point guard height from overall team size.
- Opponent quality uses each opponent's regular season ratings to avoid noisy playoff-only samples.
- Standard errors are clustered at the team-season level. Team-season fixed effects would absorb nearly all the variation in PG height, so I did not use them.
- The coefficient of interest is **b3**, the height-by-season-type interaction.

## Key Findings

| | Offensive Rating | Defensive Rating |
|---|---|---|
| PG height x Playoff (b3) | 0.308 (p = .429) | -0.318 (p = .459) |
| PG height (b1) | 0.074 (p = .609) | 0.017 (p = .906) |
| Playoff (b2) | 0.687 (p = .064) | **-2.965 (p < .001)** |

- **No evidence that point guard height matters for team efficiency**, in either season type. The interaction was not significant in either model.
- **Defense improves league-wide in the playoffs.** Team defensive rating improved by almost 3 points per 100 possessions in the playoffs, regardless of point guard height.
- **Interpret the null result carefully.** The playoff sample is small (1,164 games), which limits statistical power, and starting point guards vary little in height (SD of about 2.3 inches). The data show an absence of detected evidence, not proof that no effect exists.


## Limitations

- Rest days between games were not controlled for (planned for a future version).
- The assist-based point guard proxy may misclassify some players, such as high-assist combo guards.
- Excluding two seasons leaves a gap in the panel, though the data issues appear unrelated to height or efficiency.
- Opponent quality uses full-season averages, so it misses game-to-game form.

## Next Steps

Apply the same approach to centers and forwards to find which position drives the team-height playoff effect, add rest days as a control, use a larger sample, and replace the assist proxy with a more precise point guard classification.
```

## Tools

Python, [pandas, statsmodels, matplotlib], Jupyter

## Author

**Zeke Snyder**, Sport Analytics, Syracuse University

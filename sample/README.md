# NCAAB — Historical Closing Odds (V2) — SAMPLE

This sample holds 27 rows of the full file: the first 10 rows and the last 10 rows, so both ends of the history can be checked, and the last 7 games of the 2026 NCAA Tournament (Elite Eight, Final Four and final). Same columns and same format as the full file; only the number of rows changes. The README of the full dataset follows.

---

## NCAAB — Historical Closing Odds (V2)

NCAA men's college basketball, one CSV, updated to **2 November 2026** (games played up to that day, North American time). This is the same file as the NCAAB file of the US Sports Master V2.

Source: OddsPortal. The odds are the average closing prices shown by the source, not attributable to any single bookmaker.

## Coverage

| Seasons | First match | Last match | Rows |
|---|---|---|---|
| 2008/2009 – 2026/2027 | 2008-11-20 | 2026-11-03 | 79,780 |

| Season | Rows | Season | Rows |
|---|---|---|---|
| 2008/2009 | 774 | 2018/2019 | 5,548 |
| 2009/2010 | 3,318 | 2019/2020 | 5,326 |
| 2010/2011 | 3,579 | 2020/2021 | 4,093 |
| 2011/2012 | 3,667 | 2021/2022 | 5,480 |
| 2012/2013 | 3,821 | 2022/2023 | 5,824 |
| 2013/2014 | 4,080 | 2023/2024 | 5,805 |
| 2014/2015 | 4,135 | 2024/2025 | 5,818 |
| 2015/2016 | 4,107 | 2025/2026 | 5,755 |
| 2016/2017 | 4,098 | 2026/2027 | 121 |
| 2017/2018 | 4,431 | | |

- 79,452 games played, plus 328 rows without a score: 325 cancelled, 2 postponed, 1 awarded.
- NCAA Tournament (Stage `Play Offs`): 1,068 games over 16 tournaments, 2010 to 2026. That makes 67 games a year from 2011 (First Four included), and 63 of the 64 games in 2010 (one first-weekend game is missing at the source). One of the 1,068 is Oregon – VCU Rams (2021), a no contest marked `Cancelled`. There was no tournament in 2020: it was cancelled.
- Conference tournaments are included, under Stage `Regular Season`, as the source lists them. The other postseason tournaments (NIT, CBI, CIT) are not, apart from one CIT game (see Known issues).
- 2008/2009 runs from 20 November 2008 to 7 March 2009 only. It stops in the first days of the conference tournaments, so that season has no NCAA Tournament.
- 2026/2027 holds the first 121 games of the season (1–3 November 2026, Paris dates).
- Coverage follows the source.

## Corrections to the source

- Oregon – VCU Rams (21 March 2021) was never played: it was declared a no contest. It is marked `Cancelled`.
- The source wrote two games twice on the same day. Each is now listed once: the 18:00 copy is removed. The two games are Oregon State – Washington (7 March 2010) and Miami (Ohio) – Ohio (3 March 2012).
- For 98 games, the source's two prices are inconsistent: their implied probabilities add up to less than 100%, or to exactly 100% for Columbia – Harvard (1 February 2025). An average of bookmakers' prices should keep a margin. Their odds cells are left empty. No row was removed for this reason.

## Known issues in the source

These rows are kept as the source shows them.

- Postponed games: the source marks a postponed game as `Cancelled` on its original date, and lists it again on the day it is played. 20 rows marked `Cancelled` are followed, within 7 days, by a played game between the same two teams with the same home team. 10 of them are in 2020/2021, where some may instead be a cancelled game of a back-to-back series.
- One impossible score: Tennessee State 10 – 11 Jacksonville State (3 February 2010).
- Two schedule clashes, where at least one of the two rows is wrong: Loyola Marymount plays Dartmouth and Rider, both at 18:00 on 21 November 2010, and South Dakota St. plays IU Indy and Western Illinois one hour apart on 16 January 2011.
- 3 games of December 2010 and January 2011 list a team named Miami Heat, the NBA team's name. Their opponents (Rice, Akron, Duke) point to Miami (FL).
- One game of the CIT postseason tournament is listed under `Regular Season`: New Hampshire Wildcats – NJIT (17 March 2015).
- 3 games keep two prices whose implied probabilities add up to exactly 100%, with no margin: Stanford – Arizona (20 January 2018), Oklahoma State – TCU (4 February 2023) and UTRGV – Southern Utah (19 December 2024).

## Format

- CSV, UTF-8 with BOM, comma-separated, one row per game as listed by the source (see Known issues for postponed games).
- `Date` is ISO 8601 (`YYYY-MM-DD`). `Date` and `Time` are in Central European Time (Europe/Paris, daylight saving included), so evening games in North America usually fall on the next calendar day.
- `Time` is empty for 6 games of 3 November 2026: no kick-off time is available for them.
- Odds are in American (moneyline) format and settle on the final result, overtime included.
- An empty odds cell means one of two things. Either the source shows no price for that side, or the source's prices were inconsistent (see Corrections to the source). Of the 79,452 played games, 76,320 have both prices, 379 have a price for one side only and 2,753 have none.
- The first team is the home team, as listed by the source. The file has no neutral-site flag. NCAA Tournament games, and some early-season events, are played on neutral courts.
- Use Date, Time and both teams to identify a game. 148 pairs of rows share the same date and the same two teams, with different tip-off times, scores and odds. 147 of them are back-to-back conference series of 2020/2021: the same teams played on consecutive days, and the first game, played in the US evening, falls after midnight in Paris. The last pair is Central Michigan – Western Michigan on 3 March 2018 (Paris date): one listing is cancelled, the other is the game played.

## Columns (identical to the 8 team-sport files of the US Sports Master)

| Column | Description |
|---|---|
| League | `NCAAB` |
| Season | `2025/2026` (the season spans two calendar years) |
| Stage | `Regular Season` (conference tournaments included) or `Play Offs` (NCAA Tournament) |
| Date, Time | Tip-off, Central European Time |
| Home_Team, Away_Team | Team names as written by the source: 686 names, including small-college opponents that appear in only a few games |
| Home_Score, Away_Score | Final score, overtime included (empty for cancelled, postponed or awarded games) |
| Winner | `Home` or `Away`, from the final score; empty when there is no score |
| Total_Score | Home_Score + Away_Score |
| Margin | Absolute score difference |
| Match_Ending | `Regular Time`, `Overtime`, `Cancelled`, `Postponed`, `Awarded` |
| Final_Result_Only | `Yes` or `No`, as flagged by the source. Filled up to 23 March 2026, empty for later games: the source no longer shows the flag |
| Home_Odds, Draw_Odds, Away_Odds | Closing odds. `Draw_Odds` is always empty in NCAAB (two-way market); it is kept so that the 8 team-sport files share the same columns |
| Bookmakers | Number of bookmakers in the source's average (1 to 9), filled for 2008/2009 – 2018/2019; empty from the 2019/2020 season. The source also gives this count for 2,137 games of those seasons that have no price or a price for one side only |

## Licence

Sample: free to explore and evaluate.
Full product: single commercial user. Use the data as you wish, including the models, predictions and products you sell.
Redistribution and resale of the files, or of the raw data in another format, are not permitted.

A question before buying: contact@oliviersportsdata.com

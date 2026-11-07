# 🏀 NCAAB March Madness — Historical Closing Odds (2008–2026)

**27 games — both ends of the file, and the road to the 2026 title, Elite Eight to the final**

The full dataset holds **79,780 NCAA men's basketball games, from 20 November 2008 to 3 November 2026**: the 16 NCAA
Tournaments from 2010 to 2026 (none in 2020), First Four to the final, the conference tournaments, and the closing
moneyline of both teams for 76,320 games, with the final score and how each game ended.

→ **[Get the full archive](https://sportsdataolivier.gumroad.com/l/chbrtv?utm_source=github&utm_medium=readme&utm_campaign=ncaab&utm_content=hero)** — 79,780 games, 19 seasons, 16 NCAA Tournaments · also on [Whop](https://whop.com/oliviersportsdata/ncaab-march-madness-historical-closing-odds-2008-2026/?utm_source=github&utm_medium=readme&utm_campaign=ncaab&utm_content=whop_hero)

This sample is cut from the full file: the same 18 columns, the rows copied byte for byte.

Source: OddsPortal. The odds are the average closing prices shown by the source, not attributable to any single bookmaker.

---

## The road to the 2026 title

The last 7 games of the 2026 NCAA Tournament, as they appear in the sample. UConn was the underdog in all three of its
games shown here, after being favoured in its first three rounds: it won at +175 against Duke and at +102 against
Illinois, then lost the final at +229.

| Date, time (Paris) | Round | Teams, as listed | Final | Closing moneyline |
|---|---|---|---|---|
| 28 Mar 2026, 23:09 | Elite Eight | Illinois – Iowa | 71–59 | -357 / +261 |
| 29 Mar 2026, 01:49 | Elite Eight | Arizona – Purdue | 79–64 | -278 / +206 |
| 29 Mar 2026, 20:15 | Elite Eight | Michigan – Tennessee | 95–62 | -455 / +308 |
| 29 Mar 2026, 23:05 | Elite Eight | Duke – UConn | 72–73 | -233 / +175 |
| 5 Apr 2026, 00:09 | Final Four | UConn – Illinois | 71–62 | +102 / -135 |
| 5 Apr 2026, 02:54 | Final Four | Arizona – Michigan | 73–91 | -118 / -112 |
| 7 Apr 2026, 02:50 | Final | Michigan – UConn | 69–63 | -312 / +229 |

Tournament games are played on neutral courts: the first team is the one the source lists first. Times are Central
European Time, so the final, played on the evening of 6 April in the US, is dated 7 April.

---

## What's in this sample

| | |
|---|---|
| Rows | 27 — the first 10 rows of the file, the last 7 games of the 2026 NCAA Tournament, the last 10 rows |
| Seasons | 2008/09 (10 rows) · 2025/26 (7) · 2026/27 (10) |
| Stages | 20 `Regular Season` · 7 `Play Offs` (NCAA Tournament) |
| Overtime | 3 games |
| Prices | 41 — both prices for 20 games, one side for 1, none for 6 |
| Columns | 18 |
| Files | `sample/NCAAB_Closing_Odds_SAMPLE.csv`, plus `sample/README.md`: how the sample is built, then the README of the full product |

The first rows show what the README describes: on 20 November 2008, 6 games carry a bookmaker count but no price.
The last rows are from 3 November 2026: 6 of them have no tip-off time, and `Bookmakers` and `Final_Result_Only`
are empty, as the source no longer shows them.

---

## In this repository

| File | Content |
|---|---|
| `sample/NCAAB_Closing_Odds_SAMPLE.csv` | the 27 games, 18 columns |
| `sample/README.md` | how the sample is built, then the README of the full product |
| `NCAAB_Verify_Before_You_Buy.ipynb` | the figures of the product page, the games season by season and integrity checks, recomputed from the full file, results shown |

---

## Columns

| Column | Content |
|---|---|
| `League` | `NCAAB` |
| `Season` | `2025/2026`: the season spans two calendar years |
| `Stage` | `Regular Season` (conference tournaments included) or `Play Offs` (NCAA Tournament) |
| `Date`, `Time` | tip-off, Central European Time (Europe/Paris) |
| `Home_Team`, `Away_Team` | team names as written by the source; the first team listed is the home team |
| `Home_Score`, `Away_Score` | final score, overtime included |
| `Winner`, `Total_Score`, `Margin` | from the final score |
| `Match_Ending` | `Regular Time`, `Overtime`, `Cancelled`, `Postponed`, `Awarded` |
| `Final_Result_Only` | `Yes` or `No`, as flagged by the source, up to 23 March 2026 |
| `Home_Odds`, `Draw_Odds`, `Away_Odds` | closing moneyline; `Draw_Odds` is always empty in NCAAB (two-way market) |
| `Bookmakers` | number of bookmakers in the source's average (1 to 9), up to 2018/19 |

The same 18 columns as the 7 other team-sport files of the US Sports Master.

### How to read it

**Prices are American odds, as integers:** `-250`, and `200` for +200. **They settle on the final result, overtime
included.** **`Date` and `Time` are Central European Time**, so an evening game in the US is usually dated the next
day. **An empty odds cell** means the source shows no price for that side, or its two prices were inconsistent (see
the README).

```python
import pandas as pd

df = pd.read_csv("sample/NCAAB_Closing_Odds_SAMPLE.csv")

def prob(us):  # American odds -> implied probability
    return -us / (-us + 100) if us < 0 else 100 / (us + 100)

games = df.dropna(subset=["Home_Odds", "Away_Odds"]).copy()
games["p_home"] = games["Home_Odds"].map(prob)
games["p_away"] = games["Away_Odds"].map(prob)
games["margin"] = games["p_home"] + games["p_away"] - 1   # margin kept in the average price
games["favourite"] = (games["p_home"] > games["p_away"]).map({True: "Home", False: "Away"})
games["favourite_won"] = games["favourite"] == games["Winner"]
```

---

## Said plainly

- **Closing odds only.** No opening odds, no line movement, no spreads, no totals, no bookmaker-by-bookmaker prices.
- **No neutral-site flag.** NCAA Tournament games and some early-season events are played on neutral courts.
- **2008/09 is partial**: 774 games, none from 28 November to 18 December 2008, and it stops on 7 March 2009, in the
  first days of the conference tournaments, so that season has no NCAA Tournament. The 2010 tournament has 63 of its 64 games, and there was no tournament in 2020.
- **No NIT or CBI games.** One CIT game of 2015 is in the file, under `Regular Season`, listed in the README.
- **Some games have no price for one or both sides: 3,132 of the 79,452 games played.** They include 98 games whose
  two prices at the source were inconsistent, so those cells are left empty.
- **The number of bookmakers in the average is given up to 2018/19 only**, and the source's "final result only" flag
  up to 23 March 2026.
- **Team names are as written by the source**: 686 names, including small-college opponents that appear in only a
  few games.
- **A few source errors are kept as the source shows them and listed in the README**, such as postponed games also
  shown as cancelled on their first date.

---

## Verify before you buy

The figures of the product page, the games season by season and integrity checks on every row, recomputed from the
full file, with its SHA-256 fingerprint:
[on Kaggle](https://www.kaggle.com/code/oliviersportsdata/ncaab-v2-verify-before-you-buy) · [here, in `NCAAB_Verify_Before_You_Buy.ipynb`](NCAAB_Verify_Before_You_Buy.ipynb) (results shown).
The same sample is on [Kaggle](https://www.kaggle.com/datasets/oliviersportsdata/datasets-road-to-march-madness)
and [Hugging Face](https://huggingface.co/datasets/oliviersportsdata/NCAAB-March-Madness-Historical-Closing-Odds).

---

## The full dataset

| | Sample | Full |
|---|---|---|
| Rows | 27 | 79,780 |
| Games played | 27 | 79,452 |
| Seasons | 2008/09, 2025/26, 2026/27 | 2008/09 to 2026/27, through 3 Nov 2026 |
| NCAA Tournament games | 7 | 1,068, from 2010 to 2026 |
| Prices | 41 | 153,424, of which 405 on cancelled, postponed or awarded games |
| Columns | 18 | 18 |
| Files | 1 CSV + README | 1 CSV + README |

The 2026/27 season goes on in the
[US Sports Master monthly updates](https://sportsdataolivier.gumroad.com/l/mudppx?utm_source=github&utm_medium=readme&utm_campaign=ncaab&utm_content=monthly):
the previous month's games of the 9 US leagues, NCAAB included, in the same columns. All 9 US leagues in one ZIP,
NCAAB included: the
[US Sports Master](https://sportsdataolivier.gumroad.com/l/ahlplc?utm_source=github&utm_medium=readme&utm_campaign=ncaab&utm_content=us_sports_master).

→ **[Get the full archive](https://sportsdataolivier.gumroad.com/l/chbrtv?utm_source=github&utm_medium=readme&utm_campaign=ncaab&utm_content=footer)** · also on [Whop](https://whop.com/oliviersportsdata/ncaab-march-madness-historical-closing-odds-2008-2026/?utm_source=github&utm_medium=readme&utm_campaign=ncaab&utm_content=whop_footer)

---

## Licence

Sample: free to explore and evaluate.
Full product: single commercial user. Use the data as you wish, including the models, predictions and products you sell.
Redistribution and resale of the files, or of the raw data in another format, are not permitted.

A question before buying: contact@oliviersportsdata.com

---

*Olivier Sports Data — From Chicken Area 🐔*

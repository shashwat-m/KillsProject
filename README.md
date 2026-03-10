# Kills in the NBA: Testing Sean Miller's Defensive Theory

> *Does the college basketball concept of "kills" translate to the most offense-dominated league in the world?*

---

## Background

At a Texas Longhorns season opener, head coach **Sean Miller** introduced a metric I'd never heard before: **kills** — defined as **3 consecutive defensive stops**, whether by forced turnover, missed shot + defensive rebound, or offensive foul.

Miller's claim: reach **7+ kills in a game, and your win probability jumps to ~90%.**

This project takes that theory and stress-tests it against NBA data. The NBA runs at a completely different pace than college ball, with star-studded offenses and elite shot creation that makes sustained defensive runs much harder to come by. Does the kills concept still hold up?

---

## What is a Kill?

A **kill** is awarded every time a team records **3 defensive stops in a row**. A defensive stop is defined as:

- A forced **turnover** (steal, offensive foul, violation), or
- A **missed shot** immediately followed by a **defensive rebound**

Every 3rd consecutive stop resets the counter and adds one kill. A team can record multiple kills per game.

---

## Project Structure

```
kills-in-the-nba/
│
├── kills.Rmd   # Full analysis — narrative + code + visualizations
├── kills.pdf           # Standalone PDF
└── README.md
```

---

## Analysis Overview

**1. Scoping on the Dallas Mavericks (2024-25)**
A smaller-scale proof of concept to validate the kill detection logic on a single team before scaling to the full league.

**2. Full League Analysis (2024-25)**
Kills computed for all 30 NBA teams across the full regular season, including:
- Distribution of kills per game league-wide
- Win rate by kill count and kill bucket
- Direct test of Miller's 7+ kill threshold
- Kills differential (your kills minus opponent kills)

**3. Logistic Regression Models**
Three models tested:
- Raw kills → win probability
- Kills differential → win probability
- Mixed effects model controlling for team-level defensive talent

**4. Team Leaderboard**
Season averages for kills, opponent kills, and differential by team, with a scatter of avg kills vs. win %.

**5. Multi-Season Analysis (2020–2025)**
Win rate by kills faceted across five seasons — testing whether the relationship is stable or season-specific.

---

## Key Questions

- Does 7+ kills actually predict ~90% win probability in the NBA, as Miller claims in college?
- Is the **kills differential** (out-killing your opponent) a stronger signal than raw kill volume?
- Are kills driven by team identity (elite defenses win more *and* get more kills), or do they carry independent predictive value after controlling for team quality?
- How has the kills → wins relationship evolved as the NBA has shifted toward pace-and-space basketball?

---

## Why 2015 Onward Matters

The 2015-16 season marks a widely recognized inflection point in NBA history — the Golden State Warriors' championship cemented the **3-point revolution** as the dominant offensive paradigm. Since then, the league has seen record-breaking offensive ratings year over year, making it an increasingly hostile environment for defensive metrics to show signal.

Extending the analysis back to 2015 lets us test whether kills become *more* predictive in lower-pace, lower-efficiency eras, or whether the relationship holds regardless of the offensive environment.

---

## Planned: Era Comparisons

One of the most interesting extensions of this work is comparing kills across fundamentally different eras of NBA basketball:

| Era | Years | Defensive Character |
|-----|-------|-------------------|
| Bad Boy Pistons era | Late 1980s–early 1990s | Physical, hand-check rules, low pace |
| Jordan-era defense | Early–mid 1990s | Isolation defense, low scoring |
| Post-hand-check boom | 2005–2010 | Rule changes open up offense |
| Analytics / pace-and-space | 2015–present | 3-point explosion, record offensive ratings |

The central question: **in eras where defense dominated, did kills occur more frequently and predict wins more strongly?** Or is the kills → wins relationship stable across contexts, suggesting it captures something fundamental about momentum and defensive execution regardless of era?

> *Note: play-by-play data availability in hoopR limits how far back the analysis can go. Historical era analysis may require supplemental data sources.*

---

## Dependencies

All analysis is done in **R**. Install the required packages with:

```r
install.packages(c("dplyr", "ggplot2", "ggthemes", "tidyr", "lme4", "scales", "purrr"))

# hoopR for NBA play-by-play and box score data
install.packages("hoopR")
# or dev version:
# remotes::install_github("sportsdataverse/hoopR")
```

---

## How to Run

1. Clone the repo and open `nba_kills_analysis.Rmd` in RStudio.
2. Knit the document — the first run will take time due to play-by-play data loading (~5–10 min per season).
3. Chunks marked `cache=TRUE` will cache results so subsequent knits are fast.

> **Note on multi-season runs:** The script loads one season at a time inside the loop to manage memory. For the full 2015–2025 range, expect the first run to take a while — consider running it overnight.

---

## Data

All data is sourced via the [`hoopR`](https://hoopr.sportsdataverse.org/) R package, which pulls from ESPN's play-by-play feed. No external files required — everything is fetched at runtime.

| Data | Source | Function |
|------|--------|----------|
| Play-by-play | ESPN via hoopR | `load_nba_pbp()` |
| Team box scores | ESPN via hoopR | `load_nba_team_box()` |

---

## Status

🟢 **Active development**

- [x] Kill detection logic validated (Dallas Mavericks, 2024-25)
- [x] Full league analysis for 2024-25
- [x] Logistic regression + mixed effects models
- [x] Team-level leaderboard
- [x] Multi-season analysis (2020–2025)
- [ ] Extended historical analysis (2015–2025)
- [ ] Era comparison analysis
- [ ] Kills vs. Defensive Rating (DRTG) head-to-head
- [ ] Correlation with traditional defensive metrics (BPG, SPG, opponent FG%)

---

## Roadmap

**Next: Extending to 2015–2025**
The 3-point era provides the most relevant modern context for testing kills. Expanding the window to a decade gives much larger sample sizes and captures the full arc of pace-and-space basketball.

**Era Analysis**
Comparing kill frequency and win correlation across the Bad Boy Pistons era, the hand-check rule changes (2004-05), and the modern 3-point explosion — looking for whether defensive dominance amplifies or diminishes the signal.

**Kills vs. DRTG**
A head-to-head comparison of kills against Defensive Rating to understand whether kills capture something DRTG misses, or whether they're largely redundant with existing metrics.

**Defensive Stat Correlations**
How do kills relate to steals, blocks, and opponent FG%? Is a high-kill team necessarily a high-steal team, or does the sequential nature of kills capture a different dimension of defensive execution?

---

## Acknowledgements

- **Sean Miller** and the Texas Longhorns for introducing this concept
- [`hoopR`](https://hoopr.sportsdataverse.org/) by Saiem Gilani and the SportsDataverse team for making NBA play-by-play data accessible in R
- The sports analytics community whose work on defensive metrics provided the foundation for this project

---

## Author

**Shashwat Mishra**  
Feel free to open an issue or reach out with questions, ideas, or bugs — especially around the kill detection logic, which is the foundation everything else is built on.

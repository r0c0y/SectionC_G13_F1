# [F1 Constructor Sponsorship & Investment ROI Analytics](https://public.tableau.com/app/profile/kushagra.maheshwari4048/vizzes)
## DVA Capstone 2 - Project Repository

> **Data Visualization & Analytics**
> A data-driven industry simulation using Python and Tableau to identify high-value sponsorship opportunities in Formula 1's Modern Hybrid Era.

---

## Project Overview

| Field | Details |
|---|---|
| **Project Title** | F1 Constructor Sponsorship & Investment ROI Analytics |
| **Sector** | Sports Analytics & Corporate Sponsorship Intelligence |
| **Team ID** | Section C — Group G13 |
| **Section** | Section C |
| **Faculty Mentor** | Archit Raj |
| **Institute** | Newton School of Technology |
| **Submission Date** | April 29, 2026 |

### Team Members

| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Priyanshu Tomar | [`r0c0y`](https://github.com/r0c0y) |
| Data Lead | Anurag Kumar | [`Anuragkumar-687`](https://github.com/Anuragkumar-687) |
| ETL Lead | Priyanshu Tomar | [`r0c0y`](https://github.com/r0c0y) |
| Analysis Lead | Thejas | [`Thejas663`](https://github.com/Thejas663) |
| Visualization Lead | Kushagra Maheshwari | [`kush11-m`](https://github.com/kush11-m) |
| Strategy Lead | Piyush | [`PiyushY111`](https://github.com/PiyushY111) |
| PPT and Quality Lead | Aditya Srivastava | [`aditya-1509`](https://github.com/aditya-1509) |

---

## Business Problem

Formula 1 represents a $2.6 billion annual sponsorship market where brands seek global visibility and association with engineering excellence. However, not every team offers the same value; some are rising, while others are declining. This project addresses the lack of a structured, data-driven framework for sponsors to compare teams on expected return, preventing the misallocation of marketing budgets based on reputation rather than performance evidence.

**Core Business Question**

> Which F1 constructor teams represent the highest Growth ROI and Consistency for corporate sponsorship in the post-2014 Modern Hybrid Era?

**Decision Supported**

> This analysis enables Corporate CMOs and Investment Managers to allocate multi-million dollar sponsorship budgets based on risk-adjusted ROI scores, identifying "undervalued gems" before their market value peaks.

---

## Dataset

| Attribute | Details |
|---|---|
| **Source Name** | Ergast Developer API : Formula 1 Historical Database |
| **Direct Access Link** | [http://ergast.com/mrd/](http://ergast.com/mrd/) |
| **Row Count** | 4,626 (Master Table) / 701,433 (Raw) |
| **Column Count** | 35 (Master Table) |
| **Time Period Covered** | 2014 to 2024 (Modern Hybrid Era) |
| **Format** | CSV |

**Key Columns Used**

| Column Name | Description | Role in Analysis |
|---|---|---|
| `points` | Championship points scored | Core KPI Input (Growth/Momentum) |
| `positionOrder` | Final finishing classification | Used for Result Volatility Index |
| `status` | Race finish status (Finished/DNF) | Reliability & Finish Rate Calculation |
| `milliseconds` | Pit stop duration in ms | Operational Excellence Metric |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| Constructor Points Momentum | Tracks if a team is rising or declining YoY | `(Points_Y2 - Points_Y1) / Points_Y1 * 100` |
| Result Volatility Index | Measures predictability and investment risk | `std(positionOrder)` per constructor per season |
| Finish Rate | Reliability metric for sponsor exposure guarantee | `sum(finished) / count(races)` |
| Position Delta Average | Measures race-day execution and race craft | `mean(grid - positionOrder)` |
| Composite Investment Score | Final ranking based on weighted dimensions | `0.3*P + 0.25*C + 0.2*M + 0.15*R + 0.1*RC` |

Document KPI logic clearly in `notebooks/04_statistical_analysis.ipynb` and `notebooks/05_final_load_prep.ipynb`.

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | [F1 Sponsorship Analytics Suite](https://public.tableau.com/app/profile/kushagra.maheshwari4048/vizzes) |
| **Executive View** | Constructor Investment Scorecard — ROI vs Consistency ranking |
| **Operational View** | Race Craft Analytics — Pit stop efficiency and DNF trends |
| **Main Filters** | Constructor, Season/Year, Investment Tier (Gold/Silver/Bronze) |

Store dashboard screenshots in [`tableau/screenshots/`](tableau/screenshots/) and document the public links in [`tableau/dashboard_links.md`](tableau/dashboard_links.md).

---

## Key Insights

1. **McLaren is the highest-value opportunity**: Currently the only "Gold" tier team with +128.95% YoY growth and perfect 2024 reliability.
2. **Red Bull dominance is weakening**: Momentum has turned negative (–93.5 pts/yr) despite historical dominance, signaling a "sponsorship trap."
3. **The grid is becoming more competitive**: Top team share of points fell to 25.05% in 2024, the lowest level in the Hybrid Era.
4. **Consistency predicts performance**: Lower result volatility (R² = 0.28, p < 0.001) is strongly linked to higher championship points.
5. **Operational discipline matters**: Faster pit stops (Cohen’s d = 1.26) are a statistically significant leading indicator of team quality.
6. **Mercedes' decline is structural**: A confirmed negative trend (–29.84 pts/yr) suggests the "Silver Arrows" era has genuinely ended.
7. **Sauber is a stable hedge**: Identified as the most predictable team on the grid (lowest volatility in 2024).
8. **Diversification beats concentration**: A multi-team portfolio provides higher risk-adjusted exposure than a single-team bet.

---

## Recommendations

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | McLaren Growth | Prioritise McLaren for premium partnerships | 2–3× higher TV coverage per dollar spent |
| 2 | Portfolio Alpha | Build a diversified F1 sponsorship portfolio | ~67% reduction in single-team exposure risk |
| 3 | Momentum Shift | Limit investment in Red Bull and Alpine | Avoid 20–30% "reputation premium" overpayment |

---

## Repository Structure

```text
SectionC_G13_F1/
|
|-- data/
|   |-- raw/                         # Original dataset (never edited)
|   `-- processed/                   # Cleaned output from ETL pipeline
|
|-- notebooks/
|   |-- 01_extraction.ipynb
|   |-- 02_cleaning.ipynb
|   |-- 03_eda.ipynb
|   |-- 04_statistical_analysis.ipynb
|   `-- 05_final_load_prep.ipynb
|
|-- scripts/
|   `-- etl_pipeline.py
|
|-- tableau/
|   |-- screenshots/
|   `-- dashboard_links.md
|
|-- reports/
|   |-- README.md
|   |-- Detailed_report_F1.pdf
|   `-- concise_report.pdf
|
|-- docs/
|   `-- data_dictionary.md
|
|-- DVA-oriented-Resume/
`-- DVA-focused-Portfolio/
```

---

## Analytical Pipeline

The project follows a structured 7-step workflow:

1. **Define** - Sector selected, problem statement scoped, mentor approval obtained.
2. **Extract** - Raw dataset sourced and committed to `data/raw/`; data dictionary drafted.
3. **Clean and Transform** - Cleaning pipeline built in `notebooks/02_cleaning.ipynb`.
4. **Analyze** - EDA and statistical analysis performed in notebooks `03` and `04`.
5. **Visualize** - Interactive Tableau dashboard built and published on Tableau Public.
6. **Recommend** - 3-5 data-backed business recommendations delivered.
7. **Report** - Final project report and presentation deck completed and exported to PDF in `reports/`.

---

## Tech Stack

| Tool | Status | Purpose |
|---|---|---|
| Python + Jupyter Notebooks | Mandatory | ETL, cleaning, analysis, and KPI computation |
| Tableau Public | Mandatory | Dashboard design, publishing, and sharing |
| GitHub | Mandatory | Version control and team collaboration |

**Recommended Python libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`

---

## Submission Checklist

**GitHub Repository**

- [x] Public repository created with the correct naming convention (`SectionName_TeamID_ProjectName`)
- [x] All notebooks committed in `.ipynb` format
- [x] `data/raw/` contains the original, unedited dataset
- [x] `data/processed/` contains the cleaned pipeline output
- [x] `tableau/screenshots/` contains dashboard screenshots
- [x] `tableau/dashboard_links.md` contains the Tableau Public URL
- [x] `docs/data_dictionary.md` is complete
- [x] `README.md` explains the project, dataset, and team
- [x] All members have visible commits and pull requests

**Tableau Dashboard**

- [x] Published on Tableau Public and accessible via [Public URL](https://public.tableau.com/app/profile/kushagra.maheshwari4048/vizzes)
- [x] At least one interactive filter included
- [x] Dashboard directly addresses the business problem

**Project Report**

- [x] Final report exported as PDF: [Detailed Report](reports/Detailed_report_F1.pdf) | [Concise Report](reports/concise_report.pdf)
- [x] Cover page, executive summary, sector context, problem statement
- [x] Data description, cleaning methodology, KPI framework
- [x] EDA with written insights, statistical analysis results
- [x] Dashboard screenshots and explanation
- [x] 8-12 key insights in decision language
- [x] 3-5 actionable recommendations with impact estimates
- [x] Contribution matrix matches GitHub history

**Presentation Deck**

- [x] Final presentation exported as PDF: [Presentation Deck](reports/f1-constructor-sponsorship-and-investment-roi-analytics.pdf)
- [x] Title slide through recommendations, impact, limitations, and next steps

**Individual Assets**

- [x] DVA-oriented resume updated: [Resume Folder](DVA-oriented-Resume/)
- [x] Portfolio link or project case study added: [Portfolio Folder](DVA-focused-Portfolio/)

---

## Contribution Matrix

This table matches evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset and Sourcing | ETL and Cleaning | EDA and Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT and Viva |
|---|---|---|---|---|---|---|---|
| [**Priyanshu Tomar**](https://github.com/r0c0y) | Core | Core | Assist | Assist | Assist | Core | Core |
| [**Piyush**](https://github.com/PiyushY111) | Assist | Assist | Assist | Core | Assist | Assist | Core |
| [**Kushagra Maheshwari**](https://github.com/kush11-m) | Assist | Assist | Core | Assist | Core | Assist | Assist |
| [**Thejas**](https://github.com/Thejas663) | Assist | Core | Assist | Core | Assist | Assist | Assist |
| [**Anurag Kumar**](https://github.com/Anuragkumar-687) | Core | Assist | Assist | Assist | Assist | Core | Assist |
| [**Aditya Srivastava**](https://github.com/aditya-1509) | Assist | Assist | Assist | Assist | Core | Assist | Core |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead Name:** Priyanshu Tomar

**Date:** April 29, 2026

---

## Academic Integrity

All analysis, code, and recommendations in this repository must be the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history.

*Data Visualization & Analytics | Capstone 2*

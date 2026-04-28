# Data Dictionary

**Project:** F1 Constructor Sponsorship & Investment ROI Analytics  
**Dataset Source:** [Ergast Developer API : Formula 1 Historical Database](http://ergast.com/mrd/)  
**Era of Focus:** 2014–2024 (1.6L V6 Turbo Hybrid Regulations)  
**Last Updated:** April 2026

---

## Dataset Overview

| Metric | Value |
|:---|:---|
| Raw Files | 14 CSV files |
| Raw Total Rows | 701,433 |
| Raw Total Columns | 120 (across all files) |
| Filtered Rows (2014+) | ~440,000+ |
| Master Table (Post-Join) | 4,626 rows × 35 columns |
| Active Constructors | 20 teams |
| Active Drivers | 59 drivers |
| Seasons Covered | 2014–2024 (11 seasons) |

---

## Primary Tables Used

### 1. `results.csv` : Race Results (Core Table)
One row per driver per race. The foundation of all KPI calculations.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| resultId | int | Unique result identifier | 0% | Primary key |
| raceId | int | Foreign key to races | 0% | Links to race metadata |
| driverId | int | Foreign key to drivers | 0% | Links to driver info |
| constructorId | int | Foreign key to constructors | 0% | **Our main grouping key** |
| grid | int | Starting grid position | 0% | 0 = pit lane start |
| positionOrder | int | Final classification order | 0% | Used instead of `position` |
| points | float | Championship points scored | 0% | Core KPI input |
| laps | int | Number of laps completed | 0% | — |
| statusId | int | Foreign key to status table | 0% | Finished / DNF reason |

> **Dropped columns:** `position` (40.9% null), `time`/`milliseconds` (71.3% null), `fastestLap*` (69% null), `number`, `positionText`

### 2. `races.csv` : Race Metadata
One row per Grand Prix. Used as the bridge table via `raceId`.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| raceId | int | Unique race identifier | 0% | Primary key |
| year | int | Season year | 0% | **Filter: year >= 2014** |
| round | int | Race number within season | 0% | — |
| circuitId | int | Foreign key to circuits | 0% | Links to circuit location |
| name | string | Grand Prix name | 0% | e.g., "Australian Grand Prix" |
| date | string | Race date (YYYY-MM-DD) | 0% | Converted to datetime |

> **Dropped columns:** `fp1/fp2/fp3_date/time` (>92% null), `sprint_date/time` (>98% null)

### 3. `constructors.csv` : Team Metadata
One row per F1 constructor/team. This is our "company" lookup table.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| constructorId | int | Unique team identifier | 0% | Primary key |
| name | string | Official team name | 0% | e.g., "Red Bull", "McLaren" |
| nationality | string | Team nationality | 0% | 24 unique values |

### 4. `constructor_standings.csv` : Championship Standings
Cumulative championship standings after each race. We extract **year-end standings** for the YoY Growth KPI.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| raceId | int | Foreign key to races | 0% | Joined with races to get year |
| constructorId | int | Foreign key to constructors | 0% | — |
| points | float | Cumulative points in season | 0% | Year-end value = final points |
| position | int | Championship position | 0% | Year-end value = final standing |
| wins | int | Cumulative race wins | 0% | — |

### 5. `status.csv` : Race Status Lookup
Maps statusId to human-readable finish status.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| statusId | int | Unique status identifier | 0% | Primary key |
| status | string | Status description | 0% | e.g., "Finished", "Accident", "Engine" |

### 6. `pit_stops.csv` : Pit Stop Records
One row per pit stop event. Different granularity from the main results table.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| raceId | int | Foreign key to races | 0% | — |
| driverId | int | Foreign key to drivers | 0% | — |
| stop | int | Stop number (1st, 2nd, etc.) | 0% | — |
| lap | int | Lap number of the stop | 0% | — |
| duration | string | Duration in mm:ss.sss | 0% | Converted to seconds via milliseconds |
| milliseconds | int | Duration in milliseconds | 0% | Primary numeric field |

### 7. `circuits.csv` : Circuit Metadata
Used for geographic analysis in Tableau.

| Column | Type | Description | Nulls | Notes |
|:---|:---|:---|:---|:---|
| circuitId | int | Unique circuit identifier | 0% | Primary key |
| name | string | Circuit name | 0% | — |
| location | string | City | 0% | — |
| country | string | Country | 0% | 35 unique countries |
| lat / lng | float | GPS coordinates | 0% | For map visualisation |

---

## Derived Columns (Created During Cleaning)

| Column | Type | Formula | Purpose |
|:---|:---|:---|:---|
| position_delta | int | `grid - positionOrder` | Measures race-day position gains/losses |
| finished | int (0/1) | `1 if status contains 'Finished'` | Binary reliability flag |
| driver_name | string | `forename + ' ' + surname` | Human-readable name |
| duration_seconds | float | `milliseconds / 1000` | Pit stop duration in seconds |
| is_normal_stop | int (0/1) | `1 if duration < 60s` | Filters out red flag / penalty stops |

---

## KPI Definitions

| KPI | Formula | Business Meaning |
|:---|:---|:---|
| **Constructor Points Momentum (YoY %)** | `(Points_Y2 - Points_Y1) / Points_Y1 * 100` | Is the team rising or declining? |
| **Result Volatility Index** | `std(positionOrder)` per constructor per season | How consistent/predictable is the team? |
| **Finish Rate** | `sum(finished) / count(races)` per constructor | Reliability metric for investor confidence |
| **Position Delta Average** | `mean(position_delta)` per constructor | Does the team gain or lose positions on race day? |

---

## Data Quality Notes
- `\N` values in raw CSVs are parsed as NaN during extraction.
- The `position` column in results is 40.9% null (all DNFs). We use `positionOrder` instead.
- Practice/qualifying session timestamps are >90% null globally but improve within the 2014+ filter.
- Pit stop durations >60 seconds are flagged as abnormal (red flags, penalties).
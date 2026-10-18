# Second_Major_Project_Veda_Technology
# 🦠 COVID-19 Global Impact Analysis

---

##  Project Overview

Raw COVID-19 counts are hard to compare across countries: populations differ, reporting is inconsistent, and daily numbers are noisy. This project walks through the full analytics pipeline to turn that data into clear, fair comparisons:

1. **Clean** messy time-series data and document every assumption
2. **Derive** new metrics: 7-day rolling averages, case fatality rate (CFR) and doubling time
3. **Normalize** by population (per million) so countries are fairly comparable
4. **Visualize** waves, vaccination rollouts and country differences
5. **Investigate** a focused question instead of building a generic dashboard

---

##  Key Findings

| # | Insight |
|---|---------|
| 1 | **Per-capita ranking changes the story.** Peru (6,601 deaths per million), Bulgaria (5,670) and North Macedonia (5,422) were the hardest hit. |
| 2 | **Cases and deaths decoupled after 2021.** Omicron-era case rates were several times higher than earlier peaks, yet daily deaths peaked in 2020-21. |
| 3 | **CFR varied widely** among the six countries compared, from 0.46% (Germany) to 2.52% (South Africa), largely reflecting testing, age structure and healthcare capacity. |
| 4 | **Vaccination vs deaths: a positive correlation** (Spearman ρ = 0.63, 154 countries). This is driven by confounding (age, wealth, reporting quality), **not** by vaccines causing deaths. |
| 5 | **Simple cross-country comparisons can mislead.** Fair conclusions need adjustment for age, healthcare capacity and reporting differences. |

> 💡 The unexpected positive correlation is explained honestly in the report: early-vaccinating countries tend to be older, wealthier and better at reporting deaths, while many low-vaccination countries have young populations and limited testing.

---

##  Dataset

- **Source:** [Our World in Data (OWID) COVID-19 dataset](https://github.com/owid/covid-19-data)
- **Coverage:** 1 January 2020 to 14 August 2024
- **File used:** `covid-dataset-small.csv` (~22 MB, 15 columns)
- **After cleaning:** 266,897 daily records across 159 countries

| Column group | Columns |
|---|---|
| Identifiers | `iso_code`, `continent`, `location`, `date`, `population` |
| Cases and deaths | `total_cases`, `new_cases`, `total_deaths`, `new_deaths` |
| Per million | `total_cases_per_million`, `new_cases_per_million`, `total_deaths_per_million`, `new_deaths_per_million` |
| Vaccination | `total_vaccinations`, `people_fully_vaccinated_per_hundred` |

---

##  Methodology

### Data cleaning (documented assumptions)

| # | Issue | Action |
|---|-------|--------|
| A1 | Aggregate rows (World, continents, income groups) | Removed (`iso_code` starting with `OWID_`) to avoid double-counting |
| A2 | Very small countries | Kept only countries with population ≥ 1 million, since per-million rates are unstable for tiny populations |
| A3 | Negative daily values | Set to 0 (retrospective data corrections) |
| A4 | Missing daily values | Filled with 0 (assumed no report that day) |
| A5 | Missing cumulative values | Forward-filled within each country; leading gaps set to 0 |

### Derived metrics

| Metric | Formula |
|---|---|
| 7-day rolling average | Mean of the last 7 days of new cases / deaths per million |
| Case fatality rate (%) | `total_deaths / total_cases × 100` (only after 100 cases) |
| Doubling time (days) | `7 × ln(2) / ln(cases_today / cases_7_days_ago)` |

### Focused analysis

Each country's **% fully vaccinated on 31 Dec 2021** was compared with its **total deaths per million in 2022** using Spearman rank correlation (154 countries).

---

## 📈 Visualizations

| New cases per million (7-day avg) | New deaths per million (7-day avg) |
|:---:|:---:|
| ![Cases](charts/chart1_cases.png) | ![Deaths](charts/chart2_deaths.png) |

| People fully vaccinated (%) | Vaccination vs deaths (2022) |
|:---:|:---:|
| ![Vaccination](charts/chart3_vaccination.png) | ![Scatter](charts/chart4_vax_vs_deaths.png) |

---

## 📁 Repository Structure

```
covid19-global-impact-analysis/
│
├── covid_analysis.ipynb              # Full analysis notebook
├── covid-dataset-small.csv           # Dataset (OWID, trimmed to 15 columns)
├── COVID19_Global_Impact_Report.pdf  # 5-page executive report
├── charts/                           # Exported charts
│   ├── chart1_cases.png
│   ├── chart2_deaths.png
│   ├── chart3_vaccination.png
│   └── chart4_vax_vs_deaths.png
└── README.md
```

---

---

##  Limitations

- **Correlation is not causation.** Age structure, healthcare capacity, testing and reporting quality are not controlled for.
- **Under-reporting.** Official case and death counts vary in completeness across countries and over time.
- **Single-year window.** The 2022 comparison excludes deaths in 2020-21.
- **Imputation.** Filling missing daily values with 0 may slightly understate activity in countries that reported irregularly.

---

##  Future Work

- Control for **median age** and other demographics, then repeat the correlation
- Compare deaths **before vs after** vaccine rollout within the same country
- Use **excess-mortality** data to reduce the effect of under-reporting
- Build an **interactive dashboard** (Power BI / Tableau) with country and date slicers

---

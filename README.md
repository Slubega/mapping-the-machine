# Mapping the Machine

### A Compensation, Tenure, and Structure Analysis of the District's Public Workforce

> **MATH 014 — Introduction to Data Science · Spring 2026 · Howard University**
> Sule Lubega

---

## Overview

The DC government employs more than 40,000 people across roughly 90 agencies — a workforce comparable in size to many Fortune 500 companies, funded entirely by public dollars. This project uses the DC Public Employee Salary dataset (DC Open Data) to ask three connected questions about how that workforce is compensated, structured, and stratified.

The goal is not to produce a single ranking but to **map the structural shape of public employment in DC** — to make a workforce of more than 35,000 people legible through a disciplined set of visualizations and summary statistics.

---

## Key Findings

| Metric | Value | Interpretation |
|--------|-------|----------------|
| Global median salary | **$100,313** | Mean ($101,544) ≈ median — fingerprint of a grade-capped pay system |
| DCPS workforce share | **28%** | Nearly 1 in 3 DC employees works in public schools |
| Tenure–pay correlation | **r = 0.293** | Modest pooled, but varies dramatically by appointment track |
| DDOT P90/P10 ratio | **5.8×** | Almost 3× the median agency — driven by a two-tier engineer/laborer mix |

**Headline conclusion:** DC's public workforce is more compressed and more education-driven than a casual look would suggest. Appointment category — not agency, not tenure — is the dominant axis of pay variation.

---

## Research Questions

**Q1 — Agency Comparison**
How does compensation vary across DC's agencies, and which agencies offer the highest median pay?

**Q2 — Tenure–Pay Slope**
Does employee tenure predict pay — and does that relationship behave differently across appointment categories (CS, Ed, MSS, LS, Exe)?

**Q3 — Pay Dispersion**
Where is internal pay dispersion most extreme inside large agencies — and which agencies are most compressed?

---

## Dataset

| Property | Value |
|----------|-------|
| Source | [DC Open Data — DC Public Employee Salary Dataset](https://opendata.dc.gov/datasets/dc-employee-salary-information) |
| Raw rows | 40,611 |
| Raw columns | 9 |
| Agencies | 88 |
| Job titles | 3,698 |
| **After cleaning** | **35,627 rows · 12 columns · 0 missing critical fields** |

**Variables:** Name · Job Title · Agency · Grade · Annual Compensation · Hire Date · Appointment Type

**Engineered features:** `TENURE_YEARS` · `HIRE_YEAR` · `HIRE_DECADE` · `APPT_CATEGORY` · `APPT_STATUS`

---

## Methodology

### Data Cleaning Pipeline (7 steps)

| Step | Action | Detail |
|------|--------|--------|
| 1 | Strip whitespace | All string columns |
| 2 | Normalize job titles | Standardized to title case |
| 3 | Parse hire dates | `%Y/%m/%d` format → datetime; zero rows dropped |
| 4 | Apply $10K salary floor | Removed 4,984 data-entry artifacts (some under $1) |
| 5 | Fill missing grades | 40 missing values → sentinel `"UNK"` (rows preserved) |
| 6 | Engineer features | `TENURE_YEARS`, `HIRE_YEAR`, `HIRE_DECADE`, `APPT_CATEGORY`, `APPT_STATUS` |
| 7 | Drop ID columns | Removed `OBJECTID` and redundant `HIREDATE_STRING` |

---

## Visualizations

Eight visualizations are produced in the notebook:

| # | Chart | Key Insight |
|---|-------|-------------|
| 1 | Histogram — compensation distribution | Mean ≈ median at $100K; right tail to $400K barely moves the mean |
| 2 | Treemap — agency size × median pay | DCPS alone is 28% of workforce; OCFO/OAG are small but high-paid outliers |
| 3 | Box plots — pay by appointment category | Clean ordering: Exe > LS > MSS > ExS > Acting ≈ Ed > CS |
| 4 | Line chart — tenure bands by appointment track | LS doubles pay across a career; MSS is flat from year one |
| 5 | Heatmap — agency × appointment category | MPD exec pay ($305K) vs. DCPS exec pay ($125K) — different missions, different structures |
| 6 | Bar chart — P90/P10 pay dispersion per agency | DDOT (5.8×) is a striking outlier; DOC and CFSA most compressed (1.7×) |
| 7 | Scatter — tenure vs. compensation | r = 0.293 pooled; heterogeneous by track |
| 8 | Faceted KDE — pay density by appointment category | Visual of the multi-modal distribution within each track |

---

## Repository Structure

```
mapping-the-machine/
├── README.md
├── .gitignore
├── notebook/
│   └── Mapping_the_Machine.ipynb     ← full analysis notebook
├── data/
│   └── sule_final_project.csv        ← cleaned dataset (35,627 rows)
├── docs/
│   ├── Mapping_the_Machine_Report.pdf
│   └── Mapping_the_Machine_Presentation.pdf
└── exports/
    └── Mapping_the_Machine.html       ← rendered notebook (no install needed)
```

---

## Getting Started

### Prerequisites

```bash
pip install pandas matplotlib seaborn plotly jupyter
```

### Run the notebook

```bash
git clone https://github.com/Slubega/mapping-the-machine.git
cd mapping-the-machine
jupyter notebook notebook/Mapping_the_Machine.ipynb
```

The data file is already included at `data/sule_final_project.csv` — no download required.

### View without installing anything

Open [`exports/Mapping_the_Machine.html`](exports/Mapping_the_Machine.html) in any browser for a fully rendered, static version of the notebook.

---

## Limitations

- **Cross-sectional snapshot** — a single point-in-time dataset; individual pay trajectories cannot be inferred.
- **Part-time normalization** — `COMPRATE` is an annual rate; part-time positions may be imperfectly represented.
- **No demographic data** — the dataset contains no race, gender, or age columns, so pay equity questions along those dimensions cannot be answered here.

---

## References

- DC Open Data — [DC Public Employee Salary Dataset](https://opendata.dc.gov/datasets/dc-employee-salary-information)
- DC DCHR — [Classification & Compensation Framework](https://dchr.dc.gov/page/classification-and-compensation)
- McKinney, W. (2010). *Data Structures for Statistical Computing in Python.*
- Waskom, M. L. (2021). Seaborn: Statistical Data Visualization. *Journal of Open Source Software, 6*(60), 3021.
- Hunter, J. D. (2007). Matplotlib: A 2D Graphics Environment. *Computing in Science & Engineering, 9*(3), 90–95.
- Plotly Technologies Inc. (2015). *Plotly Express documentation.* https://plotly.com/python/plotly-express/

---

*MATH 014 — Introduction to Data Science · Spring 2026 · Howard University*

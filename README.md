# Crime in India 2021 — Statistical Analysis in R

> **Source:** NCRB Crime in India 2021, Volume I &nbsp;|&nbsp; **Language:** R &nbsp;|&nbsp; **Charts:** Base R Graphics

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Repository Structure](#2-repository-structure)
3. [Data Sources & Extraction](#3-data-sources--extraction)
4. [CSV Datasets](#4-csv-datasets)
5. [Statistical Analyses & Outcomes](#5-statistical-analyses--outcomes)
6. [Output Files](#6-output-files)
7. [Prerequisites](#7-prerequisites)
8. [How to Run](#8-how-to-run)
9. [Interpreting Results](#9-interpreting-results)
10. [Limitations & Notes](#10-limitations--notes)

---

## 1. Project Overview

This project extracts structured crime statistics from the National Crime Records Bureau (NCRB) **Crime in India 2021** report (Volume I) and applies three rigorous statistical methods in R to uncover patterns across Indian States and Union Territories.

The analysis covers:

- State-wise IPC and SLL crime trends from **2019 to 2021**
- Correlations between crime rates, population, and charge-sheeting efficiency
- Linear regression linking **murder rates to rape rates**
- ANOVA testing for **COVID-19 pandemic effects** on crime volumes

> **Key insight:** India reported **60,96,310** total cognizable crimes in 2021 — a 7.6% decline from 2020. However, crime against women **rose 15.3%** over the same period.

---

## 2. Repository Structure

| File / Folder | Description |
|---|---|
| `crime_analysis.R` | Main R script — creates CSVs and runs all 3 analyses |
| `dataset1_IPC_SLL_statewise.csv` | All 36 States/UTs: IPC, SLL, population, crime rates |
| `dataset2_murder_rape_statewise.csv` | 28 States: murder, rape, attempt-murder, dowry death rates |
| `dataset3_crime_trends_2019_2021.csv` | 19 States × 3 years: IPC cases (long format) |
| `plot1_correlation_matrix.png` | Heatmap of Pearson correlations between crime indicators |
| `plot2_regression_murder_rape.png` | Scatter plot with regression line: murder vs rape rate |
| `plot3_anova_year_crime.png` | Box plot comparing IPC volumes across 2019, 2020, 2021 |
| `Crime_in_India_2021_Volume_I.pdf` | Original NCRB source document (input) |
| `README.md` | This documentation file |
| `HOW_TO_RUN_VSCODE.docx` | Step-by-step VS Code execution guide |

---

## 3. Data Sources & Extraction

### 3.1 Source Document

- **Document:** Crime in India 2021, Volume I
- **Publisher:** National Crime Records Bureau (NCRB), Ministry of Home Affairs, Government of India
- **Published:** August 2022 &nbsp;|&nbsp; **Pages:** 546 &nbsp;|&nbsp; **Format:** PDF
- **Website:** https://ncrb.gov.in

### 3.2 Tables Used

| NCRB Table | Content | Used In Dataset |
|---|---|---|
| Table 1A.1 | IPC Crimes – State/UT-wise 2019–2021 | Dataset 1 & 3 |
| Table 1A.2 | SLL Crimes – State/UT-wise 2021 | Dataset 1 |
| Table 1A.3 | Total IPC + SLL – State/UT-wise 2021 | Dataset 1 |
| Table 1A.4 | IPC Crimes – Crime Head & State/UT 2021 | Dataset 2 |

### 3.3 Extraction Methodology

Data was extracted from the PDF using Python's `pdfplumber` library. Tables were parsed from text-extractable pages, cleaned, and hard-coded into the R script to ensure reproducibility without requiring any external PDF-reading dependency in R.

---

## 4. CSV Datasets

### 4.1 `dataset1_IPC_SLL_statewise.csv`

**Rows:** 36 (28 States + 8 UTs) &nbsp;|&nbsp; **Columns:** 13

| Column | Type | Description |
|---|---|---|
| `State_UT` | Character | Name of State or Union Territory |
| `Type` | Character | `"State"` or `"UT"` |
| `Population_Lakhs` | Numeric | Mid-year projected population 2021 (in lakhs) |
| `IPC_2019` / `IPC_2020` / `IPC_2021` | Integer | IPC case registrations per year |
| `IPC_CrimeRate_2021` | Numeric | IPC cases per lakh population (2021) |
| `IPC_CSRate_2021` | Numeric | Charge-sheeting rate for IPC cases (%) |
| `SLL_2021` | Integer | Special & Local Laws cases registered |
| `SLL_CrimeRate_2021` | Numeric | SLL cases per lakh population |
| `SLL_CSRate_2021` | Numeric | Charge-sheeting rate for SLL cases (%) |
| `Total_2021` | Integer | Total cognizable crimes (IPC + SLL) |
| `Total_CrimeRate_2021` | Numeric | Total crime rate per lakh population |

### 4.2 `dataset2_murder_rape_statewise.csv`

**Rows:** 28 (States only) &nbsp;|&nbsp; **Columns:** 10

| Column | Description |
|---|---|
| `State_UT` | State name |
| `Region` | Macro-region (North / South / East / West / Central / NE) |
| `Murder_Incidences` | Total murder cases registered (2021) |
| `Murder_Rate` | Murder cases per lakh population |
| `Rape_Incidences` | Total rape cases registered (Section 376 IPC) |
| `Rape_Rate` | Rape cases per lakh population |
| `Attempt_Murder_Incidences` | Cases of attempt to commit murder (Section 307 IPC) |
| `Attempt_Murder_Rate` | Attempt-murder rate per lakh population |
| `Dowry_Deaths_Incidences` | Dowry death cases registered (Section 304B IPC) |
| `Dowry_Deaths_Rate` | Dowry death rate per lakh population |

### 4.3 `dataset3_crime_trends_2019_2021.csv`

**Rows:** 57 (19 states × 3 years) &nbsp;|&nbsp; **Columns:** 3 &nbsp;|&nbsp; **Format:** Long (tidy)

| Column | Description |
|---|---|
| `State_UT` | State name (19 major states) |
| `Year` | Year: 2019, 2020, or 2021 |
| `IPC_Cases` | Number of IPC cases registered that year |

---

## 5. Statistical Analyses & Outcomes

### Analysis 1 — Pearson Correlation Matrix

| Property | Detail |
|---|---|
| Method | Pearson product-moment correlation — `cor.test()` |
| Dataset | `dataset1_IPC_SLL_statewise.csv` (28 States only) |
| Variables | Population, IPC Crime Rate, IPC CS Rate, SLL Crime Rate, SLL CS Rate, Total Crime Rate |
| Output | 6 × 6 correlation matrix + significance tests for key pairs |
| Plot | `plot1_correlation_matrix.png` |

#### Results

| Pair | r | p-value | Significance |
|---|---|---|---|
| IPC Crime Rate vs Total Crime Rate | 0.818 | < 0.0001 | ✅ Very Strong |
| IPC Crime Rate vs SLL Crime Rate | 0.622 | 0.0004 | ✅ Significant |
| IPC Crime Rate vs CS Rate | 0.346 | 0.071 | ❌ Not significant |
| IPC Crime Rate vs Population | 0.236 | 0.226 | ❌ Not significant |

**Interpretation:** IPC cases dominate total crime figures — states with high IPC crime rates also tend to register high SLL violations. Crucially, larger population does **not** automatically mean a higher crime rate per lakh, pointing to structural and socioeconomic drivers rather than sheer population size.

---

### Analysis 2 — Simple Linear Regression

| Property | Detail |
|---|---|
| Method | Ordinary Least Squares — `lm()` |
| Dataset | `dataset2_murder_rape_statewise.csv` (28 States) |
| Formula | `Rape_Rate ~ Murder_Rate` |
| Question | Do states with higher murder rates also report higher rape rates? |
| Plot | `plot2_regression_murder_rape.png` |

#### Results

| Metric | Value |
|---|---|
| Intercept | 1.022 |
| Slope (Murder_Rate) | **1.987** |
| p-value | **0.034** ✅ Significant |
| R² | 0.161 |
| Adjusted R² | 0.129 |

**Interpretation:** There is a statistically significant positive relationship — each +1 unit rise in murder rate is associated with ~2 additional rapes per lakh population. However, R² = 0.16 means **84% of the variance** in rape rates is explained by other factors (socioeconomic conditions, reporting culture, policing). Rajasthan tops rape rate at 16.4/lakh; West Bengal and Jharkhand lead in attempt-murder rates.

---

### Analysis 3 — One-Way ANOVA + Tukey HSD

| Property | Detail |
|---|---|
| Method | One-Way ANOVA — `aov()` + `TukeyHSD()` |
| Dataset | `dataset3_crime_trends_2019_2021.csv` (19 States × 3 Years) |
| Formula | `IPC_Cases ~ Year` |
| Question | Did COVID-19 (2020) cause a statistically significant shift in IPC crimes? |
| Plot | `plot3_anova_year_crime.png` |

#### Results

| Metric | Value |
|---|---|
| F-statistic | 0.733 |
| p-value | 0.485 ❌ Not significant |
| Mean IPC 2019 | 1,65,734 |
| Mean IPC 2020 | 2,19,558 |
| Mean IPC 2021 | 1,88,494 |

**Tukey HSD pairwise comparisons — no year pair is significantly different.**

**Interpretation:** Although 2020 shows a +32% higher mean, this is **not statistically significant** due to extremely high within-year variance. Tamil Nadu's SLL explosion (1.7 lakh → 8.9 lakh in 2020) and Gujarat's lockdown-related cases created outliers that inflated variability. The null hypothesis (no year difference) cannot be rejected at α = 0.05.

---

## 6. Output Files

| File | Type | Generated By | Description |
|---|---|---|---|
| `dataset1_IPC_SLL_statewise.csv` | CSV | Part 1 of script | 36-row state/UT crime summary |
| `dataset2_murder_rape_statewise.csv` | CSV | Part 1 of script | 28-row violent crime rates |
| `dataset3_crime_trends_2019_2021.csv` | CSV | Part 1 of script | 57-row long-format trend data |
| `plot1_correlation_matrix.png` | PNG | Analysis 1 | 800×700 correlation heatmap |
| `plot2_regression_murder_rape.png` | PNG | Analysis 2 | 800×600 regression scatter |
| `plot3_anova_year_crime.png` | PNG | Analysis 3 | 800×600 ANOVA box plot |

All output files are saved in your **R working directory** (the folder where you run the script).

---

## 7. Prerequisites

### Software Required

- **R 4.0 or higher** — https://cran.r-project.org/
- **RStudio** (optional) or **VS Code** with R extension
- No additional R packages required — script uses **Base R only**

### Optional Packages (for enhanced plots)

```r
install.packages(c("ggplot2", "corrplot", "dplyr"))
```

---

## 8. How to Run

### Option A — PowerShell / Terminal

```powershell
cd "C:\Users\shiva\Downloads\CRIME ANALYSIS 2020 2021"
Rscript crime_analysis.R
```

### Option B — R Console

```r
setwd("C:/Users/shiva/Downloads/CRIME ANALYSIS 2020 2021")
source("crime_analysis.R")
```

### Option C — RStudio

Open `crime_analysis.R` → press **Ctrl + Alt + R** (Run All)

### Option D — VS Code

See `HOW_TO_RUN_VSCODE.docx` included in this project.

---

## 9. Interpreting Results

### Correlation Matrix (Plot 1)
- Values range from **-1 to +1**
- **Green** = positive correlation, **Red** = negative correlation
- Values close to ±1 → strong relationship; values near 0 → no linear relationship

### Regression Plot (Plot 2)
- Each dot = one Indian state
- The **red line** is the OLS best-fit regression line
- State abbreviations are labeled next to each point
- **R²** shown in legend — higher means better model fit

### ANOVA Box Plot (Plot 3)
- Each box shows median (middle line), IQR (box body), and outliers (dots)
- **Red diamonds** mark the group mean
- Overlapping boxes typically indicate no significant difference
- Check Tukey HSD p-values in console output for exact pairwise comparisons

---

## 10. Limitations & Notes

- Data reflects **police-registered FIRs only** — actual crime may be underreported
- NCRB uses the **Principal Offence Rule**: only the most serious crime in an FIR is counted
- Population figures use **2011 Census projections** — may not reflect 2021 realities
- Crime rates for States and Metropolitan Cities are **not directly comparable**
- Tamil Nadu's 2020 spike (SLL: 1.7 lakh → 8.9 lakh) is largely lockdown-enforcement-related
- Delhi UT's extremely high crime rate (1,410/lakh) is partly driven by online FIR registrations
- Regression R² = 0.16 — many socioeconomic factors not modeled here

---

*Crime in India 2021 Statistical Analysis — NCRB, Ministry of Home Affairs, Government of India*
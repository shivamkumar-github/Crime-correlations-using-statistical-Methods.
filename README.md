# 📊 Crime Correlation Analysis in R

This project explores the correlation and differences in various crime-related data points across 2020 and 2021 using statistical tests in **R**. The analysis is based on data from the **National Crime Records Bureau (NCRB)** and applies statistical techniques to draw conclusions about crime trends in India.

## 📁 Case Studies Overview

### **Case Study 1: Yearly Crime Comparison (2020 vs 2021)**
- **Objective**: Check whether there is a significant difference in the number of cases registered in 2020 and 2021.
- **Test Used**: Wilcoxon Signed-Rank Test (non-parametric)
- **Hypotheses**:
  - H₀: No significant difference in median number of cases between 2020 and 2021.
  - H₁: Significant difference exists.
- **Conclusion**: P-value = 0.8259 > α = 0.05 → Fail to reject H₀. No significant difference.

---

### **Case Study 2: Crime Type Relationship Analysis (Against Women)**
- **Objective**: Study the relationship among different crimes against women.
  - **Crimes included**:
    - Section 498A (Cruelty by husband or relatives)
    - Section 376/511 (Attempt to commit rape)
    - Assault on women with intent to outrage modesty
- **Test Used**: Friedman Test
- **R Code**:
  ```r
  property <- c(77,0,105,635,68,...)
  family <- c(182,8,82,265,93,...)
  money <- c(40,0,15,118,15,...)

  data1 <- data.frame(property, family, money)
  friedman.test(as.matrix(data1))
  boxplot(data1, col = c(4, 5, 6))
  ```
- **Conclusion**: Used to determine whether these crime types are statistically different across states or categories.

---

### **Case Study 3: Gender-based Conviction Analysis**
- **Objective**: Determine if there is a significant difference in conviction rates between males and females for crimes against women.
- **Hypotheses**:
  - H₀: No difference in male and female conviction proportions.
  - H₁: Significant difference exists.
- **Conclusion**: Sample data is too extreme to support H₀; evidence suggests a significant difference.

---

## 🔍 Supporting Data Sources
- **NCRB Crime in India 2021 Report**
- **US FBI & CDC Dashboards**:
  - [FBI Crime Explorer](https://crime-data-explorer.fr.cloud.gov/pages/explorer/crime/nibrs-estimation)
  - [CDC Mortality Dashboard](https://www.cdc.gov/nchs/nvss/vsrr/mortality-dashboard.htm)

---

## 📦 Requirements
- R (>= 4.0)
- RStudio
- Base packages (`stats`, `graphics`)

---

## 📈 Visualizations
- Boxplots used to compare distributions across different crime categories.
- Statistical outputs from tests displayed in the console.

---

## 🧪 Statistical Concepts Applied
- **Wilcoxon Signed-Rank Test**: For comparing paired non-normally distributed data.
- **Friedman Test**: For non-parametric comparison of more than two related groups.

---

## 🗃️ Data Handling Notes
- Data based on "Principal Offence Rule" — only the most severe offence is counted per FIR.
- Crime data is available at national, state, and metro levels.


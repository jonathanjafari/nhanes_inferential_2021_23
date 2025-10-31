# NHANES Data Analysis Project (2021–2023)

This project explores relationships between demographic, behavioral, and health variables using data from the **National Health and Nutrition Examination Survey (NHANES)** 2021–2023.  
Analyses were conducted in **Google Colab**, using **R** for data conversion and **Python** (`pandas`, `scipy`, `statsmodels`) for data cleaning, transformation, and inferential testing.

---

## Project Objective
To investigate associations and differences between key demographic and health indicators using appropriate statistical methods.  
The project includes four guided analyses and one creative question using cleaned NHANES data.

---

## Data Sources
All datasets were obtained directly from the [NHANES 2021–2023 Continuous Data Portal](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?Cycle=2021-2023).

| Dataset | Filename | Key Variables Used |
|---------|----------|-------------------|
| Demographics | `DEMO_L.xpt` | Marital status (DMDMARTZ), Education (DMDEDUC2), Age (RIDAGEYR) |
| Blood Pressure | `BPXO_L.xpt` | Systolic (BPXOSY3), Diastolic (BPXODI3) |
| Vitamin D | `VID_L.xpt` | Vitamin D lab interpretation (LBDVD2LC) |
| Kidney – Urology | `KIQ_U_L.xpt` | Weak/failing kidneys (KIQ022) |
| Body Measures | `BMX_L.xpt` | Weight (BMXWT) |

---

## R Integration
R was used to read NHANES SAS transport files (`.xpt`) and export them to `.csv` for use in Python.  
This step demonstrates interoperability between R and Python in Colab.

```r
%%R
library(haven)
setwd("/content")
files <- c("DEMO_L.xpt","BPXO_L.xpt","VID_L.xpt","KIQ_U_L.xpt","BMX_L.xpt")
for (f in files) {
  if (file.exists(f)) {
    dat <- read_xpt(f)
    write.csv(dat, sub(".xpt$", ".csv", f, ignore.case = TRUE), row.names = FALSE)
  }
}
cat("XPT to CSV conversion complete.\n")
```

---

## Questions and Analyses

### Question 1 — Association Between Marital Status and Education Level

**Variables:** DMDMARTZ (Marital Status), DMDEDUC2 (Education Level)

**Recoding:**
- DMDMARTZ: recoded to Married (1) vs Not Married (0)
- DMDEDUC2: recoded to Bachelor's degree or higher (1) vs Less than bachelor's (0)
- Invalid codes (77, 99) removed

**Test Used:** Chi-square test of independence  
**Results:** χ²(1) = 128.42, *p* = 9.07 × 10⁻³⁰  
**Analysis:** There is a statistically significant association between marital status and education level (*p* < 0.05). Married adults are more likely to hold a bachelor's degree or higher compared to those not married.

---

### Question 2 — Difference in Mean Weight by Marital Status

**Variables:** DMDMARTZ (Marital Status), BMXWT (Weight, kg)

**Cleaning:**
- Removed invalid codes (7777, 9999, and null values)
- Restricted to adult participants (≥ 18 years)
- Recoded DMDMARTZ to Married (1) vs Not Married (0)

**Test Used:** Welch's *t*-test (unequal variances)  
**Results:** Married = 83.3 kg (SD = 21.4, n = 3,250); Not married = 83.3 kg (SD = 23.5, n = 2,732); *t* = 0.096, *p* = 0.9237  
**Analysis:** No significant difference in mean body weight between married and not-married adults (*p* > 0.05). Mean weights were nearly identical.

---

### Question 3 — Effect of Age and Marital Status on Systolic Blood Pressure

**Variables:** RIDAGEYR (Age), DMDMARTZ (Marital Status), BPXOSY3 (Systolic BP)

**Cleaning and Recoding:**
- Removed invalid codes for marital status (77, 99)
- Excluded missing or null blood pressure readings
- Recoded DMDMARTZ to Married (1) vs Not Married (0)

**Test Used:** Multiple Linear Regression (OLS)  
**Results:** β_age = 0.395 (*p* < 0.001); β_married = −1.33 (*p* = 0.003); R² = 0.135  
**Analysis:** Systolic blood pressure increases with age, and after controlling for age, married adults have slightly lower systolic BP (−1.3 mmHg). The effect is statistically significant but small.

---

### Question 4 — Correlation Between Weight and Systolic Blood Pressure

**Variables:** BMXWT (Weight, kg), BPXOSY3 (Systolic BP, mmHg)

**Cleaning:**
- Removed invalid weight and BP values (7777, 9999)
- Excluded null or placeholder data
- Continuous variables standardized for correlation testing

**Test Used:** Pearson correlation  
**Results:** *r* = 0.217, *p* = 5.46 × 10⁻⁸⁰  
**Analysis:** There is a weak positive correlation between body weight and systolic blood pressure. The relationship is statistically significant but modest in strength.

---

### Question 5 (Creative Analysis) — Vitamin D Level and Diastolic Blood Pressure

**Variables:** LBDVD2LC (Vitamin D Interpretation), BPXODI3 (Diastolic BP, mmHg)

**Cleaning:**
- Excluded null and placeholder codes
- Retained only valid Vitamin D categories (two-level interpretation)

**Test Used:** One-way ANOVA  
**Results:** *F* = 1.367, *p* = 0.2423  
**Analysis:** No statistically significant difference in mean diastolic BP across Vitamin D interpretation groups (*p* > 0.05).

---

## How to Run

1. Open the notebook `nhanes_inferential_2021_23.ipynb` in Google Colab.
2. Upload the `.xpt` files listed above when prompted.
3. Run the R cell to convert `.xpt` → `.csv`.
4. Execute each Python cell sequentially for data merging, cleaning, and analysis.
5. Review the outputs, tables, and Markdown summaries for interpretation.

---

## Notes and Limitations

- Data cleaned to remove invalid codes (e.g., 7777, 9999) and limited to adults (≥ 18 years).
- NHANES uses a complex survey design; these analyses are unweighted and for educational purposes only.
- Statistical significance does not imply causation.
- R was used for file conversion; all inferential analyses were conducted in Python.

---

## Repository Contents

```
nhanes_inferential_2021_23/
│
├── nhanes_inferential_2021_23.ipynb   # Full Google Colab analysis (R + Python)
└── README.md                          # Project overview and interpretations
```

---

**Author:** Jonathan Jafari, D.O.  
**Course:** HHA 507 – Python & Data Science for Healthcare  
**Institution:** Stony Brook University (MS-Applied Health Informatics)  
**Date:** October 2025

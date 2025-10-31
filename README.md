# NHANES Data Analysis Project (2021–2023)

This project explores relationships between demographic, behavioral, and health variables using data from the **National Health and Nutrition Examination Survey (NHANES)** 2021–2023.

Analyses were conducted in **Google Colab**, using **R** for data conversion and **Python** (`pandas`, `scipy`, `statsmodels`) for data cleaning, transformation, and inferential testing.

---

## Project Objective

To investigate associations and differences between key demographic and health indicators using appropriate statistical methods. The project includes four guided analyses and one creative question using cleaned NHANES data.

---

## Data Sources

All datasets were obtained directly from the [NHANES 2021–2023 Continuous Data Portal](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?Cycle=2021-2023).

| Dataset | Filename | Key Variables Used |
|---------|----------|-------------------|
| Demographics | `DEMO_L.xpt` | Marital status (DMDMARTZ), Education (DMDEDUC2), Age (RIDAGEYR) |
| Blood Pressure | `BPXO_L.xpt` | Systolic (BPXOSY3), Diastolic (BPXODI3) |
| Vitamin D | `VID_L.xpt` | Vitamin D lab interpretation (LBDVD2LC) |
| Kidney – Urology | `KIQ_U_L.xpt` | Weak/failing kidneys (KIQ022) |
| Physical Activity | `PAQ_L.xpt` | Minutes sedentary (PAD680) |
| Weight History | `WHQ_L.xpt` | Self-reported weight (WHD020) (optional; primary analyses use BMXWT from BMX_L.xpt) |
| Body Measures | `BMX_L.xpt` | Measured weight (BMXWT) |

---

## R Integration

R was used to read NHANES SAS transport files (`.xpt`) and export them to `.csv` for use in Python. This demonstrates interoperability between R and Python within Colab.

```r
%%R
library(haven)
setwd("/content")
files <- c("DEMO_L.xpt","BPXO_L.xpt","VID_L.xpt","KIQ_U_L.xpt","PAQ_L.xpt","WHQ_L.xpt","BMX_L.xpt")
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
- DMDMARTZ: Married (1) vs Not Married (0)
- DMDEDUC2: Bachelor's degree or higher (1) vs Less than bachelor's (0)
- Invalid codes (77, 99) removed

**Test Used:** Chi-square test of independence  
**Results:** χ²(1) = 128.42, *p* = 9.07 × 10⁻³⁰  
**Interpretation:** Statistically significant association between marital status and education level (*p* < 0.05). Married adults are more likely to hold a bachelor's degree or higher compared to those not married.

---

### Question 2 — Difference in Mean Sedentary Behavior Time by Marital Status

**Variables:** DMDMARTZ (Marital Status), PAD680 (Sedentary Behavior Time in minutes/day → renamed `sedentary_min`)

**Cleaning:**  
Removed placeholder values (7777, 9999), dropped missing values, restricted to adult participants (≥ 18 years).

**Test Used:** Welch's *t*-test (unequal variances)  
**Results:** Married = 353.3 min/day (SD = 203.9, n = 4106); Not married = 371.9 min/day (SD = 219.4, n = 3611); *t* = −3.846, *p* = 0.00012  
**Interpretation:** Statistically significant difference in sedentary time between married and not-married adults (*p* < 0.05). Not-married adults report slightly more sedentary minutes on average.

---

### Question 3 — Effect of Age and Marital Status on Systolic Blood Pressure

**Variables:** RIDAGEYR (Age), DMDMARTZ (Marital Status), BPXOSY3 (Systolic BP)

**Cleaning and Recoding:**  
Removed invalid marital codes (77, 99), excluded null BP readings, recoded Married (1) vs Not Married (0).

**Test Used:** Multiple Linear Regression (OLS)  
**Results:** β_age = 0.395 (*p* < 0.001); β_married = −1.33 (*p* = 0.003); R² = 0.135  
**Interpretation:** Systolic BP increases with age; after controlling for age, married adults have slightly lower SBP (−1.3 mmHg). The effects are statistically significant but small.

---

### Question 4 — Correlation Between Weight and Systolic Blood Pressure

**Variables:** BMXWT (Weight in kg), BPXOSY3 (Systolic BP in mmHg)

**Cleaning:**  
Removed placeholder/invalid values (7777, 9999), dropped nulls.

**Test Used:** Pearson Correlation  
**Results:** *r* = 0.217, *p* = 5.456 × 10⁻⁸⁰  
**Interpretation:** Positive linear correlation between body weight and systolic BP (α = 0.05). The correlation is statistically significant but modest in strength.

---

### Question 5 — Vitamin D Level and Diastolic Blood Pressure (Creative Analysis)

**Variables:** LBDVD2LC (Vitamin D Interpretation), BPXODI3 (Diastolic BP)

**Cleaning:**  
Removed nulls, retained valid two-level Vitamin D interpretations.

**Test Used:** One-way ANOVA  
**Results:** *F* = 1.367, *p* = 0.2423  
**Interpretation:** No significant difference in mean diastolic BP across Vitamin D interpretation groups (*p* > 0.05).

---

## How to Run

1. Open the notebook `nhanes_inferential_2021_23.ipynb` in **Google Colab**.
2. Upload the `.xpt` files:  
   `DEMO_L.xpt`, `BPXO_L.xpt`, `VID_L.xpt`, `KIQ_U_L.xpt`, `PAQ_L.xpt`, `WHQ_L.xpt`, and `BMX_L.xpt`.
3. Run the R cell to convert `.xpt` → `.csv`.
4. Execute Python cells sequentially for data merging, cleaning, and analysis.
5. Review printed outputs, visualizations, and Markdown summaries for interpretation.

---

## Notes and Limitations

- Data cleaned to remove invalid codes (7777, 9999) and limited to adults (≥ 18 years).
- NHANES uses a complex survey design; analyses are unweighted and for educational use only.
- Statistical significance ≠ causation.
- R was used for file conversion; all inferential tests were conducted in Python.

---

## Preferred Coding Language

While both R and Python were used, I primarily prefer **Python** for data analysis and visualization. Python's `pandas`, `scipy`, and `statsmodels` libraries provide an efficient workflow for data cleaning, transformation, and inferential testing, while R demonstrates interoperability for `.xpt` → `.csv` conversion within Colab.

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
**Institution:** Stony Brook University (MS – Applied Health Informatics)  
**Last Updated:** October 31, 2025

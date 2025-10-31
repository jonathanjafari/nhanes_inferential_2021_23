# NHANES Data Analysis Project (2021–2023)

This project explores relationships between demographic, behavioral, and health variables using the **NHANES 2021–2023 cycle**.

Analyses were conducted in **Google Colab**, using **R** for `.xpt` → `.csv` conversion and **Python** (`pandas`, `scipy`, `statsmodels`) for cleaning, transformation, and inferential testing.

---

## Project Objective

Investigate associations and differences between key demographic and health indicators using appropriate statistical methods. Includes four guided analyses and one creative analysis.

---

## Data Sources

All datasets were obtained from the [NHANES 2021–2023 Continuous Data Portal](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?Cycle=2021-2023).

| Dataset | Filename | Key Variables Used |
|---------|----------|-------------------|
| Demographics | `DEMO_L.xpt` | Marital status (DMDMARTZ), Education (DMDEDUC2), Age (RIDAGEYR) |
| Blood Pressure | `BPXO_L.xpt` | Systolic (BPXOSY3), Diastolic (BPXODI3) |
| Vitamin D | `VID_L.xpt` | Vitamin D interpretation (LBDVD2LC) |
| Kidney – Urology | `KIQ_U_L.xpt` | Weak/failing kidneys (KIQ022) |
| Physical Activity | `PAQ_L.xpt` | Sedentary minutes (PAD680) |
| Weight History | `WHQ_L.xpt` | Self-reported weight (WHD020) |
| Body Measures | `BMX_L.xpt` | Measured weight (BMXWT) |

---

## R Integration

R was used to read NHANES SAS transport files (`.xpt`) and export them to `.csv` for use in Python.

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

**Variables:** DMDMARTZ (marital status), DMDEDUC2 (education)

**Recoding/Cleaning:**
- DMDMARTZ: Married (1) vs Not married (0); remove 77, 99
- DMDEDUC2: Bachelor's+ (1) vs < Bachelor's (0); remove 7, 9

**Test:** Chi-square test of independence  
**Results:** χ²(1) = 128.42, *p* = 9.07 × 10⁻³⁰  
**Interpretation:** Significant association; married adults are more likely to have a bachelor's degree or higher.

---

### Question 2 — Difference in Mean Sedentary Behavior Time by Marital Status

**Variables:** DMDMARTZ (marital status), PAD680 (sedentary minutes/day → `sedentary_min`)

**Cleaning:** Remove 7777/9999, drop missing, adults only (≥18).

**Test:** Welch's *t*-test  
**Results:** Married = 353.3 min/day (SD = 203.9, n = 4106); Not married = 371.9 min/day (SD = 219.4, n = 3611); *t* = −3.846, *p* = 0.00012  
**Interpretation:** Significant difference; not-married adults report slightly more sedentary minutes on average.

---

### Question 3 — Effect of Age and Marital Status on Systolic Blood Pressure

**Variables:** RIDAGEYR (age), DMDMARTZ (marital status), BPXOSY3 (SBP)

**Cleaning/Recoding:** Remove invalid marital codes; drop missing BP; Married (1) vs Not married (0).

**Test:** OLS regression (`BPXOSY3 ~ RIDAGEYR + married_bin`)  
**Results:** β_age = 0.395 (*p* < 0.001); β_married = −1.33 (*p* = 0.003); R² = 0.135  
**Interpretation:** SBP increases with age; married adults have ≈1.3 mmHg lower SBP after adjustment (small effect).

---

### Question 4 — Correlation Between Self-Reported Weight and Sedentary Behavior Time

**Variables:**
- `WHD020` → Self-reported weight (kg) → `weight_self_kg`
- `PAD680` → Minutes per day of sedentary behavior → `sedentary_min`

**Cleaning:**  
Replaced placeholder values (7777, 9999) with missing; dropped rows with null values; restricted the dataset to adult participants (≥ 18 years).

**Test Used:** Pearson correlation  
**Results:** *r* = 0.149, *p* = 2.82 × 10⁻³²  
**Interpretation:** There is a statistically significant positive linear correlation between self-reported weight and sedentary behavior time (α = 0.05). Heavier individuals tend to report slightly more sedentary minutes per day, though the relationship is weak in magnitude.

---

### Question 5 — Vitamin D Level and Diastolic Blood Pressure (Creative)

**Variables:** LBDVD2LC (Vitamin D interpretation), BPXODI3 (DBP)

**Cleaning:** Drop missing; keep valid categories.

**Test:** One-way ANOVA  
**Results:** *F* = 1.367, *p* = 0.2423  
**Interpretation:** No significant difference in mean diastolic BP across Vitamin D interpretation groups.

---

## How to Run

1. Open `nhanes_inferential_2021_23.ipynb` in **Google Colab**.
2. Upload the `.xpt` files:  
   `DEMO_L.xpt`, `BPXO_L.xpt`, `VID_L.xpt`, `KIQ_U_L.xpt`, `PAQ_L.xpt`, `WHQ_L.xpt`, `BMX_L.xpt`.
3. Run the R cell to convert `.xpt` → `.csv`.
4. Run Python cells in order for merge, cleaning, and analyses.
5. Read outputs and Markdown interpretations under each question.

---

## Notes and Limitations

- Cleaned placeholder codes (7777, 9999) and restricted to adults (≥18).
- NHANES is a complex survey; analyses are unweighted for educational purposes.
- Statistical significance does not imply causation.
- R used for file conversion; inferential tests run in Python.

---

## Preferred Coding Language

While both R and Python were used, I primarily prefer **Python** for analysis and visualization. R was included to demonstrate `.xpt` → `.csv` conversion within Colab.

---

## Repository Contents

```
nhanes_inferential_2021_23/
│
├── nhanes_inferential_2021_23.ipynb   # Colab notebook (R + Python)
└── README.md                          # Project overview and interpretations
```

---

**Author:** Jonathan Jafari, D.O.  
**Course:** HHA 507 – Python & Data Science for Healthcare  
**Institution:** Stony Brook University (MS–Applied Health Informatics)  
**Last Updated:** October 31, 2025

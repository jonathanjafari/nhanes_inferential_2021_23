# NHANES Inferential Analysis (2021–2023)

This project explores relationships between demographic, behavioral, and health variables using data from the **National Health and Nutrition Examination Survey (NHANES) 2021–2023**.  
All analyses were conducted in **Google Colab** with data cleaning, transformation, and inferential testing performed in **Python** (`pandas`, `scipy`, and `statsmodels`).

---

## Project Objective
To investigate associations and differences between key demographic and health indicators using appropriate statistical tests.  
The project includes four guided analyses and one creative question using cleaned NHANES data.

---

## Data Sources
All datasets were obtained directly from the [NHANES 2021–2023 Continuous Data](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?Cycle=2021-2023) portal:

| Dataset | Filename | Key Variables Used |
|---------|----------|-------------------|
| Demographics | `DEMO_L.xpt` | Marital status (DMDMARTZ), Education (DMDEDUC2), Age (RIDAGEYR) |
| Blood Pressure | `BPXO_L.xpt` | Systolic (BPXOSY3), Diastolic (BPXODI3) |
| Vitamin D | `VID_L.xpt` | Vitamin D lab interpretation (LBDVD2LC) |
| Kidney – Urology | `KIQ_U_L.xpt` | Weak/failing kidneys (KIQ022) |
| Body Measures | `BMX_L.xpt` | Weight (BMXWT) |

---

## Questions and Analyses

### **Question 1 — Association between Marital Status and Education Level**
**Variables:** DMDMARTZ → Married (1) vs Not married (0); DMDEDUC2 → Bachelor's+ (1) vs < Bachelor's (0)  
**Test Used:** Chi-square test of independence  
**Results:** χ²(1) = 128.42, *p* = 9.07 × 10⁻³⁰  
**Interpretation:** There is a significant association between marital status and education level (*p* < 0.05). Married adults are **more likely** to hold a bachelor's degree or higher compared to those not married.

---

### **Question 2 — Difference in Mean Weight by Marital Status**
**Variables:** Married (1) vs Not Married (0); Weight (BMXWT → kg)  
**Test Used:** Welch's *t*-test (unequal variances)  
**Results:** Married = 83.3 kg (SD 21.4, n = 3,250); Not married = 83.3 kg (SD 23.5, n = 2,732); *t* = 0.096, *p* = 0.9237  
**Interpretation:** No significant difference in mean body weight between married and not-married adults (*p* > 0.05). Mean weights were nearly identical.

---

### **Question 3 — Effect of Age and Marital Status on Systolic Blood Pressure**
**Variables:** Age (RIDAGEYR), Marital status (married_bin), Systolic BP (BPXOSY3)  
**Test Used:** OLS Regression (`BPXOSY3 ~ RIDAGEYR + married_bin`)  
**Results:** β_age = 0.395 (*p* < 0.001); β_married = −1.33 (*p* = 0.003); R² = 0.135  
**Interpretation:** Systolic BP increases with age, and after adjusting for age, married adults have slightly lower systolic BP (−1.3 mmHg). The effect is statistically significant but small.

---

### **Question 4 — Correlation Between Weight and Systolic Blood Pressure**
**Variables:** Weight (kg), Systolic BP (mmHg)  
**Test Used:** Pearson correlation  
**Results:** *r* = 0.217, *p* = 5.46 × 10⁻⁸⁰  
**Interpretation:** A **weak positive correlation** exists between body weight and systolic BP. While statistically significant due to large sample size, the relationship is modest in strength.

---

### **Question 5 — Vitamin D Level and Diastolic Blood Pressure (Creative Analysis)**
**Variables:** Vitamin D interpretation (LBDVD2LC) and Diastolic BP (BPXODI3)  
**Test Used:** One-way ANOVA  
**Results:** *F* = 1.367, *p* = 0.2423  
**Interpretation:** No statistically significant difference in mean diastolic BP across Vitamin D interpretation groups (*p* > 0.05).

---

## Notes and Limitations
- Data were cleaned to remove invalid codes (e.g., 7777, 9999) and restricted to adults (≥ 18 years).  
- NHANES is a **complex survey** with sample weights; here, unweighted analyses were used for educational purposes.  
- Statistical significance does not imply causation; associations are descriptive.

---

## How to Run
1. Open the notebook `nhanes_inferential_2021_23.ipynb` in **Google Colab**.  
2. Upload the `.xpt` files listed above when prompted.  
3. Run each cell in order — cleaning, merging, analysis, and interpretation.  
4. Outputs (tables, results, and Markdown summaries) appear directly below each question.  

---

## Repository Contents
```
nhanes_inferential_2021_23/
│
├── nhanes_inferential_2021_23.ipynb    # Full Google Colab analysis
└── README.md                           # Project overview and interpretations
```

---

**Author:** Jonathan Jafari, D.O.  
**Course:** HHA 507 – Python & Data Science for Healthcare  
**Institution:** Stony Brook University (MS-Applied Health Informatics)  
**Date:** October 2025

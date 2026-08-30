# Certified Organic Livestock & Subsidies Analysis (2008–2011)

## Executive Summary
This project evaluates the regional distribution, longitudinal trends, and federal subsidy allocations for US certified organic livestock across two reporting periods. Utilizing state level agricultural records and certifier accreditation data, dynamic spreadsheets were constructed using advanced lookup functions, string parsing, and regular expression (REGEX) status classification. To test statistical validity, random sampling methodologies were executed to evaluate margin of error convergence, standard error behavior, and empirical probability distributions—providing a framework for modeling subsidy trends and verifying data integrity across state operations.

### Key Business Insights
* **Subsidized Cost Escalation:** Total subsidized expenditures for organic livestock surged dramatically from **$7.9M in 2008** to **$19.0M in 2011**, driven primarily by a **136% increase in subsidized milk cow costs** ($5.39M to $12.74M) and a **320% increase in beef cow costs** ($725K to $3.05M).
* **Livestock Shift:** Beef cow totals grew by **66.7%** (63,680 to 106,181), whereas sheep and lambs contracted by **20.7%** (7,455 to 5,914).
* **Certifier Retention:** Advanced regex analysis revealed an active retention rate among state certifiers, though key regional losses occurred (e.g., surrender of accreditation in Louisiana and Mississippi).

---

## Technical Stack & Functions Used
* **Spreadsheet Tool:** Google Sheets / Microsoft Excel
* * **Statistical Sampling & Inference:** Mean, Standard Error ($SE$), Margin of Error ($ME$), 95% Confidence Interval Estimation (Lower/Upper Bounds)
* **Probability & Distribution Modeling:** Relative frequency, multi-condition empirical probabilities, histogram skewness analysis
* **Lookup & Dynamic Modeling:** `VLOOKUP`, `HLOOKUP`, Absolute Referencing (`$`), `IFERROR`
* **Text Processing & Regex:** `PROPER`, `TRIM`, `REGEXREPLACE`, `REGEXEXTRACT`, `REGEXMATCH`
* **Date Parsing & Calculations:** `YEAR`, Percentage Change calculation

---

## Data Transformation & Modeling Steps

### 1. Statistical Sampling, Inferential Statistics & Probability
* **Population vs. Sample Comparative Analysis ($n = 58$) :** Evaluated sampling variability by taking random sub-samples ($n = 25$ and $n = 10$) from the 58 total state/territory entries to test point estimate accuracy:
  * **Sample 25 ($n = 25$):** Yielded a sample mean of **6,807** with a Standard Error of **1,688.80** and Margin of Error of **3,310.05** (95% CI: **[3,497 – 10,118]**).
  * **Sample 10 ($n = 10$):** Yielded a sample mean of **8,575** with a Standard Error of **2,963.00** and Margin of Error of **5,807.00** (95% CI: **[2,713 – 14,327]**).
  * **Sampling Efficiency Insight:** The data proves that taking a sample of 25 entities vs. 10 entities reduces the margin of error by roughly **43%**, indicating that policy or funding decisions regarding organic livestock subsidies should rely on samples of $n \ge 25$ to maintain statistical integrity.

* **Empirical Probability Modeling (`State Organic Livestock`):** Analyzed state-level distribution metrics to establish operational probabilities:
  * **P(Region = West):** `26.0%` probability that a selected state belongs to the West region.
  * **P(2011 Total Livestock > 10,000):** `26.1%` probability that a state exceeds 10,000 total organic livestock units.
  * **P(2011 Milk Cows between 1,000–10,000):** `32.5%` likelihood for medium-scale organic milk cow operations.

### 2. Dynamic Cross-Sheet Modeling (`Livestock and Subsidies`)
* **HLOOKUP Aggregation:** Dynamically retrieved national totals for 2008 and 2011 livestock across horizontal tables:
  ```excel
  =HLOOKUP("Beef cows", 'State Organic Livestock'!$D$4:$I$7, 4, FALSE)
```
* **VLOOKUP Price Mapping & Subsidies Costing: Applied vertical lookup against unordered price tables to calculate total subsidized valuations while managing cell locking ($):
```excel
=D4 * VLOOKUP(A4, $A$14:$C$18, 2, FALSE)
```

### 3. Data Cleaning & Regular Expressions (`Certifiers`)
* **Text Formatting & Anonymization: Standardized mixed-case text using PROPER(TRIM()) and masked confidential IDs using regex to obscure prefix and suffix digits:
```excel
=REGEXREPLACE(A4, "^.{3}|\d{3}$", "***")
```
* **Regex Status Classification: Categorized certifiers based on active vs. surrendered/revoked status using pattern matching:
```excel
=IF(REGEXMATCH(I4, "(?i)surrendered|revoked"), "Inactive", "Active")
```
* **Text Extraction & Date Parsing: Parsed geographical region names following the final comma using regex, and isolated 4-digit accreditation years:
```excel
=TRIM(REGEXEXTRACT(C4, "[^,]+$"))
```
```excel
=YEAR(D4)
```
* **Error-Handled Growth Calculations: Calculated 2008–2011 producer growth rates with graceful #DIV/0! error catching:
```excel
=IFERROR((F4 - E4) / E4, "No data")
```
## 📌 Background & Context
This project was completed as part of the **ALX Africa Data Science Program** to analyze regional trends, federal subsidy allocations, and certifier accreditation data for US certified organic livestock between 2008 and 2011. 

Through state-level agricultural records, dynamic spreadsheets were developed utilizing advanced lookup functions (`VLOOKUP`, `HLOOKUP`), regex status classification (`REGEXMATCH`, `REGEXREPLACE`), and string parsing. Statistical sampling methodologies ($n=25$ vs. $n=10$) were executed to test margin of error convergence and probability distributions—proving that an increase in sample size reduces the margin of error by approximately 43.00% to ensure empirical data integrity across state operations.

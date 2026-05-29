# Kiva Microfinance Loan Portfolio Dashboard

A 3-page executive dashboard built in **Power BI** analysing 671,205 real microfinance loan records from the [Kiva Crowdfunding dataset](https://www.kaggle.com/datasets/kiva/data-science-for-good-kiva-crowdfunding). The project covers portfolio health monitoring, funding gap analysis, and borrower demographics — replicating the kind of reporting used by microfinance institutions (MFIs).

---

## Dashboard Pages

| Page | Focus | Key Visuals |
|---|---|---|
| **Portfolio Overview** | Scale, reach, and trends | KPI cards, monthly disbursement trend, top 10 countries, loans by sector |
| **Funding Gap Analysis** | How well loans are being funded | Funding status donut, gap by sector, repayment intervals, avg loan size |
| **Borrower Demographics** | Who is borrowing | Gender breakdown, country × gender, sector × gender, avg loan term |

---

## Key Insights

- **671,205 loans** across 86 countries spanning 2014–2017
- **USD 565M** total loan amount disbursed
- **92.8% fully funded rate** — 7.2% (~48,000 loans) had unmet funding demand
- **76.7% female borrowers** — consistent with microfinance's financial inclusion focus
- **Kenya is #2** globally with 75,825 loans, behind only the Philippines
- **Agriculture dominates** at 180,302 loans (26.9% of portfolio)

---

## Tools Used

- **Python** (pandas, re) — data cleaning and feature engineering
- **Power BI Desktop** — dashboard design, DAX measures, interactive slicers
- **Kaggle** — dataset source

---

## Repository Structure

```
Kiva-microfinance-Dashboard/
├── Kiva_Loans_Data_Prep.ipynb   # Data cleaning and feature engineering notebook
├── screenshots/                 # Dashboard page screenshots
│   ├── page1_portfolio_overview.png
│   ├── page2_funding_gap.png
│   └── page3_borrower_demographics.png
└── README.md
```

> **Note:** The raw dataset (`kiva_loans.csv`) and cleaned output (`kiva_loans_clean.csv`) are not included due to file size. Download the raw data from Kaggle using the link above.

---

## Setup & Reproduction

### 1. Download the dataset
Go to [Kaggle — Kiva Crowdfunding](https://www.kaggle.com/datasets/kiva/data-science-for-good-kiva-crowdfunding) and download `kiva_loans.csv`. Place it in the same directory as the notebook.

### 2. Run the notebook
```bash
pip install pandas
jupyter notebook Kiva_Loans_Data_Prep.ipynb
```
Run all cells. This generates `kiva_loans_clean.csv`.

### 3. Open in Power BI
- Open Power BI Desktop
- **Home → Get Data → Text/CSV** → load `kiva_loans_clean.csv`
- Recreate the DAX measures below and build the 3 dashboard pages

---

## DAX Measures

```dax
Total Loans = COUNT(kiva_loans_clean[id])

Total Disbursed = SUM(kiva_loans_clean[loan_amount])

Total Funded = SUM(kiva_loans_clean[funded_amount])

Avg Loan Size = AVERAGE(kiva_loans_clean[loan_amount])

Funding Gap Total = SUM(kiva_loans_clean[funding_gap])

Fully Funded Rate =
DIVIDE(
    CALCULATE(COUNT(kiva_loans_clean[id]), kiva_loans_clean[fully_funded] = 1),
    COUNT(kiva_loans_clean[id])
) * 100

Avg Term Months = AVERAGE(kiva_loans_clean[term_in_months])

Avg Lenders Per Loan = AVERAGE(kiva_loans_clean[lender_count])
```

---

## Engineered Columns

| Column | Description |
|---|---|
| `funding_gap` | `loan_amount` − `funded_amount`. Measures unmet borrower demand. |
| `fully_funded` | Binary flag: 1 = fully funded, 0 = not fully funded. |
| `funding_status` | Text label for Power BI legends: `'Fully Funded'` / `'Not Fully Funded'`. |
| `year` | Year extracted from `posted_time`. |
| `month` | Month number (1–12) for correct calendar sort order. |
| `month_name` | Month abbreviation (Jan–Dec), sorted as Categorical. |
| `primary_gender` | Majority gender of borrower(s) using regex word boundary matching. |

---

## Author

**Emmanuel Kironji**  
BSc. Financial Engineering — JKUAT  
[kironjiemmanuel@gmail.com](mailto:kironjiemmanuel@gmail.com)

# 📦 Delivery Delay Analysis

A data analysis project examining delivery performance across routes, hubs, and service types — identifying where and why shipments are delayed, and quantifying the scale of the problem month over month.

**Author:** Mahesh Lohar

---

## 📁 Project Structure

```
.
├── analysis_ex.xlsx     # Source data + cleaned data + pivot summary (Excel)
├── Exam_.ipynb          # Python notebook: cleaning, merging, analysis, charting
├── Exam.sql             # SQL queries (reserved for query-based analysis)
├── outputs/
│   ├── clean_data.csv       # Final merged & cleaned dataset
│   ├── python_summary.csv   # Delay summary by service type
│   └── python_chart.png     # Monthly total delay chart
└── README.md
```

---

## 🎯 Objective

Delivery teams track a **promised delivery time** vs. an **actual delivery time** for each shipment. This project:

1. Loads and cleans raw `routes` and `deliveries` data
2. Merges them into a single analysis-ready dataset
3. Derives a `delay_days` metric (actual − promised, floored at 0)
4. Summarizes delay performance by **service type** (Express vs. Standard)
5. Visualizes total delay days by month
6. Exports clean data and summaries for reporting

---

## 🗂️ Data

| Dataset | Key Columns |
|---|---|
| `deliveries` | `record_id`, `month`, `route_id`, `hub`, `promised_days`, `actual_days` |
| `routes` | `route_id`, `route`, `service_type` |
| `Clean` (merged) | all of the above + `delay_days`, `month_sort` |

**Hubs covered:** Mumbai, Chennai, Delhi
**Service types:** Express, Standard
**Months covered:** Jan, Feb, Mar

---

## 🔬 Methodology

### 1. Load, Clean & Merge
- Load `routes.csv` and `deliveries.csv`
- Check and handle null values
- Convert `promised_days` / `actual_days` to numeric types
- Merge deliveries with routes on `route_id` (left join)

### 2. Derived Fields & Service-Type Analysis
- Compute `delay_days = max(actual_days - promised_days, 0)`
- Group by `service_type` to calculate:
  - **Total delay days**
  - **Delay incidence rate** (% of shipments delayed)

### 3. Charting & Export
- Aggregate total delay days by month
- Plot a bar chart of monthly delay trends
- Export the cleaned dataset and summary tables to `outputs/`

---

## 📊 Key Findings

Based on the pivot summary (`Sum of delay_days` by month × service type):

| Month | Express | Standard | Total |
|---|---|---|---|
| Jan | 1 | 7 | 8 |
| Feb | 3 | 6 | 9 |
| Mar | 8 | 9 | 17 |
| **Total** | **12** | **22** | **34** |

- **Standard service** consistently accrues more delay days than Express across every month.
- Delays are **trending upward** — March alone accounts for exactly half of the total delay burden.
- **Standard shipments are the priority area** for operational improvement.

---

## 🛠️ Tech Stack

- **Python 3** — `pandas`, `numpy`, `matplotlib`
- **Jupyter Notebook** for exploratory analysis
- **Excel** for source data, cleaning validation, and pivot-table cross-checks
- **SQL** for query-based analysis (see `Exam.sql`)

---

## ▶️ How to Run

```bash
# Install dependencies
pip install pandas numpy matplotlib

# Launch the notebook
jupyter notebook Exam_.ipynb
```

> **Note:** The notebook currently references local file paths (`C://Users//...`). Update `routes.csv` / `deliveries.csv` paths to your own environment before running, or point them at the equivalent sheets in `analysis_ex.xlsx`.

Running all cells will regenerate:
- `outputs/clean_data.csv`
- `outputs/python_summary.csv`
- `outputs/python_chart.png`

---

## 📌 Next Steps

- Populate `Exam.sql` with equivalent queries (delay calculation, service-type aggregation) for a SQL-based version of this analysis
- Add root-cause breakdown by **hub** in addition to service type
- Extend the dataset beyond Q1 (Jan–Mar) for trend reliability

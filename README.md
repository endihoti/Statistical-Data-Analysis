# Megaline Telecom Plan Analysis & Statistical Hypothesis Testing

## Project Overview
This project evaluates the commercial performance of two prepaid plans (**Surf** vs. **Ultimate**) for the telecom operator **Megaline**. Using a sample dataset of 500 clients from 2018, the analysis examines user behavior (minutes, SMS, data consumption) and calculates monthly individual revenue to determine which plan generates higher revenue for targeted advertising budget allocation.

Primary statistical hypothesis testing is conducted to evaluate revenue differences across plans and geographic regions (**NY-NJ** vs. **Other Regions**).

---

## Technical Highlights & Key Methodologies

* **Data Cleaning & Billing Logic Customization:**
  * Processed raw call, message, internet, and user logs (`5` CSV datasets).
  * Enforced Megaline rounding rules: rounded individual call durations up to the nearest minute (`np.ceil`), aggregated monthly data usage, and converted MB to GB (rounded up to the nearest integer).
  * Handled missing churn dates, converted dates to `datetime` objects, and extracted monthly features.
* **Monthly Aggregations & Revenue Calculation:**
  * Merged monthly user metrics (call duration, message counts, internet GB) into a unified dataset.
  * Implemented revenue calculation logic incorporating base plan fees ($20/mo Surf vs. $70/mo Ultimate) plus tiered overage charges for excess minutes ($0.03/min vs $0.01/min), texts ($0.03/msg vs $0.01/msg), and data ($10/GB vs $7/GB).
* **Exploratory Data Analysis (EDA) & Summary Statistics:**
  * Analyzed consumption distributions (mean, variance, standard deviation) across plans using histograms and boxplots.
  * Identified that **Surf** users frequently exceed data limits, driving significant overage revenue, whereas **Ultimate** users rarely exceed plan caps.
* **Statistical Hypothesis Testing ($\alpha = 0.05$):**
  * **Test 1 (Plan Revenue):** Evaluated whether average revenue differs between **Surf** and **Ultimate** plan users using a Two-Sample Independent $t$-test (`scipy.stats.ttest_ind` with `equal_var=False`).
  * **Test 2 (Geographic Revenue):** Evaluated whether average revenue from users in the **NY-NJ area** differs from other regions using a Two-Sample $t$-test.

---

## Dataset Overview

The analysis merges data across five primary tables:

| Dataset | Primary Features | Description |
| :--- | :--- | :--- |
| `megaline_users.csv` | `user_id`, `city`, `plan`, `reg_date`, `churn_date` | Client demographic & subscription metadata |
| `megaline_calls.csv` | `user_id`, `call_date`, `duration` | Individual call durations (rounded up per call) |
| `megaline_messages.csv` | `user_id`, `message_date` | Text message logs |
| `megaline_internet.csv` | `user_id`, `session_date`, `mb_used` | Web session data (rounded up monthly in GB) |
| `megaline_plans.csv` | `plan_name`, `usd_monthly_fee`, limits, overage rates | Plan terms and overage charges |

---

## Key Results & Business Insights

| Metric / Test | Surf Plan | Ultimate Plan | Key Takeaway |
| :--- | :---: | :---: | :--- |
| **Base Fee** | $20 / month | $70 / month | Ultimate has a $50 higher entry point |
| **Average Monthly Revenue** | Overages drive total | Near base fee ($70) | **Ultimate** brings in higher average total revenue per user |
| **Data Usage Limits** | 15 GB allowance | 30 GB allowance | Surf users regularly incur extra $10/GB fees |
| **Hypothesis 1 (Plans)** | $p$-value < 0.05 | Rejected $H_0$ | Statistically significant difference in revenue between Surf and Ultimate |
| **Hypothesis 2 (Region)** | $p$-value > 0.05 | Failed to Reject $H_0$ | No statistically significant revenue difference between NY-NJ and other regions |

> **Business Conclusion:** The **Ultimate** plan generates higher overall average revenue per user (ARPU). However, the **Surf** plan generates substantial overage revenue from data overages. The commercial department should focus advertising budget on attracting **Ultimate** plan subscribers for reliable revenue, while adjusting Surf plan messaging to highlight data tiers.

---

## Project Structure

```text
├── main.ipynb            # Jupyter notebook with data preparation, EDA, revenue calculations, and hypothesis tests
├── README.md             # Project documentation and summary
└── /datasets/            # Megaline calls, internet, messages, plans, and users CSV files

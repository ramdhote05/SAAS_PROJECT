# SaaS Customer & Subscription Analytics

A portfolio project exploring customer, subscription, revenue, churn, acquisition, and payment data for a synthetic SaaS business. The Python exploration and MySQL analysis are complete; the Power BI report is the next project phase.

> **Data notice:** All customer and business data in this repository is synthetic and intended for analytics practice. It does not describe a real company.

## Project status

- **Python:** Data exploration and cleaning notebooks are included.
- **MySQL:** The analysis notebook loads the data and creates analytical views for MRR, ARR, churn, CAC, and payment health.
- **Power BI:** Data has been loaded and the initial model relationships have been created or verified. Dashboard visuals and final report layout are still in progress.

## Repository contents

```text
.
├── 01_data_exploration.ipynb  # Explore and clean the source data
├── MRR.ipynb                  # MySQL analysis and analytical views
├── monthly_churn_rate.png     # Churn-rate visualization
└── data/
    ├── README.md              # Dataset details, joins, and modeling notes
    ├── *.csv                  # Source datasets and data dictionary
    └── cleaned/*.csv          # Cleaned datasets
```

The dataset covers 36 months (November 2023–October 2026) and includes 10,000 customers, 10,113 subscriptions, 109,303 payments, 146,190 usage records, 180 monthly marketing records, and 2,887 plan changes. See [`data/README.md`](data/README.md) for file descriptions, join keys, and additional modeling notes.

## Analysis workflow

1. Open `01_data_exploration.ipynb` in Jupyter and run the exploration and cleaning steps.
2. Load the CSV data into your MySQL `saas_analytics` database and run `MRR.ipynb` to create and inspect the analytical views.
3. Open the data in Power BI and verify the model before building report visuals.

The MySQL notebook uses database connection settings from a local `.env` file. Configure your own connection details locally; **never commit `.env` or credentials**. The repository `.gitignore` excludes `.env` and Jupyter checkpoint folders.

## Power BI model and next steps

The initial relationships to verify in Model view are:

| From | To | Expected cardinality |
| --- | --- | --- |
| `customers[customer_id]` | `subscriptions[customer_id]` | One-to-many |
| `subscriptions[subscription_id]` | `payments[subscription_id]` | One-to-many |
| `subscriptions[subscription_id]` | `vw_arr[subscription_id]` | One-to-one only if IDs are unique on both sides |

Avoid creating duplicate relationships. Keep the aggregated `vw_mrr`, `vw_churn`, `vw_cac`, and `vw_payment_health` views disconnected from the raw tables until their date fields and filtering needs are modeled deliberately.

Planned report work includes KPI cards for MRR, ARR, churn, customer count, CAC, and payment health; revenue and churn trends; acquisition-channel comparisons; and date/plan slicers. Save the Power BI report as a `.pbix` file locally. It is not included in this repository yet.

## Notes and limitations

- Normalize annual subscription prices as `price / 12` when calculating MRR.
- Active subscriptions have a blank `end_date`; cancelled subscriptions have an end date.
- Marketing spend and customer acquisition data are synthetic; CAC is a simplified spend-per-new-customer proxy.
- Payment failures do not automatically cancel a subscription. Define an explicit rule before interpreting payment failures as involuntary churn.
- Plan changes are recorded separately and do not rewrite the base subscription plan or price.

# SaaS Customer & Subscription Analytics — Synthetic Dataset

## Overview
Synthetic, relational dataset designed for a Python + SQL + Power BI capstone. It contains no real customer data.

**History:** 2023-11 to 2026-10 (36 months)  
**Customers:** 10,000  
**Subscriptions:** 10,113  
**Payments:** 109,303  
**Usage rows:** 146,190  
**Marketing rows:** 180 (5 channels × 36 months)  
**Plan changes:** 2,887

## Files
- `customers.csv` — one row per customer; acquisition channel and firmographic fields.
- `subscriptions.csv` — subscription lifecycle, plan, billing cycle, price, status and end date.
- `payments.csv` — recurring payment attempts with success, failed and refunded outcomes.
- `marketing_spend.csv` — monthly marketing spend for each acquisition channel.
- `usage_engagement.csv` — customer-month activity metrics.
- `plan_changes.csv` — upgrade/downgrade event history.
- `data_dictionary.csv` — column descriptions and join keys.

## Join keys
- `customers.customer_id` → `subscriptions.customer_id`
- `subscriptions.subscription_id` → `payments.subscription_id`
- `subscriptions.subscription_id` → `plan_changes.subscription_id`
- `customers.customer_id` → `usage_engagement.customer_id`
- `customers.acquisition_channel` + customer signup month can be compared with `marketing_spend.channel` + `marketing_spend.month`.

## Important modeling notes
- `subscriptions.price` is the contracted price for the listed plan and billing cycle. Annual plans are billed once per year (annual price is approximately 10 months of monthly list price).
- `payments.payment_status` is generated with approximately 4% failed, 1% refunded, and the remainder successful. Random outcomes are synthetic and are not intended to represent a real business.
- Active subscriptions have blank `end_date`; cancelled subscriptions have an end date.
- Signup volume trends upward with mild seasonality. Marketing spend also grows over time.
- Acquisition channels have different synthetic CAC assumptions and churn propensities. These are embedded in the data-generation process; they are not separate columns.
- `usage_engagement.csv` contains customer-month rows from signup through the observed lifecycle. Usage is synthetic and is not a clinical or operational measurement.
- Plan-change rows record events but do not rewrite the base `subscriptions.plan` or `subscriptions.price`; model changes as a separate event table when analyzing plan history.
- The data is intentionally imperfect enough for analytics exercises, but all IDs are synthetic.

## Suggested first analyses
1. Monthly recurring revenue (MRR) and annual recurring revenue (ARR). Normalize annual subscription price as `price / 12` for MRR.
2. Logo churn by month and acquisition channel.
3. Failed-payment rate and payment-related churn proxy.
4. Cohort retention by signup month.
5. CAC proxy by channel = monthly marketing spend / new customers attributed to that channel in the month. This is a simplified proxy, not a full attribution model.
6. Plan mix, upgrade/downgrade trends, usage engagement and support-ticket trends.

## Limitations
This is a synthetic teaching dataset, not observed company performance. The marketing spend is not calibrated to actual acquisition counts, and payment failures do not automatically cancel a subscription. For involuntary-churn analysis, define an explicit operational rule (for example, flag subscriptions with a failed payment and no later successful payment).

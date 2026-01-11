# 2. Billing & Cost Management (Detailed)

## Billing Accounts
Billing accounts are the foundation of payment and cost management in Google Cloud. They determine how you pay for GCP services and how costs are tracked and reported.

- **Definition:** Central entity for managing payments and invoices. Every billable GCP resource must be linked to a billing account.
- **Types:**
	- **Self-serve (Credit Card):** Pay-as-you-go, charged to a credit/debit card.
	- **Invoiced (Organization):** Monthly invoicing, typically for enterprises.
- **Project Association:**
	- A billing account can be linked to multiple projects.
	- Projects can be moved between billing accounts (useful for reorganizations or M&A).
- **Permissions:**
	- Billing Account Admin, Billing Account User, Billing Account Viewer roles control access.
- **gcloud Examples:**
	- List billing accounts:
		```sh
		gcloud beta billing accounts list
		```
	- Link a project to a billing account:
		```sh
		gcloud beta billing projects link PROJECT_ID --billing-account=BILLING_ACCOUNT_ID
		```
	- List projects linked to a billing account:
		```sh
		gcloud beta billing projects list --billing-account=BILLING_ACCOUNT_ID
		```

---

## Budgets & Alerts
Budgets and alerts help you monitor and control your GCP spending by setting cost thresholds and receiving notifications when you approach or exceed them.

- **Budgets:**
	- Set monthly, quarterly, or custom time period budgets for billing accounts or individual projects.
	- Can be based on total cost, specific services, or labels.
- **Alerts:**
	- Triggered when spending reaches a percentage of the budget (e.g., 50%, 90%, 100%).
	- Notifications sent via email or Pub/Sub (for automation or integration with ticketing systems).
- **Limitations:**
	- Budgets/alerts do NOT stop or suspend resources; they only notify.
	- Use automation (e.g., Cloud Functions) with Pub/Sub for enforcement if needed.
- **gcloud Examples:**
	- Create a budget (using gcloud alpha):
		```sh
		gcloud alpha billing budgets create \
			--billing-account=BILLING_ACCOUNT_ID \
			--display-name="My Budget" \
			--amount=1000USD \
			--threshold-rule=percent=0.5,percent=0.9,percent=1.0
		```
	- List budgets:
		```sh
		gcloud alpha billing budgets list --billing-account=BILLING_ACCOUNT_ID
		```

---

## Billing Export
Billing export allows you to analyze and report on your GCP costs by exporting detailed billing data to BigQuery or Google Sheets.

- **BigQuery Export:**
	- Exports detailed usage and cost data to a BigQuery dataset for advanced analysis, dashboards, and integration with BI tools.
	- Enables custom queries, cost breakdowns by project, service, label, etc.
- **Google Sheets Export:**
	- Exports summarized cost data to Google Sheets for simple reporting and sharing.
- **Setup:**
	- Must be configured in the Cloud Console (Billing > Billing Export).
	- Requires BigQuery dataset or Google Sheet destination.
- **gcloud Example:**
	- Enable BigQuery export for a billing account (must have a BigQuery dataset created):
		```sh
		gcloud beta billing export bigquery enable \
			--billing-account=BILLING_ACCOUNT_ID \
			--dataset=DATASET_ID \
			--project=PROJECT_ID
		```

---

## Cost Management Best Practices
- Regularly review budgets and set alerts for all billing accounts and critical projects.
- Use labels and folders to organize resources for granular cost tracking.
- Enable billing export to BigQuery for detailed analysis and anomaly detection.
- Assign least-privilege IAM roles for billing management.
- Use committed use contracts and sustained use discounts for cost optimization.

---

## Key Differences
- **Budgets/alerts** are proactive notifications; **billing export** is for analysis and reporting.
- **Billing accounts** are required for resource usage; **budgets/alerts/exports** are optional but highly recommended for cost control.

---

## Summary Table

| Feature         | Scope         | Use Case                        | Key Differences                | Example gcloud Command                        |
|-----------------|--------------|---------------------------------|-------------------------------|-----------------------------------------------|
| Billing Account | Org/Project  | Payment management              | Required for usage            | gcloud beta billing accounts list             |
| Budgets/Alerts  | Billing/Proj | Cost control, notifications     | No enforcement, just alerts   | gcloud alpha billing budgets create           |
| Billing Export  | Billing/Proj | Cost analysis/reporting         | Data to BigQuery/Sheets       | gcloud beta billing export bigquery enable    |

---

**Tip:** Always monitor your spending and set up automated alerts to avoid unexpected bills. Use BigQuery billing export for in-depth cost analysis and reporting.
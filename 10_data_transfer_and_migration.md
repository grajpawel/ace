# 10. Data Transfer & Migration (Detailed)

Google Cloud provides robust tools for migrating and transferring data from on-premises, other clouds, or within GCP. Below are the main services, their features, use cases, and relevant gcloud commands.

## Storage Transfer Service
Managed service for online data migration to Cloud Storage. Supports large-scale, multi-terabyte migrations with scheduling, incremental sync, and error handling.

- **Features:**
	- Supports scheduled, incremental, and one-time transfers.
	- Sources: AWS S3, Azure Blob Storage, HTTP/HTTPS, on-premises file systems, other GCP buckets.
	- Handles large-scale, multi-terabyte migrations with error handling and reporting.
	- Supports filtering, bandwidth limits, and notifications.
- **Best Practices:**
	- Use for bulk, managed transfers (not ad-hoc uploads).
	- Schedule transfers during off-peak hours for minimal impact.
	- Use incremental sync for ongoing migrations.
- **gcloud Examples:**
	- Create a transfer job from AWS S3 to Cloud Storage:
		```sh
		gcloud transfer jobs create s3://SOURCE_BUCKET gs://DEST_BUCKET --source-agent-pool=POOL_ID --description="S3 to GCS migration"
		```
	- Create a transfer job between GCP buckets:
		```sh
		gcloud transfer jobs create gs://SOURCE_BUCKET gs://DEST_BUCKET --description="GCS to GCS migration"
		```
	- List transfer jobs:
		```sh
		gcloud transfer jobs list
		```

---

## Other Data Transfer & Migration Tools

- **gsutil:** Command-line tool for ad-hoc file and directory transfers to/from Cloud Storage.
	- Example:
		```sh
		gsutil -m cp -r ./local-folder gs://my-bucket
		```
- **Transfer Appliance:** Physical device for offline, petabyte-scale data transfer from on-premises to GCP.
- **BigQuery Data Transfer Service:** Automates data movement from SaaS apps (e.g., Google Ads, YouTube, Salesforce) to BigQuery.

---

## Key Differences
- **Storage Transfer Service:** Bulk, managed, scheduled, and incremental transfers for large-scale migrations.
- **gsutil:** Ad-hoc, manual file transfers.
- **Transfer Appliance:** Offline, physical device for massive data migrations.
- **BigQuery Data Transfer Service:** Automated SaaS-to-BigQuery data ingestion.

---

## Summary Table

| Feature         | Use Case                | Key Differences                | Example Command                                 |
|-----------------|-------------------------|-------------------------------|-------------------------------------------------|
| Storage Transfer| Data migration          | Managed, scheduled, incremental| gcloud transfer jobs create                     |
| gsutil          | Ad-hoc file transfer    | Manual, CLI, flexible          | gsutil cp                                       |
| Transfer Appliance| Offline migration      | Physical, petabyte-scale       | N/A (order via Console)                         |
| BQ Data Transfer| SaaS to BigQuery        | Automated, scheduled           | gcloud bq mktransfer                            |

---

**Tip:** Use Storage Transfer Service for large, repeatable migrations. Use gsutil for quick, manual transfers. For petabyte-scale offline migrations, consider Transfer Appliance. For SaaS data, use BigQuery Data Transfer Service.
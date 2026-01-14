# 14. Data Analytics & Processing (Detailed)

Google Cloud offers a comprehensive suite of data analytics and processing services for batch, stream, and real-time data processing. Below are the main services, their features, use cases, and relevant gcloud commands.

## Dataflow
Fully managed service for stream and batch data processing based on Apache Beam. Unified programming model for ETL, analytics, and ML pipelines.

- **Features:**
	- Supports both batch and streaming data processing.
	- Auto-scaling based on workload.
	- Exactly-once processing semantics.
	- Integration with Pub/Sub, BigQuery, Cloud Storage, Bigtable.
	- Serverless, no cluster management.
- **Best Practices:**
	- Use for ETL pipelines, real-time analytics, and data transformations.
	- Use templates for common patterns (Pub/Sub to BigQuery, etc.).
	- Monitor job metrics for performance tuning.
- **gcloud Examples:**
	- Run a Dataflow job from a template:
		```sh
		gcloud dataflow jobs run my-job \
			--gcs-location=gs://dataflow-templates/latest/PubSub_to_BigQuery \
			--region=us-central1 \
			--parameters inputTopic=projects/PROJECT_ID/topics/my-topic,outputTableSpec=PROJECT_ID:DATASET.TABLE
		```
	- List Dataflow jobs:
		```sh
		gcloud dataflow jobs list --region=us-central1
		```
	- Describe a job:
		```sh
		gcloud dataflow jobs describe JOB_ID --region=us-central1
		```

---

## Dataproc
Managed Apache Hadoop, Spark, Hive, and Pig clusters for big data processing. Fast cluster creation and cost-effective ephemeral clusters.

- **Features:**
	- Clusters spin up in ~90 seconds.
	- Supports standard Hadoop/Spark ecosystem tools.
	- Autoscaling for cost optimization.
	- Integration with Cloud Storage (HDFS replacement), BigQuery, Bigtable.
	- Supports preemptible VMs for cost savings.
- **Best Practices:**
	- Use ephemeral clusters for jobs (create, run, delete).
	- Store data in Cloud Storage, not on cluster (separate compute/storage).
	- Use autoscaling and preemptible workers for cost optimization.
- **gcloud Examples:**
	- Create a Dataproc cluster:
		```sh
		gcloud dataproc clusters create my-cluster \
			--region=us-central1 \
			--zone=us-central1-a \
			--master-machine-type=n1-standard-2 \
			--worker-machine-type=n1-standard-2 \
			--num-workers=2
		```
	- Submit a Spark job:
		```sh
		gcloud dataproc jobs submit spark \
			--cluster=my-cluster \
			--region=us-central1 \
			--jar=gs://my-bucket/my-spark-job.jar
		```
	- List clusters:
		```sh
		gcloud dataproc clusters list --region=us-central1
		```
	- Delete a cluster:
		```sh
		gcloud dataproc clusters delete my-cluster --region=us-central1
		```

---

## Cloud Composer
Fully managed workflow orchestration service built on Apache Airflow. Schedule, monitor, and manage complex data pipelines.

- **Features:**
	- Python-based workflow definitions (DAGs).
	- Integration with GCP services and external systems.
	- Monitoring, logging, and alerting built-in.
	- Managed Airflow environment with automatic upgrades.
- **Best Practices:**
	- Use for complex, multi-step data pipelines and ETL orchestration.
	- Store DAGs in Cloud Storage for version control.
	- Monitor DAG runs for failures and performance issues.
- **gcloud Examples:**
	- Create a Composer environment:
		```sh
		gcloud composer environments create my-environment \
			--location=us-central1 \
			--python-version=3
		```
	- List environments:
		```sh
		gcloud composer environments list --locations=us-central1
		```
	- Update environment variables:
		```sh
		gcloud composer environments update my-environment \
			--location=us-central1 \
			--update-env-variables=KEY=VALUE
		```

---

## Data Fusion
Fully managed, cloud-native data integration service for building ETL/ELT pipelines using a graphical interface. No-code/low-code approach.

- **Features:**
	- Visual pipeline designer with 150+ connectors.
	- Pre-built transformations and plugins.
	- Supports batch and real-time data integration.
	- Integrates with BigQuery, Cloud Storage, databases, SaaS apps.
- **Best Practices:**
	- Use for citizen data engineers or rapid pipeline development.
	- Leverage pre-built connectors for common sources/targets.
- **Console-Based (Limited gcloud support):**
	- Most Data Fusion operations are performed via the Cloud Console.
	- List instances:
		```sh
		gcloud data-fusion instances list --location=us-central1
		```

---

## Looker & Looker Studio (formerly Data Studio)
Business intelligence and data visualization platforms.

- **Looker:** Enterprise BI platform with semantic modeling layer (LookML).
- **Looker Studio:** Free, self-service dashboarding and reporting tool.
- **Best Practices:**
	- Use Looker Studio for quick dashboards and reports.
	- Use Looker for enterprise-scale BI with governance.
	- Connect to BigQuery, Cloud SQL, Google Sheets, and more.

---

## Key Differences
- **Dataflow:** Stream/batch processing, Apache Beam, serverless, ETL/analytics.
- **Dataproc:** Managed Hadoop/Spark, ephemeral clusters, ecosystem tools.
- **Composer:** Workflow orchestration, Apache Airflow, DAG-based.
- **Data Fusion:** Visual ETL, no-code, pre-built connectors.
- **Looker/Studio:** BI and visualization, dashboards, reporting.

---

## Summary Table

| Service      | Type              | Use Case                  | Key Differences            | Example gcloud Command                 |
|--------------|-------------------|---------------------------|----------------------------|----------------------------------------|
| Dataflow     | Stream/batch      | ETL, real-time analytics  | Apache Beam, serverless    | gcloud dataflow jobs run               |
| Dataproc     | Hadoop/Spark      | Big data, Spark jobs      | Clusters, ephemeral        | gcloud dataproc clusters create        |
| Composer     | Orchestration     | Workflow management       | Apache Airflow, DAGs       | gcloud composer environments create    |
| Data Fusion  | Visual ETL        | No-code data integration  | GUI-based, connectors      | gcloud data-fusion instances list      |
| Looker Studio| BI/Visualization  | Dashboards, reports       | Free, self-service         | N/A (Console-based)                    |

---

**Tip:** Use Dataflow for unified stream/batch processing, Dataproc for Hadoop/Spark workloads, Composer for complex orchestration, and Data Fusion for visual ETL pipelines. Choose the right tool based on your team's skills and pipeline complexity.

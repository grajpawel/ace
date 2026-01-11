# 4. Storage & Databases (Detailed)

Google Cloud provides a variety of storage and database services to meet different data needs, from unstructured object storage to globally distributed relational databases. Below are the main services, their features, use cases, and relevant gcloud commands.

## Cloud Storage
Object storage for unstructured data such as files, images, backups, and media. Highly durable and available, with multiple storage classes for cost optimization.

- **Storage Classes:**
	- **Standard:** Frequent access, low latency.
	- **Nearline:** Infrequent access (≥30 days), lower cost.
	- **Coldline:** Rare access (≥90 days), archival.
	- **Archive:** Long-term, lowest cost, rarely accessed.
- **Features:**
	- Lifecycle rules for automatic data management (e.g., auto-delete, class transition).
	- Retention policies for compliance and legal hold.
	- Object versioning and uniform bucket-level access.
- **Best Practices:**
	- Use lifecycle rules to manage storage costs.
	- Enable object versioning for critical data.
	- Use signed URLs for secure, temporary access.
- **gcloud Examples:**
	- Create a bucket:
		```sh
		gcloud storage buckets create gs://my-bucket --location=us-central1 --storage-class=STANDARD
		```
	- List buckets:
		```sh
		gcloud storage buckets list
		```
	- Set a lifecycle rule:
		```sh
		gcloud storage buckets update gs://my-bucket --lifecycle-file=rule.json
		```

---

## Persistent Disks
Block storage for Compute Engine VMs. Provides high performance and durability for VM workloads.

- **Types:**
	- **Standard (HDD):** Cost-effective, good for sequential I/O.
	- **SSD:** High performance, low latency, good for databases and high IOPS workloads.
- **Locations:**
	- **Zonal:** Single zone, lower cost.
	- **Regional:** Replicated across two zones for high availability.
- **Best Practices:**
	- Use regional disks for critical workloads.
	- Take regular snapshots for backup and disaster recovery.
- **gcloud Examples:**
	- Create a standard disk:
		```sh
		gcloud compute disks create my-disk --size=100GB --zone=us-central1-a --type=pd-standard
		```
	- Create a regional SSD disk:
		```sh
		gcloud compute disks create my-regional-disk --size=100GB --type=pd-ssd --region=us-central1 --replica-zones=us-central1-a,us-central1-b
		```
	- List disks:
		```sh
		gcloud compute disks list
		```

---

## Cloud SQL
Managed relational database service supporting MySQL, PostgreSQL, and SQL Server. Handles backups, replication, patching, and failover.

- **Features:**
	- Automated backups, point-in-time recovery.
	- High availability with regional instances.
	- Vertical scaling (CPU, RAM, disk).
- **Best Practices:**
	- Enable automated backups and high availability.
	- Use private IP for secure connections.
- **gcloud Examples:**
	- Create a PostgreSQL instance:
		```sh
		gcloud sql instances create my-postgres --database-version=POSTGRES_15 --tier=db-custom-2-7680 --region=us-central1
		```
	- List SQL instances:
		```sh
		gcloud sql instances list
		```
	- Export a database:
		```sh
		gcloud sql export sql my-postgres gs://my-bucket/export.sql --database=mydb
		```

---

## Firestore
NoSQL document database for mobile, web, and IoT apps. Supports real-time updates and offline sync.

- **Modes:**
	- **Native mode:** For new apps, scalable, real-time.
	- **Datastore mode:** For legacy Datastore apps.
- **Best Practices:**
	- Use security rules to protect data.
	- Structure data for efficient queries.
- **gcloud Examples:**
	- Create a Firestore database (Native mode):
		```sh
		gcloud firestore databases create --region=us-central1
		```
	- List Firestore indexes:
		```sh
		gcloud firestore indexes composite list
		```

---

## Bigtable
Wide-column NoSQL database for high-throughput, low-latency workloads (e.g., time series, IoT, analytics).

- **Features:**
	- Massive scalability, petabyte-scale.
	- Single-digit millisecond latency.
	- Integrates with Dataflow, BigQuery, and more.
- **Best Practices:**
	- Design row keys for even data distribution.
	- Use replication for high availability.
- **gcloud Examples:**
	- Create a Bigtable instance:
		```sh
		gcloud bigtable instances create my-instance --cluster=my-cluster --cluster-zone=us-central1-b --display-name="My Bigtable" --instance-type=PRODUCTION
		```
	- List Bigtable instances:
		```sh
		gcloud bigtable instances list
		```

---

## BigQuery
Serverless, highly scalable data warehouse for analytics and business intelligence.

- **Features:**
	- SQL-based analytics on large datasets.
	- Pay-per-query or flat-rate pricing.
	- Integrates with Data Studio, Looker, and AI Platform.
- **Best Practices:**
	- Partition and cluster tables for performance and cost savings.
	- Use authorized views for data security.
- **gcloud Examples:**
	- Create a dataset:
		```sh
		gcloud bigquery datasets create my_dataset
		```
	- List datasets:
		```sh
		gcloud bigquery datasets list
		```
	- Run a query:
		```sh
		gcloud bigquery query 'SELECT COUNT(*) FROM `my_project.my_dataset.my_table`'
		```

---

## Cloud Spanner
Globally distributed, strongly consistent relational database. Combines the benefits of relational structure with horizontal scalability.

- **Features:**
	- Global transactions, high availability.
	- Automatic sharding and replication.
- **Best Practices:**
	- Use for applications requiring global consistency and high availability.
	- Design schemas for scalability.
- **gcloud Examples:**
	- Create a Spanner instance:
		```sh
		gcloud spanner instances create my-instance --config=regional-us-central1 --description="My Spanner" --nodes=1
		```
	- List Spanner instances:
		```sh
		gcloud spanner instances list
		```

---

## AlloyDB
Fully managed, PostgreSQL-compatible database designed for enterprise workloads requiring high performance and availability.

- **Features:**
	- PostgreSQL compatibility, high throughput.
	- Automated backups, failover, and scaling.
- **Best Practices:**
	- Use for modernizing PostgreSQL workloads.
	- Enable high availability for production.
- **gcloud Examples:**
	- Create an AlloyDB cluster:
		```sh
		gcloud alloydb clusters create my-cluster --region=us-central1 --network=default
		```
	- List AlloyDB clusters:
		```sh
		gcloud alloydb clusters list --region=us-central1
		```

---

## Key Differences
- **Cloud Storage:** Object storage, unstructured data, multiple classes.
- **Persistent Disks:** Block storage for VMs, zonal/regional, SSD/HDD.
- **Cloud SQL:** Managed RDBMS, vertical scaling, automated backups.
- **Firestore:** NoSQL, real-time, scalable, mobile/web/IoT.
- **Bigtable:** NoSQL, wide-column, analytics, high throughput.
- **BigQuery:** Analytics, serverless, SQL, scalable.
- **Spanner:** Global RDBMS, horizontal scaling, strong consistency.
- **AlloyDB:** PostgreSQL, high performance, managed, enterprise-ready.

---

## Summary Table

| Service         | Type         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|--------------|-------------------------|-------------------------------|-----------------------------------------------|
| Cloud Storage   | Object       | Files, backups, media   | Classes, lifecycle, retention  | gcloud storage buckets create                 |
| Persistent Disk | Block        | VM disks                | Zonal/regional, SSD/HDD        | gcloud compute disks create                   |
| Cloud SQL       | RDBMS        | Apps, websites          | Managed, backups, failover     | gcloud sql instances create                   |
| Firestore       | NoSQL        | Mobile, IoT, web        | Real-time, scalable            | gcloud firestore databases create             |
| Bigtable        | NoSQL        | Analytics, IoT          | Wide-column, high throughput   | gcloud bigtable instances create              |
| BigQuery        | Data warehouse| Analytics, BI          | Serverless, SQL, scalable      | gcloud bigquery datasets create               |
| Spanner         | RDBMS        | Global apps, finance    | Horizontal scaling, global     | gcloud spanner instances create               |
| AlloyDB         | RDBMS        | Enterprise, PostgreSQL  | High perf, managed, compatible | gcloud alloydb clusters create                |

---

**Tip:** Choose the storage or database service that best matches your data type, access patterns, and scalability needs. Use IAM and encryption for security, and automate backups for disaster recovery.
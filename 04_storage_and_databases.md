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
	- **CORS (Cross-Origin Resource Sharing):** Configure buckets to allow web apps from different origins to access resources.
	- **Signed URLs:** Time-limited URLs for secure, temporary access to objects without authentication.
	- **Customer-managed encryption keys (CMEK):** Use Cloud KMS keys for encryption.
- **Best Practices:**
	- Use lifecycle rules to manage storage costs.
	- Enable object versioning for critical data.
	- Use signed URLs for secure, temporary access.
	- Configure CORS for web applications accessing Cloud Storage.
	- Use CMEK for sensitive data requiring customer-controlled encryption.
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

## Local SSDs (Important IOPS Numbers for Exam!)
Physically attached to the VM host. Provides extremely high IOPS and low latency but data is ephemeral (lost on VM stop/delete).

- **Key Specifications:**
	- **Size:** 375 GB per local SSD (fixed size)
	- **Max per VM:** Up to 24 local SSDs (9 TB total)
	- **IOPS (Random Read):** Up to **680,000 IOPS** per local SSD
	- **IOPS (Random Write):** Up to **360,000 IOPS** per local SSD
	- **Throughput (Read):** Up to 2,650 MB/s per local SSD
	- **Throughput (Write):** Up to 1,400 MB/s per local SSD
	- **Latency:** Sub-millisecond (< 1 ms)
- **Exam-Critical Numbers:**
	- ✅ **680,000 read IOPS per local SSD** (frequently tested)
	- ✅ **360,000 write IOPS per local SSD** (frequently tested)
	- ✅ **375 GB fixed size** per local SSD
	- ✅ **Data is ephemeral** (lost on VM stop, terminate, or host maintenance)
- **Interface Types:**
	- **NVMe:** Default, highest performance
	- **SCSI:** Legacy, lower performance than NVMe
- **Use Cases:**
	- High-performance databases (temporary data)
	- Caching layers
	- Scratch space for data processing
	- Real-time analytics
	- Gaming servers (session data)
- **Limitations:**
	- Data is NOT persistent (ephemeral storage)
	- Cannot be detached and reattached to different VMs
	- Lost on VM stop, instance termination, or host maintenance
	- No snapshots or backups (must implement your own)
	- Cannot be used with some machine types (check compatibility)
- **Best Practices:**
	- Use for temporary, high-performance workloads only
	- Implement replication/backup at application level
	- Use RAID 0 for combined throughput across multiple SSDs
	- Use NVMe interface for best performance
	- Set VM to "TERMINATE" on host maintenance (default behavior)
- **Common Exam Scenarios:**
	- **"Need highest IOPS possible"** → Local SSD (680K read IOPS)
	- **"Database with temporary data, extreme performance"** → Local SSD
	- **"Data must survive VM stop"** → Persistent Disk (NOT local SSD)
	- **"Need 1 million IOPS"** → Multiple local SSDs (2x SSDs = 1.36M read IOPS)
- **gcloud Examples:**
	- Create VM with local SSD (NVMe):
		```sh
		gcloud compute instances create my-vm \
			--zone=us-central1-a \
			--machine-type=n2-standard-4 \
			--local-ssd=interface=nvme
		```
	- Create VM with multiple local SSDs:
		```sh
		gcloud compute instances create my-vm \
			--zone=us-central1-a \
			--machine-type=n2-standard-8 \
			--local-ssd=interface=nvme \
			--local-ssd=interface=nvme \
			--local-ssd=interface=nvme
		```
	- Create VM with SCSI local SSD:
		```sh
		gcloud compute instances create my-vm \
			--zone=us-central1-a \
			--local-ssd=interface=scsi
		```

**Comparison Table - Persistent Disk vs Local SSD:**

| Feature | Persistent Disk (SSD) | Local SSD |
|---------|----------------------|------------|
| **Max IOPS (Read)** | ~100,000 IOPS | **680,000 IOPS** per SSD |
| **Max IOPS (Write)** | ~100,000 IOPS | **360,000 IOPS** per SSD |
| **Persistence** | Data survives VM stop | **Ephemeral** (data lost) |
| **Size** | Flexible (10 GB - 64 TB) | Fixed **375 GB** per SSD |
| **Snapshots** | Yes | No |
| **Detach/Reattach** | Yes | No |
| **Best For** | Durable storage, databases | Temporary, ultra-high IOPS |
| **Latency** | Low (~1-2 ms) | **Sub-millisecond** |

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

## Database Migration Service (DMS)
Serverless service for migrating databases to Cloud SQL with minimal downtime.

- **Supported Sources:**
	- MySQL, PostgreSQL, SQL Server, Oracle.
	- On-premises, AWS RDS, Azure Database, other cloud providers.
- **Features:**
	- Continuous data replication for minimal downtime.
	- Schema and data migration.
	- Automated validation and monitoring.
- **Best Practices:**
	- Test migrations in non-production first.
	- Monitor replication lag during migration.
	- Use Cloud VPN or Interconnect for secure connectivity.
- **gcloud Examples:**
	- Create a migration job:
		```sh
		gcloud database-migration migration-jobs create my-migration \
			--region=us-central1 \
			--type=CONTINUOUS \
			--source=SOURCE_CONNECTION_PROFILE \
			--destination=DESTINATION_CONNECTION_PROFILE
		```
	- List migration jobs:
		```sh
		gcloud database-migration migration-jobs list --region=us-central1
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
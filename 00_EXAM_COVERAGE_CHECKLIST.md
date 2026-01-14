# ACE Exam Coverage Checklist

This checklist maps your notes to the official Google Associate Cloud Engineer exam guide to ensure complete coverage.

## Exam Domains & Coverage Status

### 1. Setting up a cloud solution environment (~17% of exam)

#### 1.1 Setting up cloud projects and accounts
- [x] Resource hierarchy (Organization, Folders, Projects, Resources) - File 01
- [x] Creating and managing projects - File 01
- [x] Enabling APIs and services - File 01
- [x] Billing accounts and linking to projects - File 02
- [x] Installing and configuring Cloud SDK (gcloud) - File 01, 11
- [x] Cloud Shell usage - File 01

#### 1.2 Managing billing configuration
- [x] Billing accounts (self-serve vs invoiced) - File 02
- [x] Budgets and alerts - File 02
- [x] Billing export (BigQuery, Google Sheets) - File 02
- [x] Cost allocation using labels - File 12

#### 1.3 Installing and configuring CLI
- [x] gcloud init and configuration - File 11
- [x] Setting default project and region/zone - File 11
- [x] Using multiple configurations - File 11

---

### 2. Planning and configuring a cloud solution (~19% of exam)

#### 2.1 Planning and estimating using Pricing Calculator
- [x] Pricing Calculator usage - File 12
- [x] Cost optimization strategies - File 17

#### 2.2 Planning and configuring compute resources
- [x] Compute Engine VM options (machine types, images, disk) - File 03
- [x] Preemptible/Spot VMs - File 03
- [x] Custom machine types - File 03
- [x] GKE (Standard, Autopilot, regional, zonal) - File 03
- [x] App Engine (Standard, Flexible) - File 03
- [x] Cloud Run - File 03
- [x] Cloud Functions - File 03
- [x] Managed instance groups (autoscaling, autohealing) - File 03
- [x] Instance templates - File 03
- [x] Startup scripts and metadata - File 03
- [x] Container-Optimized OS - File 03
- [x] Shielded VMs - File 03
- [x] Confidential VMs - File 03
- [x] Sole-tenant nodes - File 03

#### 2.3 Planning and configuring data storage
- [x] Cloud Storage (classes, lifecycle, versioning) - File 04
- [x] Persistent Disks (SSD, HDD, regional, zonal) - File 04
- [x] Cloud SQL (MySQL, PostgreSQL, SQL Server, HA) - File 04
- [x] Firestore (Native, Datastore mode) - File 04
- [x] Bigtable - File 04
- [x] Cloud Spanner - File 04
- [x] BigQuery - File 04
- [x] Filestore - File 16
- [x] Memorystore (Redis, Memcached) - File 16
- [x] AlloyDB - File 04

#### 2.4 Planning and configuring network resources
- [x] VPC (default vs custom, auto vs custom subnets) - File 05
- [x] Subnets and IP addressing - File 05
- [x] Firewall rules - File 05
- [x] Routes - File 05
- [x] Load balancing (HTTP(S), TCP/UDP, SSL Proxy, Network) - File 06
- [x] Cloud CDN - File 06
- [x] Cloud Armor - File 06
- [x] VPN (Classic, HA VPN) - File 05
- [x] Cloud Interconnect (Dedicated, Partner) - File 05
- [x] VPC peering - File 05
- [x] Shared VPC - File 05
- [x] Cloud DNS (public, private zones) - File 05
- [x] Cloud NAT - File 05
- [x] Private Google Access - File 05
- [x] Private Service Connect - File 05
- [x] VPC Flow Logs - File 05
- [x] Packet Mirroring - File 05

---

### 3. Deploying and implementing a cloud solution (~27% of exam)

#### 3.1 Deploying and implementing Compute Engine
- [x] Creating VMs (gcloud, Console) - File 03
- [x] Creating instance templates - File 03
- [x] Creating managed instance groups - File 03
- [x] Generating/uploading SSH keys - File 03
- [x] Configuring preemptible/spot VMs - File 03
- [x] Installing Ops Agent for monitoring - File 08

#### 3.2 Deploying and implementing GKE
- [x] Creating GKE clusters - File 03
- [x] Deploying containerized applications (kubectl) - File 11
- [x] Configuring monitoring and logging - File 08
- [x] Workload Identity - File 07

#### 3.3 Deploying and implementing Cloud Run
- [x] Deploying applications - File 03
- [x] Configuring scaling and concurrency - File 03
- [x] Cloud Run Jobs - File 18

#### 3.4 Deploying and implementing Cloud Functions
- [x] Creating functions (event-driven, HTTP) - File 03
- [x] Configuring triggers (Pub/Sub, Cloud Storage, HTTP) - File 03, 13

#### 3.5 Deploying and implementing data solutions
- [x] Initializing databases (Cloud SQL, Firestore, BigQuery) - File 04
- [x] Loading data (BigQuery, Cloud SQL, Firestore) - File 04
- [x] Database Migration Service - File 04
- [x] Cloud Storage bucket creation and configuration - File 04
- [x] Retention policies and lifecycle management - File 04

#### 3.6 Deploying and implementing networking resources
- [x] Creating VPCs and subnets - File 05
- [x] Creating ingress/egress firewall rules - File 05
- [x] Creating VPN connections - File 05
- [x] Creating load balancers - File 06
- [x] Creating Cloud NAT - File 05

#### 3.7 Deploying solutions using Cloud Marketplace
- [x] Browsing and deploying solutions - File 09

#### 3.8 Deploying application infrastructure using Deployment Manager or Terraform
- [x] Deployment Manager - File 09
- [x] Terraform basics - File 09

---

### 4. Ensuring successful operation of a cloud solution (~20% of exam)

#### 4.1 Managing Compute Engine resources
- [x] Managing instance lifecycle (start, stop, delete) - File 03
- [x] SSH/RDP to instances - File 03
- [x] Attaching/detaching disks - File 04
- [x] Viewing VM serial port output - File 03
- [x] Snapshots and images - File 04
- [x] Instance groups management - File 03
- [x] Live migration - File 03

#### 4.2 Managing GKE resources
- [x] Viewing cluster and workload details - File 03, 11
- [x] Viewing pod logs - File 11
- [x] Cluster management (node pools, autoscaling) - File 03

#### 4.3 Managing Cloud Run resources
- [x] Adjusting traffic splitting - File 03
- [x] Viewing and managing revisions - File 03

#### 4.4 Managing storage and database solutions
- [x] Managing Cloud Storage (moving objects, setting lifecycle) - File 04, 10
- [x] Managing Cloud SQL (backups, restores, connections) - File 04
- [x] Managing Firestore (import/export, queries) - File 04
- [x] Managing BigQuery (queries, datasets, tables) - File 04

#### 4.5 Managing networking resources
- [x] Adding/modifying subnets - File 05
- [x] Adding/modifying firewall rules - File 05
- [x] Viewing and managing routes - File 05
- [x] Administering VPNs - File 05
- [x] Managing load balancers - File 06

#### 4.6 Monitoring and logging
- [x] Cloud Monitoring (dashboards, metrics, alerts) - File 08
- [x] Cloud Logging (log viewing, filtering, sinks) - File 08
- [x] Cloud Trace - File 08
- [x] Cloud Profiler - File 08
- [x] Cloud Debugger - File 08
- [x] Error Reporting - File 08
- [x] Uptime checks - File 08
- [x] Health checks - File 08
- [x] Ops Agent installation - File 08
- [x] SLOs and SLIs - File 08, 12

---

### 5. Configuring access and security (~17% of exam)

#### 5.1 Managing IAM
- [x] IAM roles (primitive, predefined, custom) - File 07
- [x] Viewing and assigning IAM roles - File 07
- [x] Defining custom IAM roles - File 07
- [x] Managing service accounts - File 07
- [x] Service account keys - File 07
- [x] Workload Identity - File 07
- [x] IAM Conditions - File 07
- [x] Cloud Identity - File 07

#### 5.2 Managing service accounts
- [x] Creating and managing service accounts - File 07
- [x] Granting roles to service accounts - File 07
- [x] Managing service account keys - File 07
- [x] Using service accounts in applications - File 07

#### 5.3 Viewing audit logs
- [x] Audit log types (Admin Activity, Data Access, System Event) - File 07
- [x] Viewing and filtering audit logs - File 07
- [x] Exporting audit logs - File 07
- [x] Access Transparency - File 07

#### 5.4 Managing security
- [x] Secret Manager - File 15
- [x] Cloud KMS (key management, CMEK) - File 15
- [x] VPC Service Controls - File 15
- [x] Binary Authorization - File 15
- [x] Security Command Center - File 15
- [x] Organization Policy Service - File 15
- [x] Web Security Scanner - File 15
- [x] Shielded VMs - File 03
- [x] Confidential VMs - File 03

---

## Additional Important Topics Covered

### Messaging & Integration
- [x] Cloud Pub/Sub - File 13
- [x] Eventarc - File 13
- [x] Cloud Tasks - File 13
- [x] Workflows - File 13
- [x] Cloud Scheduler - File 18

### Data Analytics
- [x] Dataflow - File 14
- [x] Dataproc - File 14
- [x] Cloud Composer - File 14
- [x] Data Fusion - File 14

### CI/CD & DevOps
- [x] Cloud Build - File 09
- [x] Cloud Source Repositories - File 09
- [x] Cloud Deploy - File 18
- [x] Artifact Registry - File 16
- [x] Container Registry (legacy) - File 16

### AI/ML
- [x] Vertex AI - File 16
- [x] Pre-trained APIs (Vision, Language, Translation, Speech) - File 16

### Migration & Transfer
- [x] Storage Transfer Service - File 10
- [x] Database Migration Service - File 04
- [x] Transfer Appliance - File 10

### Additional Services
- [x] API Gateway - File 18
- [x] Cloud Endpoints - File 18
- [x] Cloud Asset Inventory - File 18
- [x] Recommender API - File 18

### Core Concepts
- [x] Regions and Zones - File 12
- [x] High Availability and Scalability - File 12
- [x] Labels and Tagging - File 12
- [x] Quotas - File 01
- [x] SLA/SLO/SLI - File 12
- [x] Resource location types (multi-region, regional, zonal) - File 12
- [x] Free Tier and Trial - File 18
- [x] Shared Responsibility Model - File 18

### Exam Strategy
- [x] Migration strategies - File 17
- [x] Disaster recovery patterns - File 17
- [x] Cost optimization - File 17
- [x] Security best practices - File 17
- [x] Troubleshooting patterns - File 17
- [x] Service selection decision trees - File 17
- [x] Exam tips and time management - File 17

---

## Key Command Patterns Covered

- [x] gcloud - File 11
- [x] kubectl - File 11
- [x] gsutil - File 11
- [x] bq - File 11
- [x] Cloud Console basics - File 12

---

## Coverage Summary

**Total Coverage: 100%**

All official exam domains are covered comprehensively across your 18 files. The notes include:

✅ All core services (Compute, Storage, Networking, Security)
✅ All management tools (IAM, Monitoring, Logging, Billing)
✅ All deployment methods (Console, gcloud, Terraform, Deployment Manager)
✅ All key concepts (Regions, HA, Quotas, SLAs)
✅ Advanced topics (Workload Identity, VPC Service Controls, Binary Authorization)
✅ Exam strategy and troubleshooting patterns
✅ Hands-on gcloud command examples throughout

**Your notes are exam-ready!** 

## Recommended Final Study Steps:

1. **Practice hands-on labs** - Create resources using gcloud commands from your notes
2. **Take practice exams** - Identify any weak areas
3. **Review exam guide** - Cross-reference with this checklist
4. **Memorize key differences** - Review all "Key Differences" and "Summary Table" sections
5. **Focus on scenarios** - Review File 17 for common exam scenarios
6. **Review gcloud syntax** - Practice common command patterns from File 11

Good luck on your exam!

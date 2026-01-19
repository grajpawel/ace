# ACE Exam Coverage Checklist

This checklist maps your notes to the official Google Associate Cloud Engineer exam guide to ensure complete coverage.

## Exam Domains & Coverage Status

### 1. Setting up a cloud solution environment (~17% of exam)

#### 1.1 Setting up cloud projects and accounts
- [x] Resource hierarchy (Organization, Folders, Projects, Resources) - [File 01](01_resource_hierarchy_and_project_setup.md)
- [x] Creating and managing projects - [File 01](01_resource_hierarchy_and_project_setup.md)
- [x] Enabling APIs and services - [File 01](01_resource_hierarchy_and_project_setup.md)
- [x] Billing accounts and linking to projects - [File 02](02_billing_and_cost_management.md)
- [x] Installing and configuring Cloud SDK (gcloud) - [File 01](01_resource_hierarchy_and_project_setup.md), [File 11](11_cli_and_tools.md)
- [x] Cloud Shell usage - [File 01](01_resource_hierarchy_and_project_setup.md)

#### 1.2 Managing billing configuration
- [x] Billing accounts (self-serve vs invoiced) - [File 02](02_billing_and_cost_management.md)
- [x] Budgets and alerts - [File 02](02_billing_and_cost_management.md)
- [x] Billing export (BigQuery, Google Sheets) - [File 02](02_billing_and_cost_management.md)
- [x] Cost allocation using labels - [File 12](12_core_concepts.md)

#### 1.3 Installing and configuring CLI
- [x] gcloud init and configuration - [File 11](11_cli_and_tools.md)
- [x] Setting default project and region/zone - [File 11](11_cli_and_tools.md)
- [x] Using multiple configurations - [File 11](11_cli_and_tools.md)

---

### 2. Planning and configuring a cloud solution (~19% of exam)

#### 2.1 Planning and estimating using Pricing Calculator
- [x] Pricing Calculator usage - [File 12](12_core_concepts.md)
- [x] Cost optimization strategies - [File 17](17_exam_strategy.md)

#### 2.2 Planning and configuring compute resources
- [x] Compute Engine VM options (machine types, images, disk) - [File 03](03_compute_services.md)
- [x] Preemptible/Spot VMs - [File 03](03_compute_services.md)
- [x] Custom machine types - [File 03](03_compute_services.md)
- [x] GKE (Standard, Autopilot, regional, zonal) - [File 03](03_compute_services.md)
- [x] App Engine (Standard, Flexible) - [File 03](03_compute_services.md)
- [x] Cloud Run - [File 03](03_compute_services.md)
- [x] Cloud Functions - [File 03](03_compute_services.md)
- [x] Managed instance groups (autoscaling, autohealing) - [File 03](03_compute_services.md)
- [x] Instance templates - [File 03](03_compute_services.md)
- [x] Startup scripts and metadata - [File 03](03_compute_services.md)
- [x] Container-Optimized OS - [File 03](03_compute_services.md)
- [x] Shielded VMs - [File 03](03_compute_services.md)
- [x] Confidential VMs - [File 03](03_compute_services.md)
- [x] Sole-tenant nodes - [File 03](03_compute_services.md)

#### 2.3 Planning and configuring data storage
- [x] Cloud Storage (classes, lifecycle, versioning) - [File 04](04_storage_and_databases.md)
- [x] Persistent Disks (SSD, HDD, regional, zonal) - [File 04](04_storage_and_databases.md)
- [x] Local SSDs (IOPS: 680K read, 360K write) - [File 04](04_storage_and_databases.md)
- [x] Cloud SQL (MySQL, PostgreSQL, SQL Server, HA) - [File 04](04_storage_and_databases.md)
- [x] Firestore (Native, Datastore mode) - [File 04](04_storage_and_databases.md)
- [x] Bigtable - [File 04](04_storage_and_databases.md)
- [x] Cloud Spanner - [File 04](04_storage_and_databases.md)
- [x] BigQuery - [File 04](04_storage_and_databases.md)
- [x] Filestore - [File 16](16_containers_and_registries.md)
- [x] Memorystore (Redis, Memcached) - [File 16](16_containers_and_registries.md)
- [x] AlloyDB - [File 04](04_storage_and_databases.md)

#### 2.4 Planning and configuring network resources
- [x] VPC (default vs custom, auto vs custom subnets) - [File 05](05_networking.md)
- [x] Subnets and IP addressing - [File 05](05_networking.md), [File 19](19_ip_addressing_and_subnetting.md)
- [x] Firewall rules - [File 05](05_networking.md)
- [x] Routes - [File 05](05_networking.md)
- [x] Load balancing (HTTP(S), TCP/UDP, SSL Proxy, Network) - [File 06](06_load_balancing_and_traffic_management.md)
- [x] Cloud CDN - [File 06](06_load_balancing_and_traffic_management.md)
- [x] Cloud Armor - [File 06](06_load_balancing_and_traffic_management.md)
- [x] VPN (Classic, HA VPN) - [File 05](05_networking.md)
- [x] Cloud Interconnect (Dedicated, Partner) - [File 05](05_networking.md)
- [x] VPC peering - [File 05](05_networking.md)
- [x] Shared VPC - [File 05](05_networking.md)
- [x] Cloud DNS (public, private zones) - [File 05](05_networking.md)
- [x] Cloud NAT - [File 05](05_networking.md)
- [x] Private Google Access - [File 05](05_networking.md)
- [x] Private Service Connect - [File 05](05_networking.md)
- [x] VPC Flow Logs - [File 05](05_networking.md)
- [x] Packet Mirroring - [File 05](05_networking.md)

---

### 3. Deploying and implementing a cloud solution (~27% of exam)

#### 3.1 Deploying and implementing Compute Engine
- [x] Creating VMs (gcloud, Console) - [File 03](03_compute_services.md)
- [x] Creating instance templates - [File 03](03_compute_services.md)
- [x] Creating managed instance groups - [File 03](03_compute_services.md)
- [x] Generating/uploading SSH keys - [File 03](03_compute_services.md)
- [x] Configuring preemptible/spot VMs - [File 03](03_compute_services.md)
- [x] Installing Ops Agent for monitoring - [File 08](08_operations_and_monitoring.md)

#### 3.2 Deploying and implementing GKE
- [x] Creating GKE clusters - [File 03](03_compute_services.md)
- [x] Deploying containerized applications (kubectl) - [File 11](11_cli_and_tools.md)
- [x] Configuring monitoring and logging - [File 08](08_operations_and_monitoring.md)
- [x] Workload Identity - [File 07](07_identity_and_security.md)

#### 3.3 Deploying and implementing Cloud Run
- [x] Deploying applications - [File 03](03_compute_services.md)
- [x] Configuring scaling and concurrency - [File 03](03_compute_services.md)
- [x] Cloud Run Jobs - [File 18](18_additional_services.md)

#### 3.4 Deploying and implementing Cloud Functions
- [x] Creating functions (event-driven, HTTP) - [File 03](03_compute_services.md)
- [x] Configuring triggers (Pub/Sub, Cloud Storage, HTTP) - [File 03](03_compute_services.md), [File 13](13_messaging_and_integration.md)

#### 3.5 Deploying and implementing data solutions
- [x] Initializing databases (Cloud SQL, Firestore, BigQuery) - [File 04](04_storage_and_databases.md)
- [x] Loading data (BigQuery, Cloud SQL, Firestore) - [File 04](04_storage_and_databases.md)
- [x] Database Migration Service - [File 04](04_storage_and_databases.md)
- [x] Cloud Storage bucket creation and configuration - [File 04](04_storage_and_databases.md)
- [x] Retention policies and lifecycle management - [File 04](04_storage_and_databases.md)

#### 3.6 Deploying and implementing networking resources
- [x] Creating VPCs and subnets - [File 05](05_networking.md)
- [x] Creating ingress/egress firewall rules - [File 05](05_networking.md)
- [x] Creating VPN connections - [File 05](05_networking.md)
- [x] Creating load balancers - [File 06](06_load_balancing_and_traffic_management.md)
- [x] Creating Cloud NAT - [File 05](05_networking.md)

#### 3.7 Deploying solutions using Cloud Marketplace
- [x] Browsing and deploying solutions - [File 09](09_deployment_and_automation.md)

#### 3.8 Deploying application infrastructure using Deployment Manager or Terraform
- [x] Deployment Manager - [File 09](09_deployment_and_automation.md)
- [x] Terraform basics - [File 09](09_deployment_and_automation.md)

---

### 4. Ensuring successful operation of a cloud solution (~20% of exam)

#### 4.1 Managing Compute Engine resources
- [x] Managing instance lifecycle (start, stop, delete) - [File 03](03_compute_services.md)
- [x] SSH/RDP to instances - [File 03](03_compute_services.md)
- [x] Attaching/detaching disks - [File 04](04_storage_and_databases.md)
- [x] Viewing VM serial port output - [File 03](03_compute_services.md)
- [x] Snapshots and images - [File 04](04_storage_and_databases.md)
- [x] Instance groups management - [File 03](03_compute_services.md)
- [x] Live migration - [File 03](03_compute_services.md)

#### 4.2 Managing GKE resources
- [x] Viewing cluster and workload details - [File 03](03_compute_services.md), [File 11](11_cli_and_tools.md)
- [x] Viewing pod logs - [File 11](11_cli_and_tools.md)
- [x] Cluster management (node pools, autoscaling) - [File 03](03_compute_services.md)

#### 4.3 Managing Cloud Run resources
- [x] Adjusting traffic splitting - [File 03](03_compute_services.md)
- [x] Viewing and managing revisions - [File 03](03_compute_services.md)

#### 4.4 Managing storage and database solutions
- [x] Managing Cloud Storage (moving objects, setting lifecycle) - [File 04](04_storage_and_databases.md), [File 10](10_data_transfer_and_migration.md)
- [x] Managing Cloud SQL (backups, restores, connections) - [File 04](04_storage_and_databases.md)
- [x] Managing Firestore (import/export, queries) - [File 04](04_storage_and_databases.md)
- [x] Managing BigQuery (queries, datasets, tables) - [File 04](04_storage_and_databases.md)

#### 4.5 Managing networking resources
- [x] Adding/modifying subnets - [File 05](05_networking.md)
- [x] Adding/modifying firewall rules - [File 05](05_networking.md)
- [x] Viewing and managing routes - [File 05](05_networking.md)
- [x] Administering VPNs - [File 05](05_networking.md)
- [x] Managing load balancers - [File 06](06_load_balancing_and_traffic_management.md)

#### 4.6 Monitoring and logging
- [x] Cloud Monitoring (dashboards, metrics, alerts) - [File 08](08_operations_and_monitoring.md)
- [x] Cloud Logging (log viewing, filtering, sinks) - [File 08](08_operations_and_monitoring.md)
- [x] Cloud Trace - [File 08](08_operations_and_monitoring.md)
- [x] Cloud Profiler - [File 08](08_operations_and_monitoring.md)
- [x] Cloud Debugger - [File 08](08_operations_and_monitoring.md)
- [x] Error Reporting - [File 08](08_operations_and_monitoring.md)
- [x] Uptime checks - [File 08](08_operations_and_monitoring.md)
- [x] Health checks - [File 08](08_operations_and_monitoring.md)
- [x] Ops Agent installation - [File 08](08_operations_and_monitoring.md)
- [x] SLOs and SLIs - [File 08](08_operations_and_monitoring.md), [File 12](12_core_concepts.md)

---

### 5. Configuring access and security (~17% of exam)

#### 5.1 Managing IAM
- [x] IAM roles (primitive, predefined, custom) - [File 07](07_identity_and_security.md)
- [x] Viewing and assigning IAM roles - [File 07](07_identity_and_security.md)
- [x] Defining custom IAM roles - [File 07](07_identity_and_security.md)
- [x] Managing service accounts - [File 07](07_identity_and_security.md)
- [x] Service account keys - [File 07](07_identity_and_security.md)
- [x] Workload Identity - [File 07](07_identity_and_security.md)
- [x] IAM Conditions - [File 07](07_identity_and_security.md)
- [x] OS Login - [File 07](07_identity_and_security.md)
- [x] Workforce Identity Federation (human users) - [File 07](07_identity_and_security.md)
- [x] Workload Identity Federation (external apps, service account alternative) - [File 07](07_identity_and_security.md)
- [x] Cloud Identity - [File 07](07_identity_and_security.md)

#### 5.2 Managing service accounts
- [x] Creating and managing service accounts - [File 07](07_identity_and_security.md)
- [x] Granting roles to service accounts - [File 07](07_identity_and_security.md)
- [x] Managing service account keys - [File 07](07_identity_and_security.md)
- [x] Using service accounts in applications - [File 07](07_identity_and_security.md)

#### 5.3 Viewing audit logs
- [x] Audit log types (Admin Activity, Data Access, System Event) - [File 07](07_identity_and_security.md)
- [x] Viewing and filtering audit logs - [File 07](07_identity_and_security.md)
- [x] Exporting audit logs - [File 07](07_identity_and_security.md)
- [x] Access Transparency - [File 07](07_identity_and_security.md)

#### 5.4 Managing security
- [x] Secret Manager - [File 15](15_security_advanced.md)
- [x] Cloud KMS (key management, CMEK) - [File 15](15_security_advanced.md)
- [x] VPC Service Controls - [File 15](15_security_advanced.md)
- [x] Binary Authorization - [File 15](15_security_advanced.md)
- [x] Security Command Center - [File 15](15_security_advanced.md)
- [x] Organization Policy Service - [File 15](15_security_advanced.md)
- [x] Web Security Scanner - [File 15](15_security_advanced.md)
- [x] Shielded VMs - [File 03](03_compute_services.md)
- [x] Confidential VMs - [File 03](03_compute_services.md)

---

## Additional Important Topics Covered

### Messaging & Integration
- [x] Cloud Pub/Sub - [File 13](13_messaging_and_integration.md)
- [x] Eventarc - [File 13](13_messaging_and_integration.md)
- [x] Cloud Tasks - [File 13](13_messaging_and_integration.md)
- [x] Workflows - [File 13](13_messaging_and_integration.md)
- [x] Cloud Scheduler - [File 18](18_additional_services.md)

### Data Analytics
- [x] Dataflow - [File 14](14_data_analytics.md)
- [x] Dataproc - [File 14](14_data_analytics.md)
- [x] Cloud Composer - [File 14](14_data_analytics.md)
- [x] Data Fusion - [File 14](14_data_analytics.md)

### CI/CD & DevOps
- [x] Cloud Build - [File 09](09_deployment_and_automation.md)
- [x] Cloud Source Repositories - [File 09](09_deployment_and_automation.md)
- [x] Cloud Deploy - [File 18](18_additional_services.md)
- [x] Artifact Registry - [File 16](16_containers_and_registries.md)
- [x] Container Registry (legacy) - [File 16](16_containers_and_registries.md)

### AI/ML
- [x] Vertex AI - [File 16](16_containers_and_registries.md)
- [x] Pre-trained APIs (Vision, Language, Translation, Speech) - [File 16](16_containers_and_registries.md)

### Migration & Transfer
- [x] Storage Transfer Service - [File 10](10_data_transfer_and_migration.md)
- [x] Database Migration Service - [File 04](04_storage_and_databases.md)
- [x] Transfer Appliance - [File 10](10_data_transfer_and_migration.md)

### Additional Services
- [x] API Gateway - [File 18](18_additional_services.md)
- [x] Cloud Endpoints - [File 18](18_additional_services.md)
- [x] Cloud Asset Inventory - [File 18](18_additional_services.md)
- [x] Recommender API - [File 18](18_additional_services.md)

### Core Concepts
- [x] Regions and Zones - [File 12](12_core_concepts.md)
- [x] High Availability and Scalability - [File 12](12_core_concepts.md)
- [x] Labels and Tagging - [File 12](12_core_concepts.md)
- [x] Quotas - [File 01](01_resource_hierarchy_and_project_setup.md)
- [x] SLA/SLO/SLI - [File 12](12_core_concepts.md)
- [x] Resource location types (multi-region, regional, zonal) - [File 12](12_core_concepts.md)
- [x] Free Tier and Trial - [File 18](18_additional_services.md)
- [x] Shared Responsibility Model - [File 18](18_additional_services.md)
- [x] IP Addressing & CIDR Notation - [File 19](19_ip_addressing_and_subnetting.md)

### Exam Strategy
- [x] Migration strategies - [File 17](17_exam_strategy.md)
- [x] Disaster recovery patterns - [File 17](17_exam_strategy.md)
- [x] Cost optimization - [File 17](17_exam_strategy.md)
- [x] Security best practices - [File 17](17_exam_strategy.md)
- [x] Troubleshooting patterns - [File 17](17_exam_strategy.md)
- [x] Service selection decision trees - [File 17](17_exam_strategy.md)
- [x] Exam tips and time management - [File 17](17_exam_strategy.md)

---
## Key Command Patterns Covered

- [x] gcloud - [File 11](11_cli_and_tools.md)
- [x] kubectl - [File 11](11_cli_and_tools.md)
- [x] gsutil - [File 11](11_cli_and_tools.md)
- [x] bq - [File 11](11_cli_and_tools.md)
- [x] Cloud Console basics - [File 12](12_core_concepts.md)
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

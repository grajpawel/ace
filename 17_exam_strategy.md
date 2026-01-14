# 17. Exam Strategy & Best Practices (Detailed)

This guide covers key strategies, patterns, and best practices for the Google Associate Cloud Engineer certification exam.

## Common Exam Scenarios

### Migration Strategies
Understanding the right migration approach is critical for the exam.

- **Lift and Shift (Rehosting):**
	- Move applications to GCP with minimal changes.
	- Use Compute Engine VMs or Migrate for Compute Engine.
	- Quick migration but doesn't leverage cloud-native features.
	- Best for: Legacy apps, quick migrations, minimal refactoring.

- **Replatforming:**
	- Make minimal cloud optimizations without major code changes.
	- Example: Migrate database to Cloud SQL, use managed services.
	- Better than lift-and-shift but not fully cloud-native.
	- Best for: Reducing operational overhead while maintaining app structure.

- **Refactoring/Re-architecting:**
	- Redesign applications for cloud-native architecture.
	- Use App Engine, Cloud Run, Cloud Functions, GKE.
	- Maximum cloud benefits but requires development effort.
	- Best for: Modernization, scalability, serverless architectures.

- **Rebuild:**
	- Rewrite applications from scratch using cloud-native technologies.
	- Highest effort but maximum optimization.
	- Best for: Outdated systems, complete technology refresh.

- **Replace:**
	- Replace existing software with SaaS alternatives.
	- Example: Move from on-premises email to Google Workspace.
	- Best for: Commodity applications with SaaS equivalents.

---

### Disaster Recovery Patterns
Understanding DR strategies and their RTO/RPO characteristics is frequently tested.

- **Backup and Restore (Cold Standby):**
	- Regular backups to Cloud Storage or snapshots.
	- Restore when needed (highest RTO/RPO).
	- Lowest cost, highest recovery time.
	- **RTO:** Hours to days | **RPO:** Hours to days.

- **Pilot Light:**
	- Minimal core components always running.
	- Scale up during disaster.
	- **RTO:** Minutes to hours | **RPO:** Minutes to hours.

- **Warm Standby:**
	- Scaled-down version always running.
	- Scale up during disaster.
	- **RTO:** Minutes | **RPO:** Near-zero with replication.

- **Hot Standby (Multi-region Active):**
	- Full production environment in multiple regions.
	- Traffic distributes across regions.
	- Highest cost, lowest RTO/RPO.
	- **RTO:** Seconds to minutes | **RPO:** Near-zero.

---

### Cost Optimization Patterns

- **Right-Sizing:**
	- Match VM size to actual workload needs.
	- Use Cloud Monitoring to track utilization.
	- Resize or use custom machine types.

- **Committed Use Discounts:**
	- 1-year or 3-year commitments for steady-state workloads.
	- Up to 57% savings for Compute Engine.

- **Sustained Use Discounts:**
	- Automatic discounts for VMs running >25% of month.
	- Up to 30% discount, no commitment needed.

- **Preemptible/Spot VMs:**
	- Up to 91% discount for fault-tolerant workloads.
	- Use for batch processing, rendering, CI/CD.

- **Storage Classes:**
	- Use appropriate Cloud Storage class (Standard, Nearline, Coldline, Archive).
	- Implement lifecycle policies for automatic transitions.

- **Resource Labels:**
	- Tag resources for cost allocation and tracking.
	- Use in billing reports and budgets.

- **Autoscaling:**
	- Scale resources based on demand.
	- Avoid over-provisioning.

---

### Security Best Practices

- **Principle of Least Privilege:**
	- Grant minimum permissions required.
	- Use predefined roles over primitive roles.
	- Regular IAM audits.

- **Defense in Depth:**
	- Multiple security layers (network, IAM, encryption, etc.).
	- VPC Service Controls for data perimeters.
	- Cloud Armor for DDoS and WAF.

- **Encryption:**
	- Data encrypted at rest (default Google-managed keys).
	- Use CMEK (Customer-Managed Encryption Keys) for sensitive data.
	- Encryption in transit (TLS/SSL).

- **Secret Management:**
	- Never hardcode secrets.
	- Use Secret Manager for API keys, passwords, certificates.
	- Rotate secrets regularly.

- **Network Security:**
	- Use custom VPCs with restrictive firewall rules.
	- Private Google Access for private VMs.
	- Cloud NAT for outbound-only internet access.
	- VPN or Interconnect for hybrid connectivity.

- **Audit and Monitoring:**
	- Enable all audit logs (Admin Activity, Data Access).
	- Export logs to BigQuery or Cloud Storage.
	- Set up alerts for security events.

---

## Service Selection Decision Trees

### Compute Service Selection
**Choose based on:**
- **Full control needed?** → Compute Engine
- **Containerized with Kubernetes?** → GKE
- **Containerized, HTTP-only, serverless?** → Cloud Run
- **Web app, minimal ops, PaaS?** → App Engine
- **Event-driven, small functions?** → Cloud Functions

### Storage Service Selection
**Choose based on:**
- **Unstructured data (files, media)?** → Cloud Storage
- **Block storage for VMs?** → Persistent Disks
- **Shared file system (NFS)?** → Filestore
- **In-memory cache?** → Memorystore

### Database Service Selection
**Choose based on:**
- **Relational, managed (MySQL/PostgreSQL/SQL Server)?** → Cloud SQL
- **Relational, global scale, horizontal scaling?** → Cloud Spanner
- **PostgreSQL, high performance, enterprise?** → AlloyDB
- **NoSQL, document, mobile/web?** → Firestore
- **NoSQL, wide-column, analytics, high throughput?** → Bigtable
- **Analytics, data warehouse, SQL?** → BigQuery

### Load Balancer Selection
**Choose based on:**
- **HTTP(S) traffic, global?** → Global HTTP(S) Load Balancer
- **TCP/UDP traffic, regional, low latency?** → Network Load Balancer
- **SSL/TLS proxy, non-HTTP, global?** → SSL Proxy Load Balancer
- **TCP proxy, non-HTTP, global?** → TCP Proxy Load Balancer

---

## Common Exam Topics & Keywords

### High Availability Keywords
- Multi-zone, multi-region
- Managed instance groups
- Autohealing, autoscaling
- Load balancing
- Regional persistent disks
- Cloud SQL high availability
- Cloud Spanner

### Cost Optimization Keywords
- Committed use discounts
- Sustained use discounts
- Preemptible/Spot VMs
- Right-sizing
- Lifecycle policies
- Storage classes
- Labels for cost allocation

### Security Keywords
- Least privilege
- Service accounts
- IAM roles and policies
- VPC Service Controls
- Cloud KMS, CMEK
- Secret Manager
- Private Google Access
- Cloud Armor
- Binary Authorization

### Performance Keywords
- SSD vs HDD
- Premium vs Standard network tier
- Cloud CDN
- Memorystore/caching
- Regional vs zonal resources
- Custom machine types

---

## Troubleshooting Patterns

### VM Cannot Access the Internet
1. Check if VM has external IP (or Cloud NAT configured).
2. Check firewall rules for egress traffic.
3. Verify routes in VPC.
4. Check if Private Google Access is needed for Google APIs.

### VM Cannot Be Reached from Internet
1. Check if VM has external IP.
2. Check firewall rules for ingress traffic on required ports.
3. Verify service is running and listening on correct port.
4. Check load balancer configuration if using one.

### VMs in Different VPCs Cannot Communicate
1. VPC Peering needed for private communication.
2. Firewall rules must allow traffic from peered VPC.
3. Check routes are created (automatic with peering).

### Application Errors Not Visible
1. Enable Error Reporting.
2. Check Cloud Logging for application logs.
3. Verify logging agent (Ops Agent) is installed on VMs.

### High Cloud Storage Costs
1. Review storage class usage.
2. Implement lifecycle policies to transition to Nearline/Coldline/Archive.
3. Delete old versions if versioning is enabled.
4. Use labels to identify costly buckets.

---

## Exam Day Tips

### Time Management
- 50 questions in 2 hours = ~2.4 minutes per question.
- Flag difficult questions and return later.
- Read questions carefully for keywords.

### Question Analysis
- **Identify the requirement:** What is the question asking for?
- **Look for constraints:** Cost, performance, security, simplicity.
- **Eliminate wrong answers:** Often 2-3 answers can be quickly eliminated.
- **Choose best fit:** Select the option that meets all requirements.

### Common Traps
- **Over-engineering:** Don't choose complex solutions when simple ones work.
- **Ignoring constraints:** Pay attention to "least cost," "most secure," "minimal management."
- **Confusing similar services:** Cloud SQL vs Spanner, Dataproc vs Dataflow, etc.

### Keywords to Watch For
- **"Most cost-effective"** → Preemptible VMs, lifecycle policies, committed use discounts.
- **"Least operational overhead"** → Managed services, serverless, automation.
- **"Highly available"** → Multi-zone, multi-region, managed instance groups.
- **"Secure"** → IAM, VPC Service Controls, private IPs, encryption.
- **"Minimal latency"** → Regional resources, CDN, caching, SSD.

---

## Critical gcloud Command Patterns

### Configuration
```sh
gcloud config set project PROJECT_ID
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
gcloud config list
```

### IAM
```sh
gcloud projects add-iam-policy-binding PROJECT_ID --member=user:EMAIL --role=ROLE
gcloud projects get-iam-policy PROJECT_ID
```

### Compute
```sh
gcloud compute instances create VM_NAME --zone=ZONE
gcloud compute instance-groups managed create GROUP --size=3 --template=TEMPLATE
```

### Storage
```sh
gcloud storage buckets create gs://BUCKET_NAME --location=LOCATION
gsutil cp FILE gs://BUCKET/
```

### Networking
```sh
gcloud compute networks create VPC_NAME --subnet-mode=custom
gcloud compute firewall-rules create RULE_NAME --network=VPC --allow=tcp:22
```

---

**Final Tip:** The exam tests breadth of knowledge across many GCP services. Focus on understanding when to use each service, their key features, and how they integrate. Practice with hands-on labs and review the official exam guide regularly.

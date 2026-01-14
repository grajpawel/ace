# 12. Core Concepts (Detailed)

Understanding Google Cloud's core concepts is essential for designing robust, scalable, and cost-effective solutions. Below are foundational ideas, best practices, and relevant gcloud commands.

## Regions & Zones
- **Region:** Independent geographic area with multiple zones. Provides redundancy, high availability, and data residency options.
- **Zone:** Deployment area within a region. Resources in different zones are isolated from each other to prevent single points of failure.
- **Best Practices:**
	- Deploy resources across multiple zones for fault tolerance.
	- Use regional resources (e.g., regional disks, regional managed instance groups) for higher availability.
	- Choose regions close to your users for lower latency.
- **gcloud Examples:**
	- List available regions:
		```sh
		gcloud compute regions list
		```
	- List available zones:
		```sh
		gcloud compute zones list
		```

---

## High Availability & Scalability
- **High Availability (HA):** System remains operational despite failures (multi-zone, multi-region, load balancing, autohealing, managed instance groups).
- **Scalability:** Ability to handle increased load by adding resources (vertical scaling = bigger machines, horizontal scaling = more machines, autoscaling groups).
- **Best Practices:**
	- Use managed instance groups with autoscaling and autohealing.
	- Deploy across multiple zones/regions for mission-critical workloads.
	- Use load balancers to distribute traffic and avoid single points of failure.
- **gcloud Examples:**
	- Create a managed instance group with autoscaling:
		```sh
		gcloud compute instance-groups managed create my-group --base-instance-name=my-vm --size=3 --template=my-template --zone=us-central1-a
		gcloud compute instance-groups managed set-autoscaling my-group --max-num-replicas=10 --target-cpu-utilization=0.6 --zone=us-central1-a
		```

---

## Labels & Resource Tagging
Key-value pairs attached to resources for organization, filtering, automation, and cost allocation.

- **Features:**
	- Used in billing reports, IAM policies, and automation scripts.
	- Helps with resource grouping, cost tracking, and policy enforcement.
- **Best Practices:**
	- Apply consistent labeling conventions across projects.
	- Use labels for cost allocation and automation.
- **gcloud Examples:**
	- Add a label to a VM:
		```sh
		gcloud compute instances add-labels my-vm --labels=env=prod,team=analytics --zone=us-central1-a
		```
	- List resources with a specific label:
		```sh
		gcloud compute instances list --filter="labels.env=prod"
		```

---

## Service Level Agreements (SLA), Objectives (SLO), and Indicators (SLI)
Understanding service reliability metrics is important for architecture decisions.

### SLA (Service Level Agreement)
- **Definition:** Contractual commitment between Google and customers.
- **Typical GCP SLAs:**
	- Compute Engine: 99.99% (multi-zone), 99.5% (single instance)
	- Cloud Storage: 99.95% (multi-region), 99.9% (regional)
	- Cloud SQL: 99.95% (HA), 99.5% (single instance)
	- GKE: 99.95% (regional), 99.5% (zonal)
	- Cloud Run: 99.95%
	- BigQuery: 99.99%
- **Service Credits:** Google provides credits if SLA is not met.
- **Exclusions:** SLA typically excludes events beyond Google's control, scheduled maintenance, customer misconfigurations.

### SLO (Service Level Objective)
- **Definition:** Internal target for service performance (stricter than SLA).
- **Purpose:** Guide operational decisions and capacity planning.
- **Example:** Target 99.99% availability even if SLA is 99.9%.

### SLI (Service Level Indicator)
- **Definition:** Quantitative measure of service level.
- **Common SLIs:**
	- Availability (uptime percentage)
	- Latency (response time)
	- Error rate
	- Throughput

**Exam Tip:** 
- SLA = Contract with customers
- SLO = Internal target
- SLI = Actual measurement

---

## Resource Location Types
Understanding location types helps optimize latency, availability, and compliance.

### Multi-region
- **Definition:** Resources replicated across multiple regions within a large geographic area.
- **Examples:** 
	- Cloud Storage: US, EU, ASIA multi-regions
	- BigQuery: US, EU multi-regions
- **Benefits:** Highest availability and disaster recovery.
- **Cost:** Typically higher than regional.
- **Use cases:** Global applications, critical data requiring redundancy.

### Dual-region
- **Definition:** Resources replicated across two specific regions.
- **Examples:**
	- Cloud Storage: us-central1 + us-east1
	- Spanner: Regional + witness region
- **Benefits:** Balance of cost and availability.
- **Use cases:** High availability within specific geography.

### Regional
- **Definition:** Resources in a specific region (multiple zones).
- **Examples:** Most GCP services (Compute Engine, GKE, Cloud SQL)
- **Benefits:** Lower latency, data residency compliance.
- **Cost:** Lower than multi-region.
- **Use cases:** Most production workloads.

### Zonal
- **Definition:** Resources in a specific zone within a region.
- **Examples:** Compute Engine instances, zonal persistent disks
- **Benefits:** Lowest cost, specific placement.
- **Risk:** Single point of failure (zone outage affects resource).
- **Use cases:** Development, testing, non-critical workloads.

### Global
- **Definition:** Resources managed globally but may have regional endpoints.
- **Examples:** 
	- HTTP(S) Load Balancer
	- Cloud CDN
	- Cloud IAM
	- VPC networks
- **Benefits:** Unified management, global reach.

**Exam Tip:** 
- Multi-region = Highest availability, highest cost
- Regional = Standard for production
- Zonal = Lower cost, lower availability
- Choose based on availability requirements and budget

---

## Cloud Console Basics
Web-based GUI for managing GCP resources. Essential for exam scenarios involving UI operations.

### Key Features:
- **Dashboard:** Overview of projects, resources, billing
- **Navigation Menu:** Access to all GCP services
- **Cloud Shell:** Browser-based terminal with gcloud pre-installed
- **Activity Logs:** Recent actions and changes
- **Notifications:** Alerts, recommendations, billing warnings
- **Search:** Find resources, documentation, products
- **IAM & Admin:** Manage permissions, service accounts, quotas
- **APIs & Services:** Enable/disable APIs, view usage

### Common Tasks:
1. **Switch Projects:** Use project selector dropdown
2. **Enable APIs:** Navigation > APIs & Services > Library
3. **View Billing:** Navigation > Billing
4. **Create Resources:** Navigate to service > Create
5. **Cloud Shell:** Click terminal icon (top right)
6. **Search Resources:** Use search bar (top)

**Best Practices:**
- Use Cloud Console for exploration and one-time tasks
- Use gcloud/Terraform for automation and IaC
- Enable MFA for console access
- Review Activity logs regularly

---

## Pricing Calculator & Cost Estimation
- **Pricing Calculator:** Web tool to estimate costs for GCP resources and architectures ([GCP Pricing Calculator](https://cloud.google.com/products/calculator)).
- **Cost Estimation:** Use budgets, alerts, and billing export for ongoing cost management.
- **Best Practices:**
	- Estimate costs before deploying new architectures.
	- Set up budgets and alerts to avoid unexpected charges.
	- Use billing export to BigQuery for detailed analysis.
- **gcloud Examples:**
	- List budgets:
		```sh
		gcloud alpha billing budgets list --billing-account=BILLING_ACCOUNT_ID
		```

---

## Key Differences
- **Regions vs Zones:** Geographic (region) vs deployment isolation (zone).
- **HA vs Scalability:** Uptime (HA) vs capacity/growth (scalability).
- **Labels:** Organization and cost tracking, not access control.
- **Pricing Calculator:** Planning and estimation, not actual billing.

---

## Summary Table

| Concept         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-------------------------|-------------------------------|-----------------------------------------------|
| Region/Zone     | Redundancy, isolation   | Region = geo, Zone = deploy    | gcloud compute regions list                   |
| HA/Scalability  | Uptime, growth          | HA = uptime, Scalability = load| gcloud compute instance-groups managed create |
| Labels/Tags     | Organization, billing   | Not for access control         | gcloud compute instances add-labels           |
| Pricing Calc    | Cost planning           | Estimate, not actual spend     | gcloud alpha billing budgets list             |

---

**Tip:** Design for high availability and scalability from the start. Use labels for organization and cost tracking. Always estimate and monitor costs to avoid surprises.
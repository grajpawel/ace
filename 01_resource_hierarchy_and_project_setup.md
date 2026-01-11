
# 1. Resource Hierarchy & Project Setup (Detailed)

## Resource Hierarchy in Google Cloud
Google Cloud Platform (GCP) organizes resources in a strict hierarchy to enable scalable, secure, and manageable cloud environments. Understanding this structure is critical for effective access control, billing, and policy enforcement.

### 1. Organization
- **Definition:** The root node of the GCP resource hierarchy, representing a company, enterprise, or domain (e.g., example.com). All resources ultimately belong to an organization.
- **Features:**
	- Centralized IAM and policy management
	- Centralized billing
	- Security and compliance controls at the highest level
- **Typical Use Case:** Large companies, enterprises, or educational institutions with multiple teams or departments.
- **gcloud Example:**
	- List organizations you have access to:
		```sh
		gcloud organizations list
		```

### 2. Folders
- **Definition:** Optional nodes used to group projects logically (e.g., by department, environment, or function). Folders can be nested for complex organizations.
- **Features:**
	- Apply IAM and organization policies at a group level
	- Reflect business structure (e.g., /Engineering/Dev, /Engineering/Prod)
- **gcloud Example:**
	- List folders under an organization:
		```sh
		gcloud resource-manager folders list --organization=ORG_ID
		```
	- Create a folder:
		```sh
		gcloud resource-manager folders create --display-name="Dev" --organization=ORG_ID
		```

### 3. Projects
- **Definition:** The main container for all GCP resources. Projects are required for resource creation and are the boundary for billing, quotas, and IAM.
- **Features:**
	- Each project has a unique project ID, name, and number
	- Isolated from other projects (IAM, quotas, billing)
	- Can be moved between folders or organizations
- **gcloud Examples:**
	- List projects:
		```sh
		gcloud projects list
		```
	- Create a new project:
		```sh
		gcloud projects create PROJECT_ID --name="Project Name" --folder=FOLDER_ID
		```
	- Set default project:
		```sh
		gcloud config set project PROJECT_ID
		```

### 4. Resources
- **Definition:** Actual GCP services and infrastructure (e.g., Compute Engine VMs, Cloud Storage buckets, BigQuery datasets) that reside within projects.
- **Features:**
	- All resources must belong to a project
	- IAM and policies can be set at the resource level for fine-grained control
- **gcloud Example:**
	- List Compute Engine instances in a project:
		```sh
		gcloud compute instances list --project=PROJECT_ID
		```

#### Key Differences Recap
- **Organizations** are global and unique per domain.
- **Folders** are optional, can be nested, and help structure resources.
- **Projects** are required for all resources and are the main boundary for billing, quotas, and IAM.
- **Resources** are the actual services and must belong to a project.

---

## APIs & Services
GCP services are exposed via APIs, which must be enabled on a per-project basis. Managing APIs is essential for controlling access, costs, and security.

- **Enabling/Disabling APIs:**
	- APIs are disabled by default. Enabling an API allows you to use the associated service.
	- Disabling an API disables all associated resources and may delete data.
- **IAM Roles:**
	- API usage is controlled via IAM roles (e.g., Service Usage Admin, roles/serviceusage.serviceUsageAdmin).
- **Billing:**
	- Some APIs are free, while others are billable. Always review pricing before enabling APIs.

**gcloud Examples:**
- List enabled APIs:
	```sh
	gcloud services list --enabled --project=PROJECT_ID
	```
- Enable an API (e.g., Compute Engine):
	```sh
	gcloud services enable compute.googleapis.com --project=PROJECT_ID
	```
- Disable an API:
	```sh
	gcloud services disable compute.googleapis.com --project=PROJECT_ID
	```

---

## Cloud Shell & Cloud SDK

### Cloud Shell
- **Definition:** A browser-based shell environment with preinstalled tools (gcloud, gsutil, kubectl, bq, etc.).
- **Features:**
	- 5GB persistent home directory
	- No setup required; always up-to-date
	- Ephemeral VM (reset on restart)
- **Use Cases:** Quick access to GCP CLI tools, troubleshooting, demos, and training.

### Cloud SDK
- **Definition:** A set of CLI tools (gcloud, gsutil, bq, kubectl) for managing GCP resources from your local machine.
- **Features:**
	- Requires installation and authentication
	- May require manual updates
	- Can be used in CI/CD pipelines
- **Installation:**
	- [Install Cloud SDK](https://cloud.google.com/sdk/docs/install)
- **Authentication:**
	```sh
	gcloud init
	```
	or
	```sh
	gcloud auth login
	```

#### Key Differences
- Cloud Shell is ephemeral and browser-based; Cloud SDK is installed locally and persistent.
- Cloud Shell is always up-to-date; Cloud SDK may require manual updates.

---

## Quotas
Quotas are limits on resource usage to prevent accidental overuse and control costs. They apply per project, per region, or per resource type.

- **Types of Quotas:**
	- Compute Engine CPUs, IP addresses, API requests, etc.
- **Purpose:**
	- Prevent resource exhaustion
	- Control costs
	- Enforce fair usage
- **Quota Management:**
	- Quotas can be increased via requests, but not all quotas are adjustable.
	- Quota increases may require justification and approval.

**gcloud Examples:**
- List all quotas for a project:
	```sh
	gcloud compute project-info describe --project=PROJECT_ID
	```
- List quotas for a specific region:
	```sh
	gcloud compute regions describe REGION --project=PROJECT_ID
	```
- Request a quota increase:
	- Use the [GCP Console Quotas page](https://console.cloud.google.com/iam-admin/quotas)

---

## Summary Table

| Feature         | Scope         | Use Case                        | Key Differences                | Example gcloud Command                  |
|-----------------|--------------|---------------------------------|-------------------------------|-----------------------------------------|
| Organization    | Global       | Company-wide policy/billing     | Root node, one per domain     | gcloud organizations list               |
| Folder          | Optional     | Grouping projects               | Nested, for structure         | gcloud resource-manager folders list    |
| Project         | Required     | Resource container, billing     | Main isolation boundary       | gcloud projects list                    |
| Resource        | Per-project  | Actual services (VM, bucket)    | Must belong to a project      | gcloud compute instances list           |
| Cloud Shell     | Per-user     | Quick CLI access                | Browser, ephemeral            | N/A (browser-based)                     |
| Cloud SDK       | Per-device   | Local CLI management            | Installed, persistent         | gcloud init                             |
| Quotas          | Per-project  | Usage/cost control              | Adjustable, not always        | gcloud compute project-info describe    |

---

**Tip:** Always use the principle of least privilege when assigning IAM roles, and regularly audit your resource hierarchy and quotas for security and cost optimization.
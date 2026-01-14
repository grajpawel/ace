# 9. Deployment & Automation (Detailed)

Google Cloud provides several tools and services to automate deployment, manage infrastructure as code, and streamline CI/CD workflows. Below are the main services, their features, use cases, and relevant gcloud commands.

## Deployment Manager
Infrastructure as Code (IaC) tool for GCP. Allows you to define, deploy, and manage resources using declarative configuration files (YAML or Python).

- **Features:**
	- Automates resource creation, updates, and deletion.
	- Supports templates and parameterization for reusable configs.
	- Enables version control and repeatable deployments.
- **Best Practices:**
	- Use templates for reusable infrastructure patterns.
	- Store configs in source control for auditability.
- **gcloud Examples:**
	- Create a deployment:
		```sh
		gcloud deployment-manager deployments create my-deployment --config=config.yaml
		```
	- Update a deployment:
		```sh
		gcloud deployment-manager deployments update my-deployment --config=config.yaml
		```
	- List deployments:
		```sh
		gcloud deployment-manager deployments list
		```

---

## Cloud Marketplace
Catalog of preconfigured solutions, VMs, and SaaS apps. Enables one-click deployment of third-party and Google solutions to your GCP projects.

- **Features:**
	- Wide range of solutions: databases, security, analytics, developer tools, etc.
	- Automated deployment and configuration.
	- Billing integrated with GCP.
- **Best Practices:**
	- Review solution documentation and pricing before deployment.
	- Use Marketplace for rapid prototyping and standardized deployments.
- **gcloud Examples:**
	- List available Marketplace solutions:
		```sh
		gcloud marketplace solutions list
		```
	- Deploy a Marketplace solution (example):
		```sh
		gcloud marketplace solutions install SOLUTION_ID --project=PROJECT_ID
		```

---

## Cloud Build
Fully managed CI/CD service for building, testing, and deploying code. Supports build triggers, pipelines, and custom build steps.

- **Features:**
	- Integrates with GitHub, Bitbucket, Cloud Source Repositories.
	- Supports Docker, Maven, Gradle, npm, and custom build steps.
	- Build triggers for automated builds on code changes.
- **Best Practices:**
	- Use build triggers for automated CI/CD.
	- Store build configs (`cloudbuild.yaml`) in source control.
- **gcloud Examples:**
	- Submit a build using a config file:
		```sh
		gcloud builds submit --config=cloudbuild.yaml .
		```
	- List build triggers:
		```sh
		gcloud beta builds triggers list
		```
	- Create a build trigger (GitHub example):
		```sh
		gcloud beta builds triggers create github --repo-name=REPO --repo-owner=OWNER --branch-pattern="main" --build-config=cloudbuild.yaml
		```

---

## Cloud Source Repositories
Fully managed, private Git repositories hosted on GCP. Integrated with Cloud Build, Cloud Debugger, and other GCP services.

- **Features:**
	- Unlimited private Git repositories.
	- Integration with GitHub and Bitbucket (mirror repos).
	- Cloud Build triggers for CI/CD.
	- IAM-based access control.
	- Integration with Cloud Shell and local IDEs.
- **Best Practices:**
	- Use for GCP-native projects and infrastructure code.
	- Set up Cloud Build triggers for automated deployments.
	- Use IAM to control repository access.
- **gcloud Examples:**
	- Create a repository:
		```sh
		gcloud source repos create my-repo
		```
	- Clone a repository:
		```sh
		gcloud source repos clone my-repo
		```
	- List repositories:
		```sh
		gcloud source repos list
		```

---

## Terraform with Google Cloud
Infrastructure as Code (IaC) tool for provisioning and managing GCP resources. Alternative to Deployment Manager.

- **Features:**
	- Declarative configuration using HashiCorp Configuration Language (HCL).
	- Multi-cloud support (GCP, AWS, Azure, etc.).
	- State management for tracking infrastructure.
	- Modules for reusable infrastructure patterns.
	- Plan and apply workflow for safe changes.
- **Best Practices:**
	- Store Terraform state in Cloud Storage with state locking.
	- Use Terraform modules for common patterns.
	- Use workspaces for multiple environments (dev, staging, prod).
	- Store Terraform code in version control.
- **Basic Terraform Example:**
	```hcl
	# provider.tf
	provider "google" {
	  project = "my-project"
	  region  = "us-central1"
	}

	# main.tf
	resource "google_compute_instance" "vm" {
	  name         = "my-vm"
	  machine_type = "e2-medium"
	  zone         = "us-central1-a"

	  boot_disk {
	    initialize_params {
	      image = "debian-cloud/debian-11"
	    }
	  }

	  network_interface {
	    network = "default"
	  }
	}
	```
- **Common Commands:**
	- Initialize Terraform:
		```sh
		terraform init
		```
	- Plan changes:
		```sh
		terraform plan
		```
	- Apply changes:
		```sh
		terraform apply
		```
	- Destroy infrastructure:
		```sh
		terraform destroy
		```

---

## Key Differences
- **Deployment Manager:** IaC, declarative, GCP-native, YAML/Python configs.
- **Marketplace:** Prebuilt solutions, fast deployment, catalog-driven.
- **Cloud Build:** CI/CD, automation, integrates with repos, supports custom pipelines.

---

## Summary Table

| Feature         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-------------------------|-------------------------------|-----------------------------------------------|
| Deployment Mgr  | IaC, automation         | YAML/Python, declarative       | gcloud deployment-manager deployments create  |
| Marketplace     | Prebuilt solutions      | Catalog, one-click deploy      | gcloud marketplace solutions list             |
| Cloud Build     | CI/CD, pipelines        | Triggers, repo integration     | gcloud builds submit                          |

---

**Tip:** Use Deployment Manager for repeatable infrastructure, Cloud Build for automated CI/CD, and Marketplace for rapid solution deployment. Store configs and build files in source control for traceability.
# 3. Compute Services (Detailed)

Google Cloud offers a range of compute services to suit different application architectures, operational models, and scalability needs. Below are the main compute offerings, their features, use cases, and relevant gcloud commands.

## Compute Engine
Infrastructure as a Service (IaaS) providing customizable virtual machines (VMs) with granular control over OS, networking, and storage.

- **VM Families:**
  - **N1:** Balanced, older generation. Good for general workloads.
  - **N2:** Higher performance, newer CPUs. Suitable for demanding applications.
  - **E2:** Cost-optimized, flexible sizing. Ideal for cost-sensitive workloads.
  - **Custom:** User-defined vCPU/memory for precise resource allocation.
  - **Spot/Preemptible:** Short-lived, low-cost VMs for batch/interruptible workloads.
- **Instance Groups:**
  - **Managed:** Autoscaling, autohealing, rolling updates. Best for stateless, scalable apps.
  - **Unmanaged:** Manual VM management, for legacy or stateful workloads.
- **Best Practices:**
  - Use managed instance groups for high availability.
  - Use labels for cost tracking and automation.
  - Use preemptible VMs for non-critical, batch jobs to save costs.
- **gcloud Examples:**
  - Create a VM:
    ```sh
    gcloud compute instances create my-vm --zone=us-central1-a --machine-type=e2-medium --image-family=debian-11 --image-project=debian-cloud
    ```
  - List VMs:
    ```sh
    gcloud compute instances list
    ```
  - Create a managed instance group:
    ```sh
    gcloud compute instance-groups managed create my-group --base-instance-name=my-vm --size=3 --template=my-template --zone=us-central1-a
    ```

---

## Google Kubernetes Engine (GKE)
Managed Kubernetes (K8s) clusters for containerized workloads. Offers both node management and serverless (Autopilot) options.

- **Cluster Types:**
  - **Standard:** User manages node pools, more control.
  - **Autopilot:** Google manages nodes, pay-per-pod, simplified operations.
  - **Regional:** Multi-zone, high availability.
  - **Zonal:** Single zone, lower cost.
  - **Private Clusters:** Nodes have no public IPs, enhanced security.
- **Best Practices:**
  - Use regional clusters for production workloads.
  - Enable node auto-upgrade and auto-repair.
  - Use Workload Identity for secure service-to-service auth.
- **gcloud Examples:**
  - Create a standard cluster:
    ```sh
    gcloud container clusters create my-cluster --zone=us-central1-a
    ```
  - Create an Autopilot cluster:
    ```sh
    gcloud container clusters create-auto my-autopilot-cluster --region=us-central1
    ```
  - List clusters:
    ```sh
    gcloud container clusters list
    ```

---

## App Engine
Platform as a Service (PaaS) for deploying web apps and APIs without managing infrastructure.

- **Environments:**
	- **Standard:** Sandboxed, supports specific runtimes (Python, Java, Go, Node.js, PHP, Ruby), scales to zero, fast startup, lower cost.
	- **Flexible:** Docker-based, custom runtimes, more control, supports background processes, SSH access, no scale to zero.
- **Key Concepts:**
	- **Services:** Independent microservices within an app (formerly called modules).
	- **Versions:** Different versions of a service for A/B testing or gradual rollout.
	- **Instances:** Running copies of a version.
	- **Traffic Splitting:** Route traffic between versions (IP, cookie, or random).
- **Features:**
	- Automatic scaling, load balancing, health checks.
	- Built-in security (DDoS protection, SSL/TLS).
	- Application versioning and traffic splitting.
	- Integration with Cloud services (Cloud SQL, Memorystore, Cloud Storage).
- **Best Practices:**
	- Use Standard for rapid scaling and cost efficiency.
	- Use Flexible for custom dependencies or background processing.
	- Use traffic splitting for canary deployments.
	- One App Engine app per project (cannot be deleted, only disabled).
- **gcloud Examples:**
	- Deploy to App Engine (Standard or Flexible):
		```sh
		gcloud app deploy
		```
	- Deploy a specific service:
		```sh
		gcloud app deploy service.yaml
		```
	- List App Engine services:
		```sh
		gcloud app services list
		```
	- Split traffic between versions:
		```sh
		gcloud app services set-traffic my-service --splits=v1=0.9,v2=0.1
		```
	- List versions:
		```sh
		gcloud app versions list
		```
	- Stop a version:
		```sh
		gcloud app versions stop v1 --service=my-service
		```

---

## Cloud Run
Fully managed compute for stateless containers. Scales to zero, pay-per-request, HTTP only.

- **Features:**
	- Deploy containers without managing servers.
	- Supports any language/runtime via containerization.
	- Integrated with IAM for secure access.
	- **Revisions:** Immutable snapshots of configuration/code.
	- **Traffic splitting:** Route traffic between revisions for canary/blue-green deployments.
	- **Concurrency:** Handle multiple requests per instance (default 80, max 1000).
	- Auto-scaling from 0 to 1000+ instances.
	- Minimum instances to reduce cold starts.
- **Best Practices:**
	- Use for microservices, APIs, and event-driven workloads.
	- Use revisions for safe rollouts and rollbacks.
	- Set appropriate concurrency based on workload (CPU vs I/O bound).
	- Use minimum instances for latency-sensitive apps.
	- Store images in Artifact Registry.
- **gcloud Examples:**
	- Deploy a container to Cloud Run:
		```sh
		gcloud run deploy my-service \
			--image=us-central1-docker.pkg.dev/PROJECT_ID/REPO/my-image \
			--region=us-central1 \
			--platform=managed \
			--allow-unauthenticated
		```
	- Deploy with minimum instances (reduce cold starts):
		```sh
		gcloud run deploy my-service \
			--image=us-central1-docker.pkg.dev/PROJECT_ID/REPO/my-image \
			--region=us-central1 \
			--min-instances=1 \
			--max-instances=100
		```
	- Split traffic between revisions:
		```sh
		gcloud run services update-traffic my-service \
			--to-revisions=my-service-v2=50,my-service-v1=50 \
			--region=us-central1
		```
	- List Cloud Run services:
		```sh
		gcloud run services list --region=us-central1
		```
	- List revisions:
		```sh
		gcloud run revisions list --service=my-service --region=us-central1
		```

---

## Cloud Functions
Event-driven, serverless functions for lightweight, single-purpose code triggered by events (HTTP, Pub/Sub, Cloud Storage, etc.).

- **Generations:**
	- **1st gen:** Original Cloud Functions, limited to 540s timeout, fewer triggers.
	- **2nd gen:** Built on Cloud Run, up to 60min timeout, more CPU/memory, all Eventarc triggers, better concurrency.
	- **Recommendation:** Use 2nd gen for new functions (better performance, more features).
- **Features:**
	- Automatic scaling, pay-per-use.
	- Supports multiple runtimes (Node.js, Python, Go, Java, .NET, Ruby, PHP).
	- Integrated with Eventarc for event routing.
	- Minimum instances (reduce cold starts) and maximum instances (control costs).
- **Common Triggers:**
	- HTTP/HTTPS requests
	- Cloud Pub/Sub messages
	- Cloud Storage events (create, delete, archive, metadata update)
	- Firestore events
	- Firebase events
	- Cloud Scheduler (cron jobs)
- **Best Practices:**
	- Use for lightweight, stateless, event-driven tasks.
	- Keep functions small and single-purpose.
	- Use 2nd gen for longer-running tasks (up to 60 minutes).
	- Set minimum instances for latency-sensitive workloads.
	- Use environment variables and Secret Manager for configuration.
- **gcloud Examples:**
	- Deploy a function (2nd gen, HTTP trigger):
		```sh
		gcloud functions deploy my-function \
			--gen2 \
			--runtime=python311 \
			--trigger-http \
			--allow-unauthenticated \
			--region=us-central1
		```
	- Deploy a function (Pub/Sub trigger):
		```sh
		gcloud functions deploy my-function \
			--gen2 \
			--runtime=python311 \
			--trigger-topic=my-topic \
			--region=us-central1
		```
	- Deploy a function (Cloud Storage trigger):
		```sh
		gcloud functions deploy my-function \
			--gen2 \
			--runtime=python311 \
			--trigger-event-filters="type=google.cloud.storage.object.v1.finalized" \
			--trigger-event-filters="bucket=my-bucket" \
			--region=us-central1
		```
	- List functions:
		```sh
		gcloud functions list --region=us-central1
		```
	- Set minimum instances (reduce cold starts):
		```sh
		gcloud functions deploy my-function \
			--gen2 \
			--runtime=python311 \
			--trigger-http \
			--min-instances=1 \
			--region=us-central1
		```

---

## Instance Templates & Metadata
Instance templates define VM configuration for creating instances and managed instance groups. Metadata provides instance-specific configuration.

### Instance Templates
- **Features:**
	- Define machine type, image, disk, network, and metadata.
	- Used by managed instance groups for consistent VM creation.
	- Immutable (cannot be edited, must create new version).
- **Best Practices:**
	- Use templates for all managed instance groups.
	- Version templates for rolling updates.
- **gcloud Examples:**
	- Create an instance template:
		```sh
		gcloud compute instance-templates create my-template \
			--machine-type=e2-medium \
			--image-family=debian-11 \
			--image-project=debian-cloud \
			--boot-disk-size=10GB
		```
	- List instance templates:
		```sh
		gcloud compute instance-templates list
		```

### Instance Metadata & Startup Scripts
- **Metadata:**
	- Key-value pairs attached to instances.
	- Accessible from within the VM via metadata server.
	- Used for configuration, startup scripts, custom data.
- **Startup Scripts:**
	- Shell scripts that run when an instance starts or restarts.
	- Specified via metadata key `startup-script` or `startup-script-url`.
	- Useful for automation, configuration, software installation.
- **Best Practices:**
	- Use startup scripts for initial configuration.
	- Store scripts in Cloud Storage and reference via URL.
	- Check startup script logs in `/var/log/syslog` or Logging.
- **gcloud Examples:**
	- Add metadata to an instance:
		```sh
		gcloud compute instances add-metadata my-vm \
			--metadata=KEY=VALUE \
			--zone=us-central1-a
		```
	- Set a startup script:
		```sh
		gcloud compute instances create my-vm \
			--metadata=startup-script='#!/bin/bash
apt-get update
apt-get install -y nginx' \
			--zone=us-central1-a
		```
	- Set startup script from Cloud Storage:
		```sh
		gcloud compute instances create my-vm \
			--metadata=startup-script-url=gs://my-bucket/startup.sh \
			--zone=us-central1-a
		```

---

## Live Migration & Availability
Google Cloud's live migration technology keeps VMs running during host maintenance.

- **Live Migration:**
	- VMs are automatically migrated to healthy hardware during maintenance.
	- No downtime or reboot for most instance types.
	- Enabled by default for standard instances.
	- Not available for preemptible/spot VMs or instances with GPUs (on older GPU types).
- **Best Practices:**
	- Design applications to handle occasional performance variations during migration.
	- Use managed instance groups for additional resilience.
- **Note:** No gcloud commands needed; live migration is automatic.

---

## Preemptible & Spot VMs
Low-cost, short-lived VMs for fault-tolerant, batch, or interruptible workloads.

- **Preemptible VMs (Legacy):**
	- Up to 80% discount vs standard VMs.
	- Can be terminated at any time by Google (24-hour max runtime).
	- 30-second termination warning.
	- No SLA, no live migration.
- **Spot VMs (Recommended):**
	- Similar pricing to preemptible VMs (up to 91% discount).
	- Can be terminated at any time (no 24-hour limit).
	- More flexible termination policies.
	- Supports instance groups and autoscaling.
- **Best Practices:**
	- Use for batch processing, rendering, data analysis, CI/CD.
	- Design applications to handle interruptions gracefully.
	- Save state frequently and use checkpointing.
	- Prefer Spot VMs over preemptible for new workloads.
- **gcloud Examples:**
	- Create a preemptible VM:
		```sh
		gcloud compute instances create my-preemptible-vm \
			--preemptible \
			--zone=us-central1-a
		```
	- Create a Spot VM:
		```sh
		gcloud compute instances create my-spot-vm \
			--provisioning-model=SPOT \
			--instance-termination-action=STOP \
			--zone=us-central1-a
		```

---

## Committed Use Discounts & Sustained Use Discounts
Cost-saving pricing models for predictable and long-running workloads.

### Committed Use Discounts (CUDs)
- **Features:**
	- 1-year or 3-year commitment for specific resources.
	- Up to 57% discount for Compute Engine and up to 70% for memory-optimized.
	- Apply to vCPU, memory, GPU, and local SSD.
	- Committed at project or folder level.
- **Best Practices:**
	- Use for steady-state, predictable workloads.
	- Analyze usage patterns before committing.
	- Combine with flexible CUDs (spend-based) for dynamic workloads.
- **gcloud Examples:**
	- Create a committed use discount:
		```sh
		gcloud compute commitments create my-commitment \
			--region=us-central1 \
			--resources=vcpu=8,memory=32GB \
			--plan=12-month
		```
	- List commitments:
		```sh
		gcloud compute commitments list
		```

### Sustained Use Discounts (SUDs)
- **Features:**
	- Automatic discounts for VMs running >25% of the month.
	- Up to 30% discount for instances running continuously.
	- Applied automatically, no commitment required.
- **Best Practices:**
	- No action needed; discounts apply automatically.
	- Review billing reports to see SUD savings.

---

## Shielded VMs
Hardened VMs with verifiable integrity and protection against rootkits and bootkits.

- **Features:**
	- **Secure Boot:** Ensures only authenticated OS software runs.
	- **vTPM (Virtual Trusted Platform Module):** Protects keys and certificates.
	- **Integrity Monitoring:** Validates boot integrity.
	- Available for most instance types.
- **Best Practices:**
	- Enable for production workloads requiring enhanced security.
	- Monitor integrity validation reports.
	- Use with Container-Optimized OS for GKE nodes.
- **gcloud Examples:**
	- Create a Shielded VM:
		```sh
		gcloud compute instances create my-shielded-vm \
			--zone=us-central1-a \
			--shielded-secure-boot \
			--shielded-vtpm \
			--shielded-integrity-monitoring
		```
	- Update Shielded VM settings:
		```sh
		gcloud compute instances update my-vm --shielded-learn-integrity-policy
		```

---

## Confidential VMs
VMs with memory encryption for data-in-use protection. Part of Google's Confidential Computing portfolio.

- **Features:**
	- Encrypts data while being processed (not just at rest or in transit).
	- Uses AMD SEV (Secure Encrypted Virtualization).
	- No code changes required for applications.
	- Performance overhead minimal (typically <10%).
- **Best Practices:**
	- Use for highly sensitive workloads (healthcare, finance, regulated data).
	- Select N2D machine types (AMD-based).
	- Combine with Shielded VM features.
- **gcloud Examples:**
	- Create a Confidential VM:
		```sh
		gcloud compute instances create my-confidential-vm \
			--zone=us-central1-a \
			--machine-type=n2d-standard-2 \
			--confidential-compute \
			--maintenance-policy=TERMINATE
		```

---

## Container-Optimized OS (COS)
Google-maintained OS image optimized for running containers on Compute Engine and GKE.

- **Features:**
	- Minimal OS footprint (smaller attack surface).
	- Automatic security updates.
	- Docker pre-installed.
	- Optimized for performance and security.
	- Read-only root filesystem.
- **Best Practices:**
	- Use for GKE node pools.
	- Use for containerized workloads on Compute Engine.
	- Preferred over general-purpose OS images for containers.
- **gcloud Examples:**
	- Create a VM with Container-Optimized OS:
		```sh
		gcloud compute instances create-with-container my-container-vm \
			--zone=us-central1-a \
			--container-image=gcr.io/my-project/my-image
		```

---

## Sole-tenant Nodes
Physical servers dedicated to a single customer. Useful for compliance, licensing, and performance isolation.

- **Features:**
	- Physical isolation from other customers.
	- Meet compliance requirements (HIPAA, PCI-DSS).
	- Bring-your-own-license (BYOL) for per-core/per-processor software.
	- Define node templates and groups.
- **Best Practices:**
	- Use for workloads requiring physical isolation.
	- Use for license-restricted software.
	- Group VMs on same node for performance.
- **gcloud Examples:**
	- Create a sole-tenant node template:
		```sh
		gcloud compute sole-tenancy node-templates create my-template \
			--node-type=n1-node-96-624
		```
	- Create a sole-tenant node group:
		```sh
		gcloud compute sole-tenancy node-groups create my-group \
			--zone=us-central1-a \
			--node-template=my-template \
			--target-size=3
		```
	- Create a VM on sole-tenant node:
		```sh
		gcloud compute instances create my-vm \
			--zone=us-central1-a \
			--node-group=my-group
		```

---

## Key Differences
- **Compute Engine:** Full VM control, most flexibility, manual scaling.
- **GKE:** Container orchestration, Kubernetes API, managed clusters.
- **App Engine:** Simplified deployment, less control, automatic scaling.
- **Cloud Run:** Containers, HTTP only, no server management, scales to zero.
- **Cloud Functions:** Small, event-driven code, no server management, pay-per-use.

---

## Summary Table

| Service         | Type      | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-----------|-------------------------|-------------------------------|-----------------------------------------------|
| Compute Engine  | IaaS      | Custom VMs, full control| Manual scaling, OS choice      | gcloud compute instances create               |
| GKE             | CaaS      | Kubernetes, containers  | Managed clusters, K8s API      | gcloud container clusters create              |
| App Engine      | PaaS      | Web apps, APIs          | Standard vs Flexible, scaling  | gcloud app deploy                             |
| Cloud Run       | Serverless| Containers, HTTP        | Stateless, scales to zero      | gcloud run deploy                             |
| Cloud Functions | Serverless| Event-driven, micro     | Triggers, pay-per-use          | gcloud functions deploy                       |

---

**Tip:** Choose the compute service that best matches your operational model, scalability needs, and management preferences. Use automation and labels for cost and resource tracking.
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
  - **Standard:** Sandboxed, supports specific runtimes (Python, Java, Go, Node.js), scales to zero, fast startup.
  - **Flexible:** Docker-based, custom runtimes, more control, supports background processes.
- **Best Practices:**
  - Use Standard for rapid scaling and cost efficiency.
  - Use Flexible for custom dependencies or background processing.
- **gcloud Examples:**
  - Deploy to App Engine (Standard or Flexible):
    ```sh
    gcloud app deploy
    ```
  - List App Engine services:
    ```sh
    gcloud app services list
    ```

---

## Cloud Run
Fully managed compute for stateless containers. Scales to zero, pay-per-request, HTTP only.

- **Features:**
  - Deploy containers without managing servers.
  - Supports any language/runtime via containerization.
  - Integrated with IAM for secure access.
- **Best Practices:**
  - Use for microservices, APIs, and event-driven workloads.
  - Use revisions for safe rollouts and rollbacks.
- **gcloud Examples:**
  - Deploy a container to Cloud Run:
    ```sh
    gcloud run deploy my-service --image=gcr.io/PROJECT_ID/my-image --region=us-central1 --platform=managed
    ```
  - List Cloud Run services:
    ```sh
    gcloud run services list --region=us-central1
    ```

---

## Cloud Functions
Event-driven, serverless functions for lightweight, single-purpose code triggered by events (HTTP, Pub/Sub, Cloud Storage, etc.).

- **Features:**
  - Automatic scaling, pay-per-use.
  - Supports multiple runtimes (Node.js, Python, Go, Java, etc.).
  - Integrated with Eventarc for event routing.
- **Best Practices:**
  - Use for lightweight, stateless, event-driven tasks.
  - Keep functions small and single-purpose.
- **gcloud Examples:**
  - Deploy a function (HTTP trigger):
    ```sh
    gcloud functions deploy my-function --runtime=python310 --trigger-http --allow-unauthenticated --region=us-central1
    ```
  - List functions:
    ```sh
    gcloud functions list --region=us-central1
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
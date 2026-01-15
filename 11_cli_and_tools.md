# 11. CLI & Tools (Detailed)

Google Cloud provides several command-line tools for managing resources, automating workflows, and interacting with services. Below are the main tools, their features, use cases, and relevant commands.

## gcloud
The main CLI for managing GCP resources (projects, IAM, compute, networking, storage, etc.). Supports scripting, automation, and interactive use.

- **Features:**
	- Manage almost all GCP resources from the command line.
	- Supports authentication, configuration profiles, and output formatting (JSON, YAML, text).
	- Can be used in Cloud Shell or installed locally via Cloud SDK.
- **Best Practices:**
	- Use configuration profiles for managing multiple projects/environments.
	- Script repetitive tasks for automation.
- **gcloud Examples:**
	- List all projects:
		```sh
		gcloud projects list
		```
	- Set the active project:
		```sh
		gcloud config set project PROJECT_ID
		```
	- List compute instances:
		```sh
		gcloud compute instances list
		```

---

## kubectl
CLI for interacting with Kubernetes clusters (GKE or other). Used to deploy, manage, and troubleshoot Kubernetes resources (pods, services, deployments, etc.).

- **Features:**
	- Works with kubeconfig files for cluster authentication.
	- Supports declarative and imperative resource management.
- **Best Practices:**
	- Use declarative manifests (YAML) for version control and repeatability.
	- Use namespaces to organize resources.
- **kubectl Examples:**
	- List all pods in a namespace:
		```sh
		kubectl get pods -n NAMESPACE
		```
	- Apply a deployment manifest:
		```sh
		kubectl apply -f deployment.yaml
		```
	- Get cluster info:
		```sh
		kubectl cluster-info
		```

---

## When to Use gcloud vs kubectl (Important for Exam!)

Understanding when to use `gcloud` vs `kubectl` is critical for GKE-related exam questions.

### Use `gcloud` for:
- **GKE cluster management** (infrastructure layer):
	- Creating, updating, deleting GKE clusters
	- Managing node pools (add, delete, resize)
	- Configuring cluster settings (autoscaling, network policy, Workload Identity)
	- Getting cluster credentials
	- Upgrading cluster version
	- Viewing cluster info
- **Examples:**
	```sh
	gcloud container clusters create my-cluster
	gcloud container clusters get-credentials my-cluster --zone=us-central1-a
	gcloud container node-pools create my-pool --cluster=my-cluster
	gcloud container clusters upgrade my-cluster
	```

### Use `kubectl` for:
- **Kubernetes workload management** (application layer):
	- Deploying applications (pods, deployments, statefulsets)
	- Managing services (ClusterIP, NodePort, LoadBalancer)
	- Creating config maps and secrets
	- Scaling deployments
	- Viewing logs and troubleshooting pods
	- Applying Kubernetes manifests (YAML)
	- Managing namespaces, ingresses, persistent volumes
- **Examples:**
	```sh
	kubectl create deployment my-app --image=gcr.io/project/image
	kubectl expose deployment my-app --type=LoadBalancer --port=80
	kubectl scale deployment my-app --replicas=3
	kubectl get pods
	kubectl logs my-pod
	kubectl apply -f deployment.yaml
	```

### Quick Decision Tree:
- **Question about the cluster itself?** → Use `gcloud`
- **Question about apps/workloads running IN the cluster?** → Use `kubectl`
- **Managing GKE infrastructure?** → `gcloud`
- **Managing Kubernetes resources?** → `kubectl`

### Common Exam Scenario:
**"You need to deploy a new application to GKE and expose it to the internet."**
1. ✅ `gcloud container clusters get-credentials my-cluster` (get access to cluster)
2. ✅ `kubectl create deployment my-app --image=...` (deploy app)
3. ✅ `kubectl expose deployment my-app --type=LoadBalancer` (expose to internet)

**NOT:** `gcloud container clusters deploy ...` ❌ (doesn't exist)

---

## gsutil
CLI for Cloud Storage operations (upload, download, sync, ACLs, lifecycle management). Optimized for large data transfers and scripting.

- **Features:**
	- Supports parallel transfers, resumable uploads, and scripting.
	- Manage bucket and object ACLs, lifecycle rules, and metadata.
- **Best Practices:**
	- Use `-m` for parallel operations on large datasets.
	- Use lifecycle rules for automated data management.
- **gsutil Examples:**
	- Upload a file to a bucket:
		```sh
		gsutil cp file.txt gs://my-bucket/
		```
	- Sync a local folder to a bucket:
		```sh
		gsutil -m rsync -r ./local-folder gs://my-bucket/
		```
	- List all buckets:
		```sh
		gsutil ls
		```

---

## bq
CLI for BigQuery operations (query, load, export, manage datasets/tables). Supports SQL queries, job management, and data transfers.

- **Features:**
	- Run interactive and batch SQL queries.
	- Load/export data to/from BigQuery tables.
	- Manage datasets, tables, and jobs.
- **Best Practices:**
	- Use parameterized queries for automation.
	- Export query results to Cloud Storage for sharing or further analysis.
- **bq Examples:**
	- Run a SQL query:
		```sh
		bq query 'SELECT COUNT(*) FROM `my_project.my_dataset.my_table`'
		```
	- List datasets:
		```sh
		bq ls
		```
	- Load data from Cloud Storage:
		```sh
		bq load --source_format=CSV my_dataset.my_table gs://my-bucket/data.csv
		```

---

## Key Differences
- **gcloud:** General GCP management, broadest scope, supports most services.
- **kubectl:** Kubernetes-specific, works with GKE and other clusters.
- **gsutil:** Cloud Storage-specific, optimized for large data transfers and scripting.
- **bq:** BigQuery-specific, SQL analytics and data management.

---

## Summary Table

| Tool     | Main Use                | Key Differences                | Example Command                               |
|----------|-------------------------|-------------------------------|-----------------------------------------------|
| gcloud   | GCP resource mgmt       | Broad, scripting, automation   | gcloud projects list                          |
| kubectl  | Kubernetes mgmt         | K8s-specific, clusters         | kubectl get pods                              |
| gsutil   | Cloud Storage ops       | Data transfer, storage         | gsutil cp                                     |
| bq       | BigQuery ops            | SQL, analytics, data mgmt      | bq query                                      |

---

**Tip:** Use the right CLI for the right job. Automate repetitive tasks, use configuration profiles, and store scripts in version control for reliability and auditability.
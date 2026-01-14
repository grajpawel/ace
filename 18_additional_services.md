# 18. Additional Services & Features (Detailed)

This file covers additional GCP services and features that are important for the Associate Cloud Engineer exam.

## Cloud Scheduler
Fully managed cron job service for scheduling tasks, including HTTP/S endpoints, Pub/Sub topics, and App Engine applications.

- **Features:**
	- Cron-based scheduling (Unix cron format).
	- Triggers HTTP/S endpoints, Pub/Sub messages, App Engine tasks.
	- Automatic retries with exponential backoff.
	- Time zone support.
	- High availability and reliability.
- **Best Practices:**
	- Use for scheduled data pipelines, batch jobs, periodic maintenance.
	- Combine with Cloud Functions or Cloud Run for serverless scheduled tasks.
	- Set appropriate retry policies.
	- Monitor job execution via Cloud Logging.
- **gcloud Examples:**
	- Create an HTTP target job:
		```sh
		gcloud scheduler jobs create http my-job \
			--schedule="0 2 * * *" \
			--uri="https://example.com/task" \
			--http-method=POST \
			--location=us-central1
		```
	- Create a Pub/Sub target job:
		```sh
		gcloud scheduler jobs create pubsub my-pubsub-job \
			--schedule="*/10 * * * *" \
			--topic=my-topic \
			--message-body="Scheduled message" \
			--location=us-central1
		```
	- List jobs:
		```sh
		gcloud scheduler jobs list --location=us-central1
		```
	- Run a job manually:
		```sh
		gcloud scheduler jobs run my-job --location=us-central1
		```

---

## Cloud Endpoints & API Gateway
Services for developing, deploying, and managing APIs.

### Cloud Endpoints
API management platform for REST and gRPC APIs. Provides monitoring, authentication, and API key validation.

- **Features:**
	- API key validation and management.
	- Authentication (API keys, JWT, Firebase, Auth0).
	- Monitoring and logging.
	- Supports App Engine, GKE, Compute Engine.
	- OpenAPI and gRPC API specifications.
- **Best Practices:**
	- Use for microservices APIs requiring authentication.
	- Monitor API usage and performance.
	- Implement rate limiting and quotas.
- **Deployment:**
	- Configure via OpenAPI/gRPC spec files.
	- Deploy using `gcloud endpoints services deploy`.

### API Gateway
Fully managed gateway for serverless backends (Cloud Functions, Cloud Run).

- **Features:**
	- API key validation.
	- Request/response transformation.
	- CORS support.
	- Integration with Cloud Functions and Cloud Run.
- **Best Practices:**
	- Use for exposing serverless backends as managed APIs.
	- Implement authentication and rate limiting.
- **gcloud Examples:**
	- Create an API config:
		```sh
		gcloud api-gateway api-configs create my-config \
			--api=my-api \
			--openapi-spec=api-spec.yaml \
			--backend-auth-service-account=SA@PROJECT.iam.gserviceaccount.com
		```
	- Create a gateway:
		```sh
		gcloud api-gateway gateways create my-gateway \
			--api=my-api \
			--api-config=my-config \
			--location=us-central1
		```

---

## Cloud Run Jobs
Run containers to completion for batch processing, data processing, and one-time tasks.

- **Features:**
	- Containers that run to completion (not long-running services).
	- Automatic retries and parallelism.
	- Scheduled execution via Cloud Scheduler.
	- Pay only for execution time.
- **Best Practices:**
	- Use for batch jobs, ETL tasks, periodic processing.
	- Combine with Cloud Scheduler for cron-like execution.
	- Set appropriate timeout and retry policies.
- **gcloud Examples:**
	- Create a job:
		```sh
		gcloud run jobs create my-job \
			--image=gcr.io/PROJECT/my-image \
			--region=us-central1 \
			--task-timeout=10m
		```
	- Execute a job:
		```sh
		gcloud run jobs execute my-job --region=us-central1
		```
	- List jobs:
		```sh
		gcloud run jobs list --region=us-central1
		```

---

## Cloud Deploy
Managed continuous delivery service for GKE and Cloud Run. Automates deployment pipelines.

- **Features:**
	- Progressive delivery (canary, blue-green deployments).
	- Declarative delivery pipelines.
	- Integration with Cloud Build, Artifact Registry.
	- Automated rollbacks.
	- Approval gates for production deployments.
- **Best Practices:**
	- Use for GitOps-based continuous delivery.
	- Implement progressive delivery patterns for safer rollouts.
	- Define separate targets for dev, staging, prod.
- **gcloud Examples:**
	- Create a delivery pipeline:
		```sh
		gcloud deploy apply --file=clouddeploy.yaml --region=us-central1
		```
	- Create a release:
		```sh
		gcloud deploy releases create release-001 \
			--delivery-pipeline=my-pipeline \
			--region=us-central1 \
			--images=my-app=us-central1-docker.pkg.dev/PROJECT/REPO/IMAGE:TAG
		```
	- List pipelines:
		```sh
		gcloud deploy delivery-pipelines list --region=us-central1
		```

---

## Cloud Asset Inventory
Service for searching, discovering, and managing GCP resources and metadata.

- **Features:**
	- Query all resources across projects and organizations.
	- Export asset history and metadata.
	- Monitor asset changes over time.
	- Search by asset type, labels, properties.
	- Integration with Security Command Center.
- **Best Practices:**
	- Use for compliance reporting and auditing.
	- Monitor resource changes for security.
	- Export asset inventory regularly for backup.
- **gcloud Examples:**
	- Search assets:
		```sh
		gcloud asset search-all-resources --scope=projects/PROJECT_ID --query="name:my-vm"
		```
	- Export assets:
		```sh
		gcloud asset export \
			--output-path=gs://my-bucket/assets.json \
			--content-type=resource \
			--project=PROJECT_ID
		```
	- List assets:
		```sh
		gcloud asset list --project=PROJECT_ID --asset-types=compute.googleapis.com/Instance
		```

---

## Recommendations & Recommender API
AI-powered recommendations for cost optimization, security, performance, and sustainability.

- **Recommendation Types:**
	- **Cost:** Idle resources, right-sizing VMs, committed use discounts.
	- **Security:** IAM policy changes, firewall rules, overly permissive access.
	- **Performance:** VM machine type upgrades, disk type changes.
	- **Sustainability:** Move to lower-carbon regions.
- **Features:**
	- Automated recommendations across multiple categories.
	- Apply recommendations with one click or via API.
	- Track recommendation history.
- **Best Practices:**
	- Review recommendations regularly (weekly/monthly).
	- Prioritize cost and security recommendations.
	- Test recommendations in dev before applying to prod.
- **gcloud Examples:**
	- List cost recommendations:
		```sh
		gcloud recommender recommendations list \
			--project=PROJECT_ID \
			--location=us-central1 \
			--recommender=google.compute.instance.MachineTypeRecommender
		```
	- Mark recommendation as claimed:
		```sh
		gcloud recommender recommendations mark-claimed RECOMMENDATION_ID \
			--project=PROJECT_ID \
			--location=us-central1 \
			--recommender=google.compute.instance.MachineTypeRecommender
		```

---

## Shared Responsibility Model
Understanding the division of security responsibilities between Google and customers.

### Google's Responsibilities:
- Physical security of data centers
- Hardware and infrastructure security
- Network infrastructure security
- Hypervisor and host OS security
- Encryption at rest (default) and in transit
- Platform services and APIs

### Customer's Responsibilities:
- IAM and access control
- Data classification and encryption (CMEK)
- Application security and code
- Network configuration (VPC, firewall rules)
- OS and software patching (on Compute Engine)
- Logging and monitoring configuration
- Compliance and regulatory requirements

### Shared Responsibilities:
- Network security (Google provides infrastructure, customer configures)
- Identity and access (Google provides IAM, customer manages policies)
- Data protection (Google encrypts, customer controls keys and access)

**Exam Tip:** Know that Google manages the infrastructure, but customers are responsible for configuring security properly (IAM, firewall rules, encryption keys, etc.).

---

## Free Tier & Trial Credits
Understanding what's available for free helps with cost-effective architecture decisions.

### Always Free Tier
Available indefinitely, even after trial expires:
- **Compute Engine:** 1 f1-micro VM instance per month (US regions only)
- **Cloud Storage:** 5 GB standard storage
- **BigQuery:** 1 TB queries per month, 10 GB storage
- **Cloud Functions:** 2 million invocations per month
- **Cloud Run:** 2 million requests per month
- **Cloud Pub/Sub:** 10 GB messages per month
- **Cloud Firestore:** 1 GB storage, 50K reads, 20K writes per day

### Free Trial
- $300 credit for 90 days
- Access to all GCP services
- No automatic billing after trial (must upgrade manually)

**Best Practices:**
- Use Always Free tier for dev/test environments.
- Monitor usage to stay within free limits.
- Understand which services have free tier before architecting.

---

## Important Limits & Quotas to Remember

### Common Default Quotas:
- **Projects:** 25 per billing account (requestable increase)
- **VPCs:** 15 per project
- **Firewall rules:** 200 per VPC
- **Static IP addresses:** Varies by region
- **CPUs (all regions):** Varies by project (typically 24-72)
- **Persistent disk size:** Varies by region
- **Cloud Storage bucket count:** No limit
- **GKE clusters:** 100 per project per region

**Best Practices:**
- Request quota increases before reaching limits.
- Plan architecture considering quota limits.
- Use quota monitoring and alerts.
- Distribute resources across regions if needed.

---

## Summary Table

| Service              | Type                | Use Case                       | Key Differences              |
|----------------------|---------------------|--------------------------------|------------------------------|
| Cloud Scheduler      | Cron service        | Scheduled tasks                | Managed cron, multi-target   |
| Cloud Endpoints      | API management      | REST/gRPC APIs                 | Auth, monitoring, keys       |
| API Gateway          | API gateway         | Serverless API management      | Cloud Functions/Run backend  |
| Cloud Run Jobs       | Batch processing    | Container-based jobs           | Run to completion            |
| Cloud Deploy         | CD service          | Continuous delivery            | Progressive delivery, GKE    |
| Cloud Asset Inventory| Asset management    | Resource discovery             | Search, export, monitoring   |
| Recommender          | AI recommendations  | Cost, security, performance    | Automated optimization       |

---

**Tip:** These services complement core GCP offerings. Cloud Scheduler is frequently tested for scheduled tasks, API Gateway for serverless APIs, and Recommender for cost optimization scenarios.

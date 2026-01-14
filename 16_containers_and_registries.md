# 16. Container Registries & AI/ML Services (Detailed)

## Container Registries

### Artifact Registry
Next-generation container registry and universal package manager. Successor to Container Registry with additional features.

- **Features:**
	- Support for Docker images, Maven, npm, Python, Apt, Yum, and more.
	- Regional and multi-regional repositories.
	- Vulnerability scanning with Container Analysis.
	- IAM integration for fine-grained access control.
	- Immutable image tags.
	- Encryption at rest (Google-managed or customer-managed keys).
- **Best Practices:**
	- Use Artifact Registry for all new projects (Container Registry is deprecated).
	- Enable vulnerability scanning for production images.
	- Use regional repositories for lower latency.
	- Implement image retention policies to manage costs.
- **gcloud Examples:**
	- Create a Docker repository:
		```sh
		gcloud artifacts repositories create my-repo \
			--repository-format=docker \
			--location=us-central1 \
			--description="My Docker repository"
		```
	- List repositories:
		```sh
		gcloud artifacts repositories list --location=us-central1
		```
	- Configure Docker authentication:
		```sh
		gcloud auth configure-docker us-central1-docker.pkg.dev
		```
	- Push an image:
		```sh
		docker tag my-image:latest us-central1-docker.pkg.dev/PROJECT_ID/my-repo/my-image:latest
		docker push us-central1-docker.pkg.dev/PROJECT_ID/my-repo/my-image:latest
		```
	- List images:
		```sh
		gcloud artifacts docker images list us-central1-docker.pkg.dev/PROJECT_ID/my-repo
		```

---

### Container Registry (Legacy)
Original container image registry service. Being replaced by Artifact Registry.

- **Status:** Deprecated. Migrate to Artifact Registry.
- **Features:**
	- Docker image storage in Cloud Storage buckets.
	- Regional, multi-regional, and global storage.
	- Vulnerability scanning available.
- **Migration:**
	- Use `gcloud container images` commands to copy images to Artifact Registry.
	- Update CI/CD pipelines to push to Artifact Registry.

---

## AI & Machine Learning Services

Google Cloud offers a comprehensive suite of AI/ML services for building, deploying, and managing machine learning models.

### Vertex AI
Unified AI platform for building, deploying, and scaling ML models. Combines AutoML and custom training.

- **Features:**
	- AutoML for no-code model training (vision, language, tabular, video).
	- Custom training with pre-built containers or custom containers.
	- Model deployment with online and batch predictions.
	- Feature Store for ML feature management.
	- ML Pipelines for workflow automation.
	- Model monitoring and explainability.
- **Best Practices:**
	- Use AutoML for quick prototyping and standard use cases.
	- Use custom training for specialized models or existing code.
	- Store features in Feature Store for consistency and reuse.
- **gcloud Examples:**
	- Create a dataset:
		```sh
		gcloud ai datasets create --display-name=my-dataset --region=us-central1
		```
	- Train an AutoML model (example):
		```sh
		gcloud ai models upload --region=us-central1 --display-name=my-model --artifact-uri=gs://my-bucket/model/
		```
	- List models:
		```sh
		gcloud ai models list --region=us-central1
		```

---

### Pre-trained AI APIs
Ready-to-use AI services for common tasks without training custom models.

- **Vision API:**
	- Image analysis: label detection, OCR, face detection, landmark detection, object localization.
	- Example use: Content moderation, document processing, image search.
- **Natural Language API:**
	- Text analysis: sentiment, entity extraction, syntax analysis, content classification.
	- Example use: Customer feedback analysis, content categorization.
- **Translation API:**
	- Language translation with 100+ languages.
	- AutoML Translation for custom domain-specific translation.
- **Speech-to-Text API:**
	- Audio transcription with real-time and batch processing.
	- Support for multiple languages and speaker diarization.
- **Text-to-Speech API:**
	- Convert text to natural-sounding speech.
	- Multiple voices and languages.
- **Video Intelligence API:**
	- Video analysis: label detection, shot change detection, explicit content detection, object tracking.

**Best Practices:**
- Use pre-trained APIs for standard use cases to save time and cost.
- Combine multiple APIs for comprehensive solutions.
- Monitor API usage and costs via Cloud Monitoring.

**gcloud Examples:**
- Analyze an image (Vision API):
	```sh
	gcloud ml vision detect-labels gs://my-bucket/image.jpg
	```
- Detect sentiment (Natural Language API):
	```sh
	gcloud ml language analyze-sentiment --content="I love this product!"
	```
- Translate text:
	```sh
	gcloud ml translate translate "Hello world" --target-language=es
	```

---

### AI Platform (Legacy)
Original ML platform for training and deploying models. Functionality migrated to Vertex AI.

- **Status:** Being replaced by Vertex AI.
- **Migration:** Move training and prediction workloads to Vertex AI.

---

## Additional Storage Services

### Filestore
Fully managed NFS file storage for applications requiring file system interface. Ideal for shared storage across VMs.

- **Features:**
	- NFSv3 protocol support.
	- High-performance tiers (Basic, High Scale, Enterprise).
	- Integration with Compute Engine and GKE.
	- Up to 100 TB per instance.
- **Best Practices:**
	- Use for applications requiring shared file storage (content management, media rendering, analytics).
	- Choose tier based on performance and capacity needs.
- **gcloud Examples:**
	- Create a Filestore instance:
		```sh
		gcloud filestore instances create my-filestore \
			--zone=us-central1-a \
			--tier=BASIC_HDD \
			--file-share=name=share1,capacity=1TB \
			--network=name=default
		```
	- List instances:
		```sh
		gcloud filestore instances list
		```

---

### Memorystore
Fully managed in-memory data store for Redis and Memcached. Provides low-latency data caching and storage.

- **Redis Features:**
	- Redis 6.x and 7.x support.
	- Standard (single node) and High Availability (replicated).
	- Up to 300 GB per instance.
	- Automatic failover and backups.
- **Memcached Features:**
	- Memcached 1.5 and 1.6 support.
	- Multi-node for horizontal scaling.
	- Up to 5 TB per instance.
- **Best Practices:**
	- Use Redis for session storage, caching, and pub/sub messaging.
	- Use Memcached for simple distributed caching.
	- Enable HA for production Redis instances.
- **gcloud Examples:**
	- Create a Redis instance:
		```sh
		gcloud redis instances create my-redis \
			--size=1 \
			--region=us-central1 \
			--tier=BASIC
		```
	- Create a Memcached instance:
		```sh
		gcloud memcache instances create my-memcache \
			--node-count=1 \
			--node-cpu=1 \
			--node-memory=1GB \
			--region=us-central1
		```
	- List instances:
		```sh
		gcloud redis instances list --region=us-central1
		gcloud memcache instances list --region=us-central1
		```

---

## Key Differences
- **Artifact Registry vs Container Registry:** Modern, multi-format vs legacy Docker-only.
- **Vertex AI vs Pre-trained APIs:** Custom models vs ready-to-use.
- **Filestore:** Network file system, shared storage.
- **Memorystore:** In-memory cache, low latency.

---

## Summary Table

| Service          | Type              | Use Case                    | Key Differences           | Example gcloud Command                    |
|------------------|-------------------|-----------------------------|---------------------------|-------------------------------------------|
| Artifact Registry| Container/package | Docker, npm, Maven, etc.    | Universal, regional       | gcloud artifacts repositories create      |
| Container Registry| Container (legacy)| Docker images              | Deprecated, migrate       | N/A (use Artifact Registry)               |
| Vertex AI        | ML platform       | Custom ML models            | AutoML, training, deploy  | gcloud ai models list                     |
| Vision API       | Pre-trained AI    | Image analysis              | No training needed        | gcloud ml vision detect-labels            |
| Filestore        | NFS storage       | Shared file storage         | NFSv3, VMs/GKE            | gcloud filestore instances create         |
| Memorystore      | Cache             | Redis/Memcached             | In-memory, low latency    | gcloud redis instances create             |

---

**Tip:** Use Artifact Registry for all container and package management. Leverage Vertex AI for custom ML models and pre-trained APIs for quick AI integration. Use Filestore for shared files and Memorystore for caching.

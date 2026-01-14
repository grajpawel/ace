# 13. Messaging & Integration (Detailed)

Google Cloud provides messaging and integration services for building decoupled, event-driven architectures. Below are the main services, their features, use cases, and relevant gcloud commands.

## Cloud Pub/Sub
Fully managed, real-time messaging service for asynchronous communication between services. Supports both push and pull delivery models.

- **Core Concepts:**
	- **Topics:** Named resources to which publishers send messages.
	- **Subscriptions:** Named resources representing message streams from a topic to subscribers.
	- **Messages:** Data payload with optional attributes.
- **Subscription Types:**
	- **Pull:** Subscriber requests messages on demand (polling).
	- **Push:** Pub/Sub sends messages to HTTPS endpoints automatically.
- **Features:**
	- At-least-once delivery guarantee.
	- Message ordering (with ordering keys).
	- Dead letter topics for undeliverable messages.
	- Message retention (default 7 days, configurable up to 31 days).
	- Message filtering based on attributes.
- **Best Practices:**
	- Use push for low-latency, serverless integrations (Cloud Run, Cloud Functions).
	- Use pull for batch processing or when subscribers control message rate.
	- Enable dead letter queues for error handling.
	- Use message ordering only when necessary (reduces throughput).
- **gcloud Examples:**
	- Create a topic:
		```sh
		gcloud pubsub topics create my-topic
		```
	- List topics:
		```sh
		gcloud pubsub topics list
		```
	- Create a pull subscription:
		```sh
		gcloud pubsub subscriptions create my-subscription --topic=my-topic
		```
	- Create a push subscription:
		```sh
		gcloud pubsub subscriptions create my-push-sub --topic=my-topic --push-endpoint=https://example.com/push
		```
	- Publish a message:
		```sh
		gcloud pubsub topics publish my-topic --message="Hello World"
		```
	- Pull messages:
		```sh
		gcloud pubsub subscriptions pull my-subscription --auto-ack --limit=10
		```

---

## Eventarc
Event routing service that delivers events from Google services, SaaS, and custom sources to Cloud Run, Cloud Functions, and Workflows.

- **Features:**
	- Unified event routing across GCP services.
	- Support for 90+ event sources (Cloud Storage, Pub/Sub, Firestore, etc.).
	- CloudEvents standard format.
	- Advanced filtering and routing rules.
- **Best Practices:**
	- Use for event-driven architectures and serverless workflows.
	- Combine with Cloud Run for scalable event processing.
- **gcloud Examples:**
	- Create a trigger for Cloud Storage events:
		```sh
		gcloud eventarc triggers create my-trigger \
			--location=us-central1 \
			--destination-run-service=my-service \
			--destination-run-region=us-central1 \
			--event-filters="type=google.cloud.storage.object.v1.finalized" \
			--event-filters="bucket=my-bucket"
		```
	- List triggers:
		```sh
		gcloud eventarc triggers list --location=us-central1
		```

---

## Cloud Tasks
Asynchronous task execution service for scheduling and managing distributed task queues. Ideal for offloading work and rate limiting.

- **Features:**
	- HTTP target tasks (invoke endpoints).
	- App Engine tasks (invoke App Engine handlers).
	- Task scheduling with delays and rate limits.
	- Task deduplication.
- **Best Practices:**
	- Use for background jobs, delayed execution, and rate-limited operations.
	- Set appropriate retry policies for resilience.
- **gcloud Examples:**
	- Create a queue:
		```sh
		gcloud tasks queues create my-queue --location=us-central1
		```
	- Create an HTTP task:
		```sh
		gcloud tasks create-http-task my-task \
			--queue=my-queue \
			--url=https://example.com/process \
			--method=POST \
			--location=us-central1
		```
	- List queues:
		```sh
		gcloud tasks queues list --location=us-central1
		```

---

## Workflows
Orchestrate and automate Google Cloud and HTTP-based API services using a YAML/JSON workflow definition language.

- **Features:**
	- Serverless, no infrastructure management.
	- Built-in error handling and retries.
	- Integration with GCP services and external APIs.
	- Conditional logic, loops, and variable support.
- **Best Practices:**
	- Use for multi-step automation and service orchestration.
	- Combine with Eventarc for event-driven workflows.
- **gcloud Examples:**
	- Deploy a workflow:
		```sh
		gcloud workflows deploy my-workflow --source=workflow.yaml --location=us-central1
		```
	- Execute a workflow:
		```sh
		gcloud workflows run my-workflow --location=us-central1
		```
	- List workflows:
		```sh
		gcloud workflows list --location=us-central1
		```

---

## Key Differences
- **Pub/Sub:** Messaging, async communication, publish/subscribe pattern.
- **Eventarc:** Event routing, connects sources to consumers, CloudEvents.
- **Cloud Tasks:** Task queues, scheduled execution, rate limiting.
- **Workflows:** Orchestration, multi-step automation, service coordination.

---

## Summary Table

| Service    | Type           | Use Case                    | Key Differences              | Example gcloud Command              |
|------------|----------------|-----------------------------|------------------------------|-------------------------------------|
| Pub/Sub    | Messaging      | Async messaging, decoupling | Topics/subs, push/pull       | gcloud pubsub topics create         |
| Eventarc   | Event routing  | Event-driven architectures  | CloudEvents, 90+ sources     | gcloud eventarc triggers create     |
| Cloud Tasks| Task queue     | Background jobs, rate limit | HTTP/App Engine targets      | gcloud tasks queues create          |
| Workflows  | Orchestration  | Service coordination        | YAML workflows, serverless   | gcloud workflows deploy             |

---

**Tip:** Use Pub/Sub for real-time messaging, Eventarc for event routing, Cloud Tasks for scheduled background work, and Workflows for complex multi-step automation. These services work together to build resilient, decoupled architectures.

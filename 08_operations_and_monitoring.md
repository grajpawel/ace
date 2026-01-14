# 8. Operations & Monitoring (Detailed)

Google Cloud provides a comprehensive suite of operations and monitoring tools to ensure visibility, reliability, and performance of your applications and infrastructure. Below are the main services, their features, use cases, and relevant gcloud commands.

## Cloud Monitoring
Collects metrics from GCP resources, VMs, and custom sources. Provides dashboards, alerting, and integrations for incident response.

- **Features:**
	- Pre-built and custom dashboards for visualization.
	- Alerting policies for incidents and SRE practices.
	- Integrates with Cloud Logging, PagerDuty, Slack, and more.
- **Best Practices:**
	- Set up alerting for critical metrics (CPU, memory, latency).
	- Use Uptime Checks for external monitoring.
- **gcloud Examples:**
	- List monitoring policies:
		```sh
		gcloud monitoring policies list
		```
	- Create a basic alerting policy (example):
		```sh
		gcloud monitoring policies create --notification-channels=CHANNEL_ID --condition-display-name="High CPU" --condition-filter="metric.type=\"compute.googleapis.com/instance/cpu/utilization\""
		```

---

## Cloud Logging
Centralized log management for all GCP services. Supports log routing (sinks), log-based metrics, and alerting.

- **Features:**
	- Log sinks route logs to BigQuery, Pub/Sub, or Cloud Storage.
	- Log-based metrics for custom monitoring.
	- Integration with Cloud Monitoring for alerting.
- **Best Practices:**
	- Create log sinks for long-term retention or analysis.
	- Use log-based metrics for custom alerts.
- **gcloud Examples:**
	- List log sinks:
		```sh
		gcloud logging sinks list
		```
	- Create a log sink to BigQuery:
		```sh
		gcloud logging sinks create my-sink bigquery.googleapis.com/projects/PROJECT_ID/datasets/MY_DATASET
		```

---

## Ops Agent
Unified agent for monitoring and logging on Compute Engine VMs. Replaces older Stackdriver agents.

- **Features:**
	- Collects system and application metrics and logs.
	- Single agent for both monitoring and logging.
- **Best Practices:**
	- Use Ops Agent for all new VM deployments.
	- Keep agent up to date for new features and security.
- **gcloud Examples:**
	- Install Ops Agent on a VM:
		```sh
		gcloud compute ssh INSTANCE_NAME --command="curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh && sudo bash add-google-cloud-ops-agent-repo.sh --also-install"
		```
	- List installed agents (via metadata):
		```sh
		gcloud compute instances describe INSTANCE_NAME --format='get(metadata.items)'
		```

---

## Diagnostics Tools
Advanced tools for application and infrastructure diagnostics.

- **Cloud Trace:** Distributed tracing for latency analysis and performance bottlenecks.
- **Profiler:** CPU and memory profiling for optimizing applications.
- **Debugger:** Live debugging of running applications without stopping them.
- **Best Practices:**
	- Use Trace and Profiler in production for performance optimization.
	- Use Debugger for troubleshooting without downtime.
- **gcloud Examples:**
	- List traces:
		```sh
		gcloud trace list-traces
		```
	- List profilers:
		```sh
		gcloud alpha profiler deployments list --project=PROJECT_ID
		```

---

## Health Checks
Used by instance groups and load balancers to determine resource health. Ensures only healthy resources receive traffic.

- **Types:** HTTP(S), TCP, SSL, gRPC.
- **Best Practices:**
	- Use health checks for all managed instance groups and load balancers.
	- Tune health check parameters for your workload.
- **gcloud Examples:**
	- Create an HTTP health check:
		```sh
		gcloud compute health-checks create http my-hc --port=80
		```
	- List health checks:
		```sh
		gcloud compute health-checks list
		```

---

## Error Reporting
Automatically groups and displays errors from Cloud services and applications. Helps identify and track application errors.

- **Features:**
	- Automatic error grouping and deduplication.
	- Stack trace analysis.
	- Integration with Cloud Logging.
	- Email and mobile notifications.
	- Support for App Engine, Cloud Functions, Cloud Run, GKE, Compute Engine.
- **Best Practices:**
	- Use client libraries to report custom errors.
	- Set up notifications for new or recurring errors.
	- Link to issue tracking systems.
- **gcloud Examples:**
	- List error groups:
		```sh
		gcloud error-reporting events list
		```
	- Note: Most Error Reporting operations are performed via Console or API.

---

## Service Level Objectives (SLOs) & Monitoring
Define and monitor service reliability using SLIs (Service Level Indicators) and SLOs (Service Level Objectives).

- **Concepts:**
	- **SLI:** Quantitative measure of service level (e.g., latency, availability, error rate).
	- **SLO:** Target value or range for an SLI (e.g., 99.9% availability).
	- **SLA:** Contractual agreement with consequences for not meeting SLOs.
	- **Error Budget:** Amount of acceptable unreliability (100% - SLO).
- **Features:**
	- Define SLOs in Cloud Monitoring.
	- Track SLO compliance and error budget burn rate.
	- Alerting on SLO violations.
- **Best Practices:**
	- Define SLOs based on user experience.
	- Start with achievable SLOs and iterate.
	- Use error budgets for change management decisions.
	- Monitor SLO burn rate for early warnings.
- **gcloud Examples:**
	- Create an SLO (example using Console or API):
		- SLOs are typically configured via Cloud Console or Monitoring API.

---

## Uptime Checks
Monitor availability and response time of public endpoints (URLs, VMs, load balancers).

- **Features:**
	- HTTP(S), TCP, and custom protocol checks.
	- Checks from multiple global locations.
	- Alerting on failures or latency threshold violations.
	- Integration with Cloud Monitoring dashboards.
- **Best Practices:**
	- Set up uptime checks for all public-facing services.
	- Use checks from multiple regions for global monitoring.
	- Configure alerts for rapid incident response.
- **gcloud Examples:**
	- Create an uptime check:
		```sh
		gcloud monitoring uptime-checks create my-uptime-check \
			--display-name="My Website Check" \
			--resource-type=uptime-url \
			--host=example.com \
			--path=/
		```
	- List uptime checks:
		```sh
		gcloud monitoring uptime-checks list
		```

---

## Key Differences
- **Monitoring:** Metrics, dashboards, alerts for resource and app health.
- **Logging:** Log collection, routing, analysis, and alerting.
- **Ops Agent:** Unified agent for VMs, replaces old agents.
- **Diagnostics:** Deep analysis, debugging, tracing, profiling.
- **Health Checks:** Resource health, used by LBs and instance groups, not for metrics/logs.

---

## Summary Table

| Feature         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-------------------------|-------------------------------|-----------------------------------------------|
| Monitoring      | Metrics, alerting       | Dashboards, alerts             | gcloud monitoring policies list               |
| Logging         | Log management          | Centralized, sinks, metrics    | gcloud logging sinks create                   |
| Ops Agent       | VM monitoring/logging   | Unified, replaces old agents   | gcloud compute ssh (install agent)            |
| Diagnostics     | App analysis/debugging  | Trace, Profiler, Debugger      | gcloud trace list-traces                      |
| Health Checks   | Resource health         | Used by LBs, instance groups   | gcloud compute health-checks create           |

---

**Tip:** Set up monitoring, logging, and health checks for all production workloads. Use diagnostics tools for performance tuning and troubleshooting. Export logs and metrics for long-term analysis and compliance.
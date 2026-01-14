# 15. Advanced Security Services (Detailed)

Google Cloud provides comprehensive security services to protect data, applications, and infrastructure. Below are the key advanced security services, their features, use cases, and relevant gcloud commands.

## Secret Manager
Secure storage, versioning, and access control for secrets (API keys, passwords, certificates, tokens).

- **Features:**
	- Automatic encryption at rest and in transit.
	- Secret versioning and rotation.
	- IAM integration for access control.
	- Audit logging of secret access.
	- Integration with Cloud Functions, Cloud Run, GKE, Compute Engine.
- **Best Practices:**
	- Never hardcode secrets in code or config files.
	- Use IAM to grant least-privilege access to secrets.
	- Enable secret rotation for sensitive credentials.
	- Use secret versions for safe rollbacks.
- **gcloud Examples:**
	- Create a secret:
		```sh
		gcloud secrets create my-secret --replication-policy=automatic
		```
	- Add a secret version:
		```sh
		echo -n "my-secret-value" | gcloud secrets versions add my-secret --data-file=-
		```
	- Access a secret:
		```sh
		gcloud secrets versions access latest --secret=my-secret
		```
	- List secrets:
		```sh
		gcloud secrets list
		```
	- Grant access to a secret:
		```sh
		gcloud secrets add-iam-policy-binding my-secret \
			--member="serviceAccount:SA@PROJECT.iam.gserviceaccount.com" \
			--role="roles/secretmanager.secretAccessor"
		```

---

## Cloud KMS (Key Management Service)
Managed encryption key management for customer-managed encryption keys (CMEK). Create, rotate, and manage cryptographic keys.

- **Features:**
	- Symmetric and asymmetric encryption keys.
	- Automatic key rotation.
	- Integration with Cloud Storage, BigQuery, Compute Engine, etc.
	- Hardware Security Module (HSM) support via Cloud HSM.
	- IAM for key access control.
- **Best Practices:**
	- Use CMEK for sensitive data requiring customer-controlled encryption.
	- Enable automatic key rotation for active keys.
	- Use separate keys for different environments (dev, prod).
	- Audit key usage via Cloud Logging.
- **gcloud Examples:**
	- Create a key ring:
		```sh
		gcloud kms keyrings create my-keyring --location=us-central1
		```
	- Create a key:
		```sh
		gcloud kms keys create my-key \
			--keyring=my-keyring \
			--location=us-central1 \
			--purpose=encryption
		```
	- List keys:
		```sh
		gcloud kms keys list --keyring=my-keyring --location=us-central1
		```
	- Encrypt data:
		```sh
		echo "plaintext" | gcloud kms encrypt \
			--key=my-key \
			--keyring=my-keyring \
			--location=us-central1 \
			--plaintext-file=- \
			--ciphertext-file=encrypted.bin
		```

---

## VPC Service Controls
Security perimeters to protect GCP services and prevent data exfiltration. Control access to resources based on context (IP, identity, device).

- **Features:**
	- Create service perimeters around sensitive projects/resources.
	- Prevent unauthorized data access or copying.
	- Context-aware access policies (IP ranges, VPC networks, access levels).
	- Works with BigQuery, Cloud Storage, Bigtable, and more.
- **Best Practices:**
	- Use for highly regulated environments (HIPAA, PCI-DSS).
	- Define perimeters around sensitive data projects.
	- Use access levels for fine-grained control.
- **gcloud Examples:**
	- Create an access policy:
		```sh
		gcloud access-context-manager policies create --title="My Policy"
		```
	- Create a service perimeter:
		```sh
		gcloud access-context-manager perimeters create my-perimeter \
			--title="My Perimeter" \
			--resources=projects/PROJECT_NUMBER \
			--restricted-services=storage.googleapis.com,bigquery.googleapis.com \
			--policy=POLICY_ID
		```
	- List perimeters:
		```sh
		gcloud access-context-manager perimeters list --policy=POLICY_ID
		```

---

## Binary Authorization
Deploy-time security for container images. Enforce policies requiring trusted signatures before deployment to GKE or Cloud Run.

- **Features:**
	- Policy-based deployment controls.
	- Integration with Container Analysis for vulnerability scanning.
	- Attestation-based verification (signed by trusted authority).
	- Break-glass option for emergencies.
- **Best Practices:**
	- Require attestations from CI/CD pipelines.
	- Use with Artifact Registry vulnerability scanning.
	- Define policies per cluster or environment.
- **gcloud Examples:**
	- Create a Binary Authorization policy:
		```sh
		gcloud container binauthz policy import policy.yaml
		```
	- Export current policy:
		```sh
		gcloud container binauthz policy export > policy.yaml
		```

---

## Security Command Center
Centralized security and risk management platform. Discover assets, vulnerabilities, and security threats across GCP.

- **Features:**
	- Asset discovery and inventory.
	- Vulnerability scanning and reporting.
	- Threat detection and anomaly detection.
	- Security Health Analytics.
	- Integration with Cloud Armor, Web Security Scanner, Event Threat Detection.
- **Best Practices:**
	- Enable for all production projects and organizations.
	- Set up notifications for critical findings.
	- Regularly review and remediate findings.
- **Console-Based (Limited gcloud support):**
	- Most operations are performed via the Cloud Console.

---

## Organization Policy Service
Centralized constraints and guardrails for resource configurations across the organization. Enforce compliance and governance.

- **Features:**
	- Boolean constraints (allow/deny specific actions).
	- List constraints (allow/deny specific values).
	- Inheritance from organization to folders to projects.
	- Examples: Disable service account key creation, restrict VM locations, enforce domain restrictions.
- **Best Practices:**
	- Apply policies at the organization or folder level.
	- Use for compliance, security, and cost controls.
	- Test policies in dev before applying to prod.
- **gcloud Examples:**
	- Set an organization policy:
		```sh
		gcloud resource-manager org-policies set-policy policy.yaml --project=PROJECT_ID
		```
	- List constraints:
		```sh
		gcloud resource-manager org-policies list --project=PROJECT_ID
		```
	- Describe a policy:
		```sh
		gcloud resource-manager org-policies describe CONSTRAINT_NAME --project=PROJECT_ID
		```

---

## Web Security Scanner
Automated vulnerability scanner for App Engine, Compute Engine, and GKE web applications. Detects common vulnerabilities (XSS, SQL injection, etc.).

- **Features:**
	- Crawls and scans web applications.
	- Detects OWASP Top 10 vulnerabilities.
	- Integration with Security Command Center.
- **Best Practices:**
	- Run scans regularly (weekly or on major releases).
	- Remediate findings promptly.
- **Console-Based (Limited gcloud support):**
	- Configure and run scans via the Cloud Console.

---

## Key Differences
- **Secret Manager:** Store secrets, versioning, access control.
- **Cloud KMS:** Manage encryption keys, CMEK, rotation.
- **VPC Service Controls:** Data exfiltration prevention, perimeters.
- **Binary Authorization:** Container deployment policies, attestation.
- **Security Command Center:** Centralized security monitoring, threat detection.
- **Organization Policy:** Governance, compliance constraints.
- **Web Security Scanner:** Vulnerability scanning for web apps.

---

## Summary Table

| Service                  | Type                | Use Case                     | Key Differences           | Example gcloud Command                    |
|--------------------------|---------------------|------------------------------|---------------------------|-------------------------------------------|
| Secret Manager           | Secret storage      | API keys, passwords          | Versioning, IAM           | gcloud secrets create                     |
| Cloud KMS                | Key management      | Encryption keys, CMEK        | Rotation, HSM support     | gcloud kms keys create                    |
| VPC Service Controls     | Data perimeter      | Data exfiltration prevention | Context-aware, perimeters | gcloud access-context-manager perimeters  |
| Binary Authorization     | Container security  | Deploy-time policy           | Attestation, GKE/Run      | gcloud container binauthz policy          |
| Security Command Center  | Security monitoring | Threat detection, compliance | Centralized, findings     | N/A (Console-based)                       |
| Organization Policy      | Governance          | Compliance constraints       | Org-wide, inheritance     | gcloud resource-manager org-policies      |
| Web Security Scanner     | Vulnerability scan  | OWASP Top 10 detection       | Automated, web apps       | N/A (Console-based)                       |

---

**Tip:** Use Secret Manager for secrets, Cloud KMS for encryption keys, VPC Service Controls for data protection, and Security Command Center for monitoring. Apply Organization Policies for governance and Binary Authorization for container security.

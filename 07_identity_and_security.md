# 7. Identity & Security (Detailed)

Google Cloud provides robust identity and security controls to protect resources and data. Below are the main components, their features, use cases, and relevant gcloud commands.

## IAM (Identity and Access Management)
Controls who (principals) can do what (roles) on which resources. Enforces the principle of least privilege.

- **Principals:**
  - Users (Google accounts), groups, service accounts, Google Workspace domains.
- **Roles:**
  - **Primitive:** Basic (Owner, Editor, Viewer) – project-wide, broad permissions.
  - **Predefined:** Granular, service-specific (e.g., Storage Object Viewer).
  - **Custom:** User-defined, tailored permissions for specific needs.
- **Best Practices:**
  - Grant only the permissions required (least privilege).
  - Use predefined roles over primitive roles for better security.
  - Regularly audit IAM policies.
- **gcloud Examples:**
  - List IAM policy for a project:
    ```sh
    gcloud projects get-iam-policy PROJECT_ID
    ```
  - Add a role to a user:
    ```sh
    gcloud projects add-iam-policy-binding PROJECT_ID --member="user:USER_EMAIL" --role="roles/viewer"
    ```
  - Remove a role from a user:
    ```sh
    gcloud projects remove-iam-policy-binding PROJECT_ID --member="user:USER_EMAIL" --role="roles/viewer"
    ```

---

## Service Accounts
Special Google accounts for applications, VMs, or services to access resources securely.

- **Features:**
  - Can be assigned IAM roles for resource access.
  - Can use key files (JSON) or workload identity federation for authentication.
  - Used by Compute Engine, GKE, Cloud Functions, etc.
- **Best Practices:**
  - Use one service account per application or workload.
  - Rotate keys regularly and avoid downloading keys unless necessary.
  - Use Workload Identity Federation for secure, keyless auth in multi-cloud/hybrid.
- **gcloud Examples:**
  - Create a service account:
    ```sh
    gcloud iam service-accounts create my-sa --display-name="My Service Account"
    ```
  - List service accounts:
    ```sh
    gcloud iam service-accounts list
    ```
  - Grant a role to a service account:
    ```sh
    gcloud projects add-iam-policy-binding PROJECT_ID --member="serviceAccount:MY-SA@PROJECT_ID.iam.gserviceaccount.com" --role="roles/editor"
    ```

---

## Workload Identity (GKE)
Recommended way for GKE workloads to access Google Cloud services. Allows Kubernetes service accounts to act as Google service accounts.

- **Features:**
	- No service account keys needed (more secure).
	- Kubernetes pods authenticate as Google service accounts.
	- Automatic credential rotation.
	- Fine-grained IAM permissions per workload.
- **Best Practices:**
	- Always use Workload Identity for GKE clusters (instead of node service accounts).
	- Create dedicated Google service accounts per application.
	- Grant minimal permissions to each service account.
- **gcloud Examples:**
	- Enable Workload Identity on a new cluster:
		```sh
		gcloud container clusters create my-cluster \
			--workload-pool=PROJECT_ID.svc.id.goog \
			--region=us-central1
		```
	- Enable Workload Identity on existing cluster:
		```sh
		gcloud container clusters update my-cluster \
			--workload-pool=PROJECT_ID.svc.id.goog \
			--region=us-central1
		```
	- Bind Kubernetes SA to Google SA:
		```sh
		gcloud iam service-accounts add-iam-policy-binding GSA_NAME@PROJECT_ID.iam.gserviceaccount.com \
			--role=roles/iam.workloadIdentityUser \
			--member="serviceAccount:PROJECT_ID.svc.id.goog[NAMESPACE/KSA_NAME]"
		```

---

## Custom IAM Roles
Create tailored roles with specific permissions when predefined roles don't fit requirements.

- **Features:**
	- Define exact permissions needed.
	- Can be created at organization or project level.
	- Support for IAM conditions (time-based, resource-based access).
	- Must specify support level (ALPHA, BETA, GA).
- **Best Practices:**
	- Use predefined roles when possible (easier to maintain).
	- Create custom roles for specific organizational needs.
	- Regularly review and update custom role permissions.
	- Test custom roles in dev before deploying to prod.
- **gcloud Examples:**
	- Create a custom role:
		```sh
		gcloud iam roles create myCustomRole \
			--project=PROJECT_ID \
			--title="My Custom Role" \
			--description="Custom role for specific permissions" \
			--permissions=compute.instances.list,compute.instances.get \
			--stage=GA
		```
	- Update a custom role:
		```sh
		gcloud iam roles update myCustomRole \
			--project=PROJECT_ID \
			--add-permissions=compute.instances.start
		```
	- List custom roles:
		```sh
		gcloud iam roles list --project=PROJECT_ID --show-deleted
		```

---

## Audit Logs
Track admin activity, data access, system events, and policy denials for compliance, troubleshooting, and security monitoring.

- **Types:**
  - **Admin Activity:** All admin actions (always enabled, cannot be disabled).
  - **Data Access:** Reads/writes to user data (can be enabled/disabled).
  - **System Event:** GCP system actions.
  - **Policy Denied:** Denied access attempts due to policy.
- **Best Practices:**
	- Enable Data Access logs for sensitive resources.
	- Export logs to BigQuery or Cloud Storage for analysis and retention.
- **gcloud Examples:**
	- List audit log sinks:
		```sh
		gcloud logging sinks list
		```
	- Create a log sink to export to BigQuery:
		```sh
		gcloud logging sinks create my-sink bigquery.googleapis.com/projects/PROJECT_ID/datasets/MY_DATASET --log-filter='logName:"cloudaudit.googleapis.com"'
		```

### Access Transparency
Provides logs of Google Cloud support and engineering access to customer data.

- **Features:**
	- Records when Google personnel access your content.
	- Available for Google Cloud Premium Support customers.
	- Logs include justification, timestamp, and accessed resources.
	- Helps meet compliance requirements.
- **Best Practices:**
	- Review Access Transparency logs regularly.
	- Integrate with security monitoring systems.
- **Note:** Enterprise-only feature, requires Premium Support.

---

## IAM Conditions
Add conditional logic to IAM policies for fine-grained, attribute-based access control.

- **Features:**
	- Time-based access (grant access during business hours only).
	- Resource-based access (access specific resources only).
	- IP-based access (restrict to specific IP ranges).
	- Multiple conditions combined with AND/OR logic.
- **Common Use Cases:**
	- Temporary access for contractors (time-based).
	- Access to dev resources only during work hours.
	- Restrict access by geography/IP.
	- Access to specific resource patterns (e.g., buckets with specific prefix).
- **Best Practices:**
	- Use for temporary or conditional access requirements.
	- Test conditions thoroughly before production use.
	- Document condition logic for maintainability.
- **Example Condition Expressions:**
	- Time-based: `request.time < timestamp('2026-12-31T00:00:00Z')`
	- Resource-based: `resource.name.startsWith('projects/_/buckets/dev-')`
	- IP-based: `origin.ip in ['203.0.113.0/24']`
- **gcloud Examples:**
	- Add IAM binding with condition:
		```sh
		gcloud projects add-iam-policy-binding PROJECT_ID \
			--member="user:user@example.com" \
			--role="roles/storage.objectViewer" \
			--condition='expression=request.time < timestamp("2026-12-31T00:00:00Z"),title=Temporary Access,description=Access until end of 2026'
		```

---

## Cloud Identity
Identity as a Service (IDaaS) for managing users and groups. Foundation for Google Workspace and GCP IAM.

- **Features:**
	- User and group management.
	- Single Sign-On (SSO) integration.
	- Multi-factor authentication (MFA).
	- Device management.
	- Works with Google Workspace or standalone.
- **Free vs Premium:**
	- **Free:** Basic user/group management, essential security.
	- **Premium:** Advanced security, endpoint management, context-aware access.
- **Best Practices:**
	- Use Cloud Identity for centralized user management.
	- Integrate with existing identity providers (SAML, OIDC).
	- Enforce MFA for all users.
	- Use groups for IAM policy management (not individual users).
- **Integration with GCP:**
	- Cloud Identity users/groups can be granted IAM roles.
	- Supports organization-level policies.
	- Works with VPC Service Controls and Access Context Manager.

---

## Key Differences
- **IAM:** Who can access what, at what level (roles, policies).
- **Service Accounts:** Non-human identity for apps/VMs, used for automation and secure access.
- **Audit Logs:** Record of actions and access, not access control.

---

## Summary Table

| Feature         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-------------------------|-------------------------------|-----------------------------------------------|
| IAM             | Access control          | Roles, least privilege         | gcloud projects get-iam-policy                |
| Service Account | App/VM identity         | Non-human, key/federation      | gcloud iam service-accounts create            |
| Audit Logs      | Compliance, monitoring  | Records actions, not control   | gcloud logging sinks create                   |

---

**Tip:** Regularly audit IAM policies and service accounts. Enable and export audit logs for compliance and security monitoring. Use least privilege and avoid using primitive roles in production.
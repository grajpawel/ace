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

## Workforce Identity Federation (Important for Exam!)
Allows external users (human workforce) to access Google Cloud resources using external identity providers (IdPs) like Azure AD, Okta, or any OIDC/SAML provider. **No need for Google accounts or service account keys.**

- **What It Is:**
	- Identity federation for **human users** (employees, contractors)
	- Different from Workload Identity Federation (for applications/workloads)
	- Maps external identities to Google Cloud IAM
	- SSO (Single Sign-On) support
- **Key Concepts:**
	- **Workforce Identity Pool:** Container for external identities from your organization
	- **Workforce Identity Provider:** Connection to external IdP (Azure AD, Okta, ADFS, etc.)
	- **Attribute Mapping:** Maps external user attributes to Google Cloud principals
	- **Attribute Conditions:** Control access based on user attributes (groups, departments, etc.)
- **Supported Identity Providers:**
	- Azure Active Directory (Azure AD)
	- Okta
	- Active Directory Federation Services (ADFS)
	- Any OIDC-compatible provider
	- Any SAML 2.0-compatible provider
- **Common Exam Scenarios:**
	- **Question:** "Company uses Azure AD. How to grant Azure AD users access to GCP without creating Google accounts?"
	- **Answer:** Configure Workforce Identity Federation with Azure AD as the provider
	- **Question:** "Need to restrict GCP access to only users in 'Engineering' group from Okta"
	- **Answer:** Use attribute conditions in Workforce Identity Federation
- **Benefits:**
	- No Google Cloud Identity or Google Workspace accounts required
	- Centralized identity management (use existing IdP)
	- No service account keys to manage
	- Attribute-based access control (ABAC)
	- Audit trail through external IdP
- **Use Cases:**
	- Federate workforce from Azure AD to GCP
	- Grant contractors from Okta temporary GCP access
	- Multi-cloud scenarios (Azure users accessing GCP)
	- Organizations without Google Workspace
- **Best Practices:**
	- Use attribute conditions to restrict access by group/department
	- Map external user attributes (email, groups) to IAM principals
	- Enable session duration limits for security
	- Use with Cloud Identity for hybrid scenarios
	- Regularly review and audit federated access
- **How It Works:**
	1. User authenticates with external IdP (Azure AD, Okta)
	2. IdP returns token (OIDC or SAML assertion)
	3. User exchanges token for Google Cloud short-lived credentials
	4. User accesses GCP resources with mapped IAM permissions
- **gcloud Examples:**
	- Create a workforce identity pool:
		```sh
		gcloud iam workforce-pools create my-workforce-pool \
			--organization=ORG_ID \
			--location=global \
			--display-name="My Workforce Pool"
		```
	- Create a workforce identity provider (OIDC, e.g., Azure AD):
		```sh
		gcloud iam workforce-pools providers create-oidc my-azure-provider \
			--workforce-pool=my-workforce-pool \
			--location=global \
			--issuer-uri="https://login.microsoftonline.com/TENANT_ID/v2.0" \
			--client-id="AZURE_CLIENT_ID" \
			--attribute-mapping="google.subject=assertion.sub,google.groups=assertion.groups" \
			--attribute-condition="assertion.groups.contains('Engineering')"
		```
	- List workforce pools:
		```sh
		gcloud iam workforce-pools list --organization=ORG_ID --location=global
		```
	- Grant IAM role to federated users:
		```sh
		gcloud projects add-iam-policy-binding PROJECT_ID \
			--member="principalSet://iam.googleapis.com/locations/global/workforcePools/my-workforce-pool/attribute.email/user@example.com" \
			--role="roles/viewer"
		```

**Exam Tip - Key Differences:**
- **Workforce Identity Federation:** For **human users** (employees) from external IdPs (Azure AD, Okta)
- **Workload Identity Federation:** For **applications/workloads** from external clouds (AWS, Azure VMs)
- **Workload Identity (GKE):** For **Kubernetes pods** in GKE to access GCP services

---

## Workload Identity Federation (Critical Exam Topic!)
Allows applications running **outside GCP** (AWS, Azure, on-premises) to access Google Cloud resources **without service account keys**. This is the solution when you cannot create new service accounts due to Organization Policy constraints.

- **What It Is:**
	- Identity federation for **applications and workloads** (not human users)
	- Enables keyless authentication from external environments
	- Maps external workload identities to Google Cloud service accounts
	- Different from Workforce Identity Federation (for humans)
- **Key Concepts:**
	- **Workload Identity Pool:** Container for external workload identities
	- **Workload Identity Provider:** Connection to external identity source (AWS, Azure, OIDC)
	- **Attribute Mapping:** Maps external identity attributes to GCP principals
	- **Service Account Impersonation:** External workload impersonates GCP service account
- **Supported External Identity Providers:**
	- AWS (using AWS IAM roles)
	- Azure Active Directory (Azure AD for apps)
	- Any OIDC-compatible provider
	- On-premises identity systems with OIDC
- **Critical Exam Scenario - Service Account Creation Blocked:**
	- **Question:** "Organization Policy blocks creating new service accounts. How can external applications authenticate to GCP?"
	- **Answer:** Use **Workload Identity Federation** to map external identities to existing service accounts
	- **Key Point:** WIF doesn't require creating NEW service accounts - it uses impersonation of EXISTING service accounts
	- **Alternative:** If you need a new service account but creation is blocked, you can:
		1. Use WIF with an existing service account (impersonation)
		2. Request Organization Policy exception from admin
		3. Use Workforce Identity Federation for human access (if applicable)
- **Common Exam Scenarios:**
	- **Question:** "AWS EC2 instance needs to access GCS. Don't want to use service account keys."
	- **Answer:** Configure Workload Identity Federation with AWS as provider
	- **Question:** "Azure VM needs to authenticate to BigQuery without downloading JSON keys"
	- **Answer:** Use Workload Identity Federation with Azure AD
	- **Question:** "Organization Policy `iam.disableServiceAccountCreation` is enforced. How to grant access?"
	- **Answer:** Use Workload Identity Federation to impersonate existing service account
- **Benefits:**
	- **No service account keys** to manage, rotate, or secure
	- **No new service accounts needed** (uses impersonation)
	- Works with Organization Policies that restrict service account creation
	- Automatic credential rotation (short-lived tokens)
	- Audit trail through external identity provider
	- Centralized identity management
	- Improved security posture (no long-lived credentials)
- **Use Cases:**
	- AWS applications accessing GCP services
	- Azure VMs reading from Cloud Storage
	- On-premises apps using BigQuery
	- Multi-cloud CI/CD pipelines (GitHub Actions, GitLab CI)
	- When Organization Policy blocks service account creation
	- Third-party SaaS apps needing GCP access
- **How It Works:**
	1. External workload authenticates with its identity provider (AWS IAM, Azure AD)
	2. Identity provider issues a token (OIDC token, AWS STS token, etc.)
	3. Workload exchanges external token for Google Cloud access token
	4. Workload impersonates a Google Cloud service account
	5. Workload accesses GCP resources using service account permissions
- **Best Practices:**
	- Use attribute conditions to restrict which external identities can authenticate
	- Map external identities to dedicated service accounts (least privilege)
	- Enable audit logging for all federation events
	- Set token expiration to shortest necessary duration
	- Use with existing service accounts when creation is blocked
	- Regularly review and rotate workload identity pool configurations
- **gcloud Examples:**
	- Create a workload identity pool:
		```sh
		gcloud iam workload-identity-pools create my-pool \
			--project=PROJECT_ID \
			--location=global \
			--display-name="My Workload Pool"
		```
	- Create a workload identity provider for AWS:
		```sh
		gcloud iam workload-identity-pools providers create-aws my-aws-provider \
			--workload-identity-pool=my-pool \
			--location=global \
			--account-id=AWS_ACCOUNT_ID
		```
	- Create a workload identity provider for Azure:
		```sh
		gcloud iam workload-identity-pools providers create-oidc my-azure-provider \
			--workload-identity-pool=my-pool \
			--location=global \
			--issuer-uri="https://login.microsoftonline.com/TENANT_ID/v2.0" \
			--allowed-audiences="api://YOUR_APP_ID" \
			--attribute-mapping="google.subject=assertion.sub"
		```
	- Create a workload identity provider for generic OIDC:
		```sh
		gcloud iam workload-identity-pools providers create-oidc my-oidc-provider \
			--workload-identity-pool=my-pool \
			--location=global \
			--issuer-uri="https://token.actions.githubusercontent.com" \
			--allowed-audiences="https://github.com/my-org" \
			--attribute-mapping="google.subject=assertion.sub,attribute.repository=assertion.repository"
		```
	- Grant service account impersonation to external identity:
		```sh
		gcloud iam service-accounts add-iam-policy-binding MY_SA@PROJECT_ID.iam.gserviceaccount.com \
			--role=roles/iam.workloadIdentityUser \
			--member="principalSet://iam.googleapis.com/projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/my-pool/attribute.aws_role/arn:aws:sts::AWS_ACCOUNT:assumed-role/ROLE_NAME"
		```
	- List workload identity pools:
		```sh
		gcloud iam workload-identity-pools list --location=global --project=PROJECT_ID
		```

**Exam Scenario - Organization Policy Constraints:**

**Scenario:** Your organization has enforced `constraints/iam.disableServiceAccountCreation` policy. You need to grant an AWS Lambda function access to Cloud Storage.

**Solution:**
1. ✅ Use **Workload Identity Federation** with AWS as the provider
2. ✅ Configure the Lambda to impersonate an **existing** service account (no new SA needed)
3. ✅ Grant the existing service account `roles/storage.objectViewer` on the bucket
4. ✅ AWS Lambda authenticates using AWS IAM, exchanges for GCP token, impersonates service account

**NOT the solution:**
- ❌ Create a new service account with JSON key (blocked by policy)
- ❌ Use user credentials (not for application authentication)
- ❌ Store service account key in Lambda environment variables (insecure, blocked anyway)

**Complete Federation Comparison Table:**

| Feature | Workforce Identity Federation | Workload Identity Federation | Workload Identity (GKE) |
|---------|-------------------------------|------------------------------|-------------------------|
| **For** | Human users (employees) | Applications/workloads | Kubernetes pods in GKE |
| **Source** | Azure AD, Okta, SAML/OIDC IdP | AWS, Azure, OIDC providers | GKE Kubernetes SA |
| **Use Case** | Employee access to GCP Console/APIs | External apps accessing GCP | GKE pods accessing GCP services |
| **Creates New SA?** | No (maps to existing principals) | No (impersonates existing SA) | No (maps K8s SA to GSA) |
| **When SA Creation Blocked** | ✅ Works (no SA needed) | ✅ Works (impersonates existing) | ✅ Works (uses existing SA) |
| **Auth Type** | SSO for humans | API/programmatic | Pod service account |
| **Example** | Azure AD user → GCP Console | AWS Lambda → Cloud Storage | GKE pod → BigQuery |
| **Command Prefix** | `gcloud iam workforce-pools` | `gcloud iam workload-identity-pools` | `gcloud container clusters` |

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

## OS Login
Manages SSH access to Compute Engine VMs using IAM instead of managing SSH keys manually. Simplifies access control and improves security.

- **Features:**
	- Links Linux user accounts to Google identities (IAM).
	- Automatically manages SSH keys (no manual key distribution).
	- Integrates with IAM roles for access control.
	- Supports two-factor authentication (2FA).
	- Provides audit logs for SSH access.
- **Key IAM Roles:**
	- `roles/compute.osLogin` - Standard user access (non-admin).
	- `roles/compute.osAdminLogin` - Admin (sudo) access.
- **Metadata Keys:**
	- `enable-oslogin=TRUE` - Enable OS Login at project or instance level.
	- `enable-oslogin-2fa=TRUE` - Require 2FA for SSH access.
- **Best Practices:**
	- Enable OS Login organization-wide for centralized access management.
	- Use `roles/compute.osAdminLogin` sparingly (grant only when sudo needed).
	- Enable 2FA for privileged accounts.
	- Use OS Login instead of managing SSH keys manually (metadata-based keys).
- **gcloud Examples:**
	- Enable OS Login at project level:
		```sh
		gcloud compute project-info add-metadata \
			--metadata enable-oslogin=TRUE
		```
	- Enable OS Login with 2FA:
		```sh
		gcloud compute project-info add-metadata \
			--metadata enable-oslogin=TRUE,enable-oslogin-2fa=TRUE
		```
	- Grant OS Login access to a user:
		```sh
		gcloud projects add-iam-policy-binding PROJECT_ID \
			--member=user:alice@example.com \
			--role=roles/compute.osLogin
		```
	- Grant OS Admin Login (sudo) access:
		```sh
		gcloud projects add-iam-policy-binding PROJECT_ID \
			--member=user:alice@example.com \
			--role=roles/compute.osAdminLogin
		```
	- SSH to a VM with OS Login:
		```sh
		gcloud compute ssh VM_NAME --zone=ZONE
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
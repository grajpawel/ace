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
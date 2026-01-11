# 6. Load Balancing & Traffic Management (Detailed)

Google Cloud offers a range of load balancing and traffic management solutions to ensure high availability, scalability, and security for your applications. Below are the main services, their features, use cases, and relevant gcloud commands.

## Load Balancers
Distribute incoming traffic across multiple resources to maximize uptime and performance. Choose the right load balancer based on protocol, scope, and features.

- **HTTP(S) Load Balancer:**
  - Global, Layer 7 (application), supports SSL offload, content-based routing, autoscaling, Cloud CDN integration.
  - Use for web apps, APIs, and global services.
- **SSL Proxy Load Balancer:**
  - Global, Layer 4, for encrypted non-HTTP traffic (e.g., SMTPS, FTPS).
- **TCP Proxy Load Balancer:**
  - Global, Layer 4, for non-HTTP TCP traffic.
- **Network Load Balancer:**
  - Regional, Layer 4, for TCP/UDP, ultra-low latency, preserves client IP.
- **Best Practices:**
  - Use global HTTP(S) LB for multi-region, scalable web apps.
  - Use network LB for latency-sensitive, regional workloads.
  - Integrate with Cloud Armor for security.
- **gcloud Examples:**
  - Create a global HTTP(S) load balancer (basic example):
    ```sh
    gcloud compute forwarding-rules create my-http-rule \
      --global \
      --target-http-proxy=my-proxy \
      --ports=80
    ```
  - List load balancers:
    ```sh
    gcloud compute forwarding-rules list
    ```

---

## Network Service Tiers
Choose between Premium and Standard tiers to optimize for performance or cost.

- **Premium Tier:**
  - Uses Google’s global network, low latency, high reliability.
  - Recommended for production and latency-sensitive workloads.
- **Standard Tier:**
  - Uses public internet, lower cost, regional routing.
  - Suitable for dev/test or cost-sensitive workloads.
- **gcloud Example:**
  - Set network tier for a forwarding rule:
    ```sh
    gcloud compute forwarding-rules create my-rule --network-tier=PREMIUM ...
    ```

---

## Cloud CDN
Content Delivery Network that caches HTTP(S) content at Google edge locations, reducing latency and improving performance for global users.

- **Features:**
  - Integrated with HTTP(S) Load Balancer.
  - Caches static and dynamic content.
  - Reduces origin server load.
- **Best Practices:**
  - Use for static assets, APIs, and global web apps.
  - Set appropriate cache-control headers.
- **gcloud Examples:**
  - Enable Cloud CDN on a backend service:
    ```sh
    gcloud compute backend-services update my-backend --enable-cdn --global
    ```
  - List backend services:
    ```sh
    gcloud compute backend-services list
    ```

---

## Cloud Armor
DDoS protection and Web Application Firewall (WAF) for Google Cloud services. Protects against attacks and enforces security policies at the edge.

- **Features:**
  - Predefined and custom security rules.
  - Protects HTTP(S) Load Balancers.
  - Rate limiting, IP allow/deny lists, geo-blocking.
- **Best Practices:**
  - Apply Cloud Armor policies to all public-facing services.
  - Regularly review and update security rules.
- **gcloud Examples:**
  - Create a security policy:
    ```sh
    gcloud compute security-policies create my-policy
    ```
  - Add a rule to deny traffic from a specific IP:
    ```sh
    gcloud compute security-policies rules create 1000 --security-policy=my-policy --expression="origin.ip == '1.2.3.4'" --action=deny-403
    ```
  - List security policies:
    ```sh
    gcloud compute security-policies list
    ```

---

## Key Differences
- **HTTP(S) LB:** Layer 7, global, advanced routing, SSL offload.
- **Network LB:** Layer 4, regional, simple TCP/UDP, preserves client IP.
- **Premium vs Standard:** Network quality (global) vs cost (regional).
- **Cloud CDN:** Caching, not a load balancer, HTTP(S) only.
- **Cloud Armor:** Security, not traffic distribution, protects L7 services.

---

## Summary Table

| Feature         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-------------------------|-------------------------------|-----------------------------------------------|
| HTTP(S) LB      | Web apps, APIs          | Layer 7, global, SSL, routing  | gcloud compute forwarding-rules create        |
| Network LB      | TCP/UDP, low latency    | Layer 4, regional             | gcloud compute forwarding-rules create        |
| Service Tiers   | Network quality/cost    | Premium (global) vs Standard   | gcloud compute forwarding-rules create        |
| Cloud CDN       | Content caching         | Edge locations, HTTP(S) only   | gcloud compute backend-services update        |
| Cloud Armor     | Security, DDoS/WAF      | Not a load balancer           | gcloud compute security-policies create       |

---

**Tip:** Use global HTTP(S) Load Balancer with Cloud CDN and Cloud Armor for secure, scalable, and high-performance web applications. Choose the right network tier for your workload and budget.
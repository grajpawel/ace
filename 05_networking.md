# 5. Networking (Detailed)

Google Cloud networking provides secure, scalable, and flexible connectivity for your resources. Below are the main networking components, their features, use cases, and relevant gcloud commands.

## VPC (Virtual Private Cloud)
A global, private, software-defined network for GCP resources. VPCs provide isolation, segmentation, and control over IP addressing, routing, and firewall rules.

- **Default VPC:**
	- Pre-created in new projects, auto mode subnets in each region, open firewall rules (allow SSH, RDP, ICMP).
	- Good for quick start, but less secure by default.
- **Custom VPC:**
	- User-defined, custom subnets, no default firewall rules.
	- Recommended for production, tighter security and control.
- **Best Practices:**
	- Use custom VPCs for production.
	- Restrict firewall rules to least privilege.
- **gcloud Examples:**
	- List VPC networks:
		```sh
		gcloud compute networks list
		```
	- Create a custom VPC:
		```sh
		gcloud compute networks create my-vpc --subnet-mode=custom
		```

---

## Subnets
Subnets are regional segments of a VPC. Each subnet defines a range of IP addresses and can have primary and secondary ranges (for GKE pods/services).

- **Features:**
	- Regional, not global.
	- Support for primary and secondary IP ranges.
- **Best Practices:**
	- Use secondary ranges for GKE clusters.
	- Plan IP ranges to avoid overlap.
- **gcloud Examples:**
	- Create a subnet:
		```sh
		gcloud compute networks subnets create my-subnet --network=my-vpc --region=us-central1 --range=10.0.0.0/24
		```
	- List subnets:
		```sh
		gcloud compute networks subnets list
		```

---

## IP Addresses
GCP resources can have internal (private) or external (public) IP addresses.

- **Internal IPs:** Used for communication within the VPC.
- **External IPs:** Used for internet access or public-facing services.
- **Best Practices:**
	- Use internal IPs whenever possible for security.
	- Reserve static external IPs for production services.
- **gcloud Examples:**
	- List external IPs:
		```sh
		gcloud compute addresses list
		```
	- Reserve a static external IP:
		```sh
		gcloud compute addresses create my-ip --region=us-central1
		```

---

## Shared VPC
Allows multiple projects (service projects) to share a common VPC network from a host project. Centralizes network management and security.

- **Features:**
	- Host project owns the VPC; service projects attach resources.
	- IAM controls which projects can use the shared VPC.
- **Best Practices:**
	- Use for large organizations with multiple teams/projects.
	- Centralize network and security policy management.
- **gcloud Examples:**
	- Enable shared VPC on a host project:
		```sh
		gcloud compute shared-vpc enable HOST_PROJECT_ID
		```
	- Attach a service project:
		```sh
		gcloud compute shared-vpc associated-projects add SERVICE_PROJECT_ID --host-project=HOST_PROJECT_ID
		```

---

## VPC Network Peering
Private connectivity between VPCs (within or across organizations). Allows resources to communicate privately without public IPs.

- **Features:**
	- No transitive routing (A-B, B-C does not mean A-C).
	- Each side must create a peering connection.
- **Best Practices:**
	- Use for secure, private connectivity between projects or orgs.
	- Avoid overlapping IP ranges.
- **gcloud Examples:**
	- Create a peering connection:
		```sh
		gcloud compute networks peerings create my-peering --network=NETWORK1 --peer-network=NETWORK2 --auto-create-routes
		```
	- List peerings:
		```sh
		gcloud compute networks peerings list
		```

---

## Routes & Firewall Rules
Routes control traffic flow within a VPC. Firewall rules are stateful and control ingress/egress at the network level.

- **Best Practices:**
	- Use custom routes for advanced traffic engineering.
	- Restrict firewall rules to only required ports and sources.
- **gcloud Examples:**
	- Create a firewall rule:
		```sh
		gcloud compute firewall-rules create allow-ssh --network=my-vpc --allow=tcp:22
		```
	- List firewall rules:
		```sh
		gcloud compute firewall-rules list
		```

---

## Cloud VPN
Provides encrypted tunnels from on-premises or other clouds to GCP VPCs.

- **Classic VPN:** Single tunnel, less HA.
- **HA VPN:** Multiple tunnels, high availability, recommended for production.
- **Best Practices:**
	- Use HA VPN for critical workloads.
	- Monitor VPN status and logs.
- **gcloud Examples:**
	- Create an HA VPN gateway:
		```sh
		gcloud compute vpn-gateways create my-ha-vpn --network=my-vpc --region=us-central1
		```
	- List VPN tunnels:
		```sh
		gcloud compute vpn-tunnels list
		```

---

## Cloud Interconnect
Provides dedicated or partner connections for high bandwidth, low latency connectivity between on-premises and GCP.

- **Dedicated Interconnect:** Direct fiber connection to Google.
- **Partner Interconnect:** Through a service provider.
- **Best Practices:**
	- Use for hybrid cloud or large data transfers.
	- Use VLAN attachments for segmentation.
- **gcloud Examples:**
	- List interconnects:
		```sh
		gcloud compute interconnects list
		```

---

## Cloud Router
Provides dynamic BGP routing for VPN and Interconnect connections.

- **Best Practices:**
	- Use for dynamic, scalable routing.
- **gcloud Examples:**
	- Create a Cloud Router:
		```sh
		gcloud compute routers create my-router --network=my-vpc --region=us-central1
		```
	- List routers:
		```sh
		gcloud compute routers list
		```

---

## Cloud NAT
Provides outbound internet access for private resources (no public IPs) in a VPC.

- **Best Practices:**
	- Use for private GKE nodes or VMs needing outbound access.
- **gcloud Examples:**
	- Create a Cloud NAT gateway:
		```sh
		gcloud compute routers nats create my-nat --router=my-router --region=us-central1 --auto-allocate-nat-external-ips --nat-all-subnet-ip-ranges
		```
	- List NAT gateways:
		```sh
		gcloud compute routers nats list --router=my-router --region=us-central1
		```

---

## Cloud DNS
Managed DNS service for public and private DNS zones.

- **Best Practices:**
	- Use private zones for internal DNS resolution.
	- Use DNSSEC for security.
- **gcloud Examples:**
	- Create a DNS managed zone:
		```sh
		gcloud dns managed-zones create my-zone --dns-name="example.com." --description="My zone"
		```
	- List managed zones:
		```sh
		gcloud dns managed-zones list
		```

---

## Key Differences
- **Default vs Custom VPC:** Prebuilt vs user-defined, open vs secure.
- **Shared VPC:** Centralized network for multiple projects.
- **Peering:** Private, no transitive routing.
- **VPN vs Interconnect:** Encrypted tunnels vs dedicated lines.
- **Cloud NAT:** Outbound only, no inbound.

---

## Summary Table

| Feature         | Use Case                | Key Differences                | Example gcloud Command                        |
|-----------------|-------------------------|-------------------------------|-----------------------------------------------|
| VPC             | Private network         | Global, default/custom         | gcloud compute networks create                |
| Subnet          | IP allocation           | Regional, primary/secondary    | gcloud compute networks subnets create        |
| Shared VPC      | Multi-project network   | Centralized, host/service proj | gcloud compute shared-vpc enable              |
| Peering         | Private VPC connect     | No transitive routing          | gcloud compute networks peerings create       |
| VPN             | Hybrid connectivity     | Encrypted, classic/HA          | gcloud compute vpn-gateways create            |
| Interconnect    | High bandwidth hybrid   | Dedicated/partner, VLANs       | gcloud compute interconnects list             |
| Cloud Router    | Dynamic routing         | BGP, VPN/Interconnect          | gcloud compute routers create                 |
| Cloud NAT       | Outbound for private    | No inbound, managed            | gcloud compute routers nats create            |
| Cloud DNS       | DNS management          | Public/private, DNSSEC         | gcloud dns managed-zones create               |

---

**Tip:** Design your network for security and scalability. Use custom VPCs, restrict firewall rules, and leverage shared VPC and peering for multi-project environments.
| Peering         | VPC-VPC connectivity    | No transitive routing          |
| VPN             | On-prem connectivity    | Classic vs HA, encrypted       |
| Interconnect    | High bandwidth, hybrid  | Dedicated/partner, not VPN     |
| Cloud NAT       | Outbound internet       | No public IPs, private only    |
| Cloud DNS       | DNS management          | Public/private zones           |
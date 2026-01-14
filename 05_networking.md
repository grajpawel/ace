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

- **Firewall Rules:**
	- Stateful (return traffic allowed automatically).
	- Apply to all instances or specific targets (tags, service accounts).
	- Priority-based (0-65535, lower number = higher priority).
	- Default deny for ingress, allow for egress.
- **Best Practices:**
	- Use custom routes for advanced traffic engineering.
	- Restrict firewall rules to only required ports and sources.
	- Use service accounts or tags for targeting specific instances.
	- Regularly review and remove unused rules (use Firewall Insights).
- **gcloud Examples:**
	- Create a firewall rule:
		```sh
		gcloud compute firewall-rules create allow-ssh --network=my-vpc --allow=tcp:22 --source-ranges=0.0.0.0/0
		```
	- List firewall rules:
		```sh
		gcloud compute firewall-rules list
		```
	- Create a firewall rule with target tags:
		```sh
		gcloud compute firewall-rules create allow-http --network=my-vpc --allow=tcp:80 --target-tags=web-server
		```

---

## VPC Flow Logs
Capture network traffic metadata for monitoring, forensics, and security analysis.

- **Features:**
	- Sample network flows (configurable sampling rate).
	- Logs source/dest IPs, ports, protocols, bytes, packets.
	- Integrated with Cloud Logging and BigQuery.
	- Minimal performance impact.
- **Best Practices:**
	- Enable on subnets requiring traffic analysis.
	- Adjust sampling rate to balance detail and cost.
	- Export to BigQuery for analysis and long-term retention.
	- Use for security monitoring, troubleshooting, and compliance.
- **gcloud Examples:**
	- Enable VPC Flow Logs on a subnet:
		```sh
		gcloud compute networks subnets update my-subnet \
			--region=us-central1 \
			--enable-flow-logs \
			--logging-aggregation-interval=interval-5-sec \
			--logging-flow-sampling=0.5
		```
	- Disable VPC Flow Logs:
		```sh
		gcloud compute networks subnets update my-subnet --region=us-central1 --no-enable-flow-logs
		```

---

## Packet Mirroring
Clone network traffic from specific instances for security analysis and monitoring.

- **Features:**
	- Mirror traffic to collector instances for inspection.
	- Filter by subnet, instance, or network tags.
	- Supports IDS/IPS, forensics, and compliance tools.
- **Best Practices:**
	- Use for security monitoring and threat detection.
	- Send mirrored traffic to security appliances or analysis tools.
	- Be mindful of collector instance capacity.
- **gcloud Examples:**
	- Create a packet mirroring policy:
		```sh
		gcloud compute packet-mirrorings create my-mirror \
			--region=us-central1 \
			--network=my-vpc \
			--collector-ilb=FORWARDING_RULE \
			--mirrored-subnets=my-subnet
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

## Private Google Access & Private Service Access
Enable private connectivity to Google services without using external IP addresses.

### Private Google Access
Allows VMs with only internal IPs to reach Google APIs and services.

- **Features:**
	- Access Cloud Storage, BigQuery, and other Google services from private VMs.
	- Enabled per subnet.
	- No internet gateway or NAT required for Google services.
- **Best Practices:**
	- Enable for subnets with private-only VMs.
	- Use with Cloud NAT for non-Google internet access.
- **gcloud Examples:**
	- Enable Private Google Access on a subnet:
		```sh
		gcloud compute networks subnets update my-subnet --region=us-central1 --enable-private-ip-google-access
		```
	- Disable Private Google Access:
		```sh
		gcloud compute networks subnets update my-subnet --region=us-central1 --no-enable-private-ip-google-access
		```

### Private Service Access
Provides private connectivity to managed services (Cloud SQL, Memorystore) via VPC peering.

- **Features:**
	- Services get internal IPs in your VPC.
	- No public internet exposure.
	- Uses VPC peering with Google-managed VPC.
- **Best Practices:**
	- Use for secure database and cache connections.
	- Reserve IP ranges for service connections.
- **gcloud Examples:**
	- Allocate IP range for private service connection:
		```sh
		gcloud compute addresses create my-service-range \
			--global \
			--purpose=VPC_PEERING \
			--prefix-length=16 \
			--network=my-vpc
		```
	- Create a private service connection:
		```sh
		gcloud services vpc-peerings connect \
			--service=servicenetworking.googleapis.com \
			--ranges=my-service-range \
			--network=my-vpc
		```

---

## Private Service Connect
Provides private access to Google-managed and third-party services via internal endpoints in your VPC.

- **Features:**
	- Access services via internal IPs in your VPC.
	- No VPC peering required (unlike Private Service Access).
	- Supports Google APIs, third-party services, and your own services.
	- Traffic stays within Google's network.
- **Best Practices:**
	- Use for accessing Google APIs from on-premises via Cloud VPN/Interconnect.
	- Use for consuming partner services privately.
- **gcloud Examples:**
	- Create a Private Service Connect endpoint:
		```sh
		gcloud compute forwarding-rules create my-psc-endpoint \
			--region=us-central1 \
			--network=my-vpc \
			--address=10.0.0.5 \
			--target-service-attachment=projects/PROJECT/regions/REGION/serviceAttachments/SERVICE
		```

---

## Network Intelligence Center
Suite of network monitoring and troubleshooting tools.

- **Tools:**
	- **Connectivity Tests:** Validate network paths between endpoints.
	- **Network Topology:** Visualize VPC network architecture.
	- **Performance Dashboard:** Monitor latency and packet loss.
	- **Firewall Insights:** Identify shadowed, unused, or overly permissive rules.
- **Best Practices:**
	- Use Connectivity Tests for troubleshooting network issues.
	- Review Firewall Insights regularly to optimize security.
	- Monitor Performance Dashboard for latency issues.
- **gcloud Examples:**
	- Create a connectivity test:
		```sh
		gcloud network-management connectivity-tests create my-test \
			--source-instance=projects/PROJECT/zones/ZONE/instances/VM1 \
			--destination-ip-address=10.0.0.5 \
			--protocol=TCP \
			--destination-port=80
		```
	- Run a connectivity test:
		```sh
		gcloud network-management connectivity-tests run my-test
		```

---

## Cloud DNS
Managed DNS service for public and private DNS zones. Provides authoritative DNS resolution with low latency and high availability.

- **Zone Types:**
	- **Public Zones:** Serve DNS records to the internet.
	- **Private Zones:** Serve DNS records to VPC networks only (internal resolution).
- **DNS Record Types (Important for Exam):**
	- **A Record:** Maps hostname to IPv4 address (e.g., `example.com` → `203.0.113.1`)
	- **AAAA Record:** Maps hostname to IPv6 address
	- **CNAME Record:** Canonical name, maps alias to another domain name (e.g., `www.example.com` → `example.com`)
		- Cannot be used at zone apex (root domain)
		- Points to another domain, not an IP
	- **NS Record:** Name Server, delegates subdomain to another DNS server
		- Specifies authoritative name servers for a zone
		- Example: `subdomain.example.com` delegated to `ns1.google.com`
	- **MX Record:** Mail Exchange, directs email to mail servers (has priority value)
	- **TXT Record:** Text record for verification, SPF, DKIM, etc.
	- **PTR Record:** Pointer for reverse DNS lookup (IP to hostname)
	- **SOA Record:** Start of Authority, contains zone metadata (created automatically)
- **Features:**
	- DNSSEC for security (protects against DNS spoofing)
	- 100% uptime SLA
	- Anycast name servers for low latency
	- Private DNS zones for split-horizon DNS
	- Cloud DNS peering and DNS forwarding
- **Common Exam Scenarios:**
	- **Use A record** when mapping domain to static IP
	- **Use CNAME** when creating alias (www, blog, shop)
	- **Use NS record** when delegating subdomain to different name servers
	- **Cannot use CNAME at zone apex** (must use A or AAAA)
- **Best Practices:**
	- Use private zones for internal DNS resolution.
	- Use DNSSEC for public zones.
	- Use Cloud DNS peering for cross-VPC DNS resolution.
	- Set appropriate TTL values (lower for frequently changing records).
- **gcloud Examples:**
	- Create a public DNS managed zone:
		```sh
		gcloud dns managed-zones create my-zone \
			--dns-name="example.com." \
			--description="My public zone"
		```
	- Create a private DNS zone:
		```sh
		gcloud dns managed-zones create my-private-zone \
			--dns-name="internal.example.com." \
			--description="My private zone" \
			--visibility=private \
			--networks=my-vpc
		```
	- Add an A record:
		```sh
		gcloud dns record-sets create www.example.com. \
			--zone=my-zone \
			--type=A \
			--ttl=300 \
			--rrdatas=203.0.113.1
		```
	- Add a CNAME record:
		```sh
		gcloud dns record-sets create blog.example.com. \
			--zone=my-zone \
			--type=CNAME \
			--ttl=300 \
			--rrdatas=www.example.com.
		```
	- Add an MX record:
		```sh
		gcloud dns record-sets create example.com. \
			--zone=my-zone \
			--type=MX \
			--ttl=300 \
			--rrdatas="10 mail.example.com."
		```
	- List DNS records:
		```sh
		gcloud dns record-sets list --zone=my-zone
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
| Private Google Access | Google services access | Subnet-level, no external IP | gcloud compute networks subnets update      |
| Private Service Connect | Private endpoints    | Internal IPs, no peering      | gcloud compute forwarding-rules create        |
| Network Intelligence | Monitoring/troubleshooting | Tests, topology, insights  | gcloud network-management connectivity-tests  |
| Cloud DNS       | DNS management          | Public/private, DNSSEC         | gcloud dns managed-zones create               |

---

**Tip:** Design your network for security and scalability. Use custom VPCs, restrict firewall rules, and leverage shared VPC and peering for multi-project environments. Enable Private Google Access for private VMs and use Network Intelligence Center for troubleshooting.
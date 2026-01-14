# 19. IP Addressing & Subnetting (CIDR Notation Explained)

Understanding IP addresses, CIDR notation, and subnet masks is crucial for the ACE exam, especially for VPC and networking questions.

## CIDR Notation Basics

### What is CIDR?
**CIDR (Classless Inter-Domain Routing)** is a method for allocating IP addresses and routing. It replaces the old "classful" network design.

### CIDR Format: `IP_ADDRESS/PREFIX_LENGTH`
- **IP Address:** The network address (e.g., 10.0.0.0)
- **/PREFIX_LENGTH:** Number of bits used for the network portion (e.g., /8, /16, /24)

**Example:** `10.0.0.0/8`
- Network: 10.0.0.0
- Prefix length: /8 (first 8 bits are the network, remaining 24 bits are for hosts)

---

## Understanding the /PREFIX Number

### What Does /8, /16, /24 Mean?

An IPv4 address has **32 bits** total. The /PREFIX tells you how many bits are used for the network portion.

| CIDR | Network Bits | Host Bits | Subnet Mask     | Number of IPs        | Usable IPs*        |
|------|--------------|-----------|-----------------|----------------------|--------------------|
| /8   | 8            | 24        | 255.0.0.0       | 16,777,216          | 16,777,216         |
| /12  | 12           | 20        | 255.240.0.0     | 1,048,576           | 1,048,576          |
| /16  | 16           | 16        | 255.255.0.0     | 65,536              | 65,536             |
| /20  | 20           | 12        | 255.255.240.0   | 4,096               | 4,096              |
| /24  | 24           | 8         | 255.255.255.0   | 256                 | 256                |
| /28  | 28           | 4         | 255.255.255.240 | 16                  | 16                 |
| /29  | 29           | 3         | 255.255.255.248 | 8                   | 8                  |
| /32  | 32           | 0         | 255.255.255.255 | 1                   | 1                  |

*Note: In traditional networking, you lose 2 IPs (network address and broadcast). In GCP, you lose 4 IPs per subnet (network, default gateway, second-to-last for future use, broadcast). However, for VPC sizing, Google doesn't enforce the traditional -2 rule as strictly.

### Formula to Calculate IP Count:
**Number of IPs = 2^(32 - PREFIX)**

Examples:
- /8: 2^(32-8) = 2^24 = 16,777,216 IPs
- /12: 2^(32-12) = 2^20 = 1,048,576 IPs
- /16: 2^(32-16) = 2^16 = 65,536 IPs
- /24: 2^(32-24) = 2^8 = 256 IPs

**Key Rule:** 
- **Lower prefix = Larger network** (more IPs)
- **Higher prefix = Smaller network** (fewer IPs)

---

## RFC 1918 Private IP Ranges

These are the standard private IP ranges (not routable on the public internet):

| Range              | CIDR Notation    | Total IPs    | Common Use                  |
|--------------------|------------------|--------------|-----------------------------|
| 10.0.0.0 - 10.255.255.255 | 10.0.0.0/8 | 16,777,216   | Large enterprises, GCP VPCs |
| 172.16.0.0 - 172.31.255.255 | 172.16.0.0/12 | 1,048,576 | Medium networks            |
| 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16 | 65,536 | Small networks, home routers|

**For GCP VPCs:** You can use any RFC 1918 range or even public IP ranges (though not recommended for private networks).

---

## Exam Question Breakdown

### Question:
> You need to create a custom VPC with a single subnet. The subnet's range must be **as large as possible**. Which range should you use?

**Options:**
- A. `0.0.0.0/0`
- B. `10.0.0.0/8` ✅ **CORRECT**
- C. `172.16.0.0/12`
- D. `192.168.0.0/16`

### Analysis:

#### Option A: `0.0.0.0/0`
- This represents ALL possible IP addresses (the entire internet).
- **Not valid for a VPC subnet** - too broad and includes public IPs.
- Used for default routes (route to internet), not subnet ranges.

#### Option B: `10.0.0.0/8` ✅
- **Largest private IP range:** 16,777,216 IPs
- /8 means only the first 8 bits are fixed (10.x.x.x)
- Valid RFC 1918 private range
- **This is the correct answer** because it provides the maximum number of IPs.

#### Option C: `172.16.0.0/12`
- **Medium-sized private range:** 1,048,576 IPs
- /12 means the first 12 bits are fixed (172.16-31.x.x)
- Smaller than /8

#### Option D: `192.168.0.0/16`
- **Smallest private range:** 65,536 IPs
- /16 means the first 16 bits are fixed (192.168.x.x)
- Much smaller than /8 or /12

### Why /8 is Larger than /12:

Think of it this way:
- **/8:** Only the first 8 bits are "locked" (the network part). You have 24 bits free for host addresses.
  - Range: 10.0.0.0 to 10.255.255.255
  - Free to use: All of 10.x.x.x

- **/12:** The first 12 bits are "locked". You have only 20 bits free for host addresses.
  - Range: 172.16.0.0 to 172.31.255.255
  - Free to use: 172.16-31.x.x (only part of the second octet)

**The smaller the /PREFIX number, the more bits available for hosts = more IPs!**

---

## Common Subnet Sizes for GCP

### Recommended Subnet Sizes:

| Use Case                          | Recommended CIDR | IPs     | Example          |
|-----------------------------------|------------------|---------|------------------|
| Very large VPC (enterprise)       | /8 or /9         | 16M-8M  | 10.0.0.0/8       |
| Large production environment      | /16              | 65,536  | 10.1.0.0/16      |
| Standard production subnet        | /20              | 4,096   | 10.1.0.0/20      |
| Medium subnet                     | /24              | 256     | 10.1.1.0/24      |
| Small subnet (dev/test)           | /28              | 16      | 10.1.1.0/28      |
| GKE cluster subnet                | /20 or larger    | 4,096+  | 10.2.0.0/20      |
| GKE secondary range (pods)        | /14 to /20       | 4K-256K | 10.4.0.0/14      |
| GKE secondary range (services)    | /20 to /24       | 256-4K  | 10.8.0.0/20      |

### GCP Subnet Constraints:

- **Minimum subnet size:** /29 (8 IPs, but only 4 usable due to GCP reservations)
- **Maximum subnet size:** Limited by VPC range (can be /8 if using 10.0.0.0/8)
- **GCP reserves 4 IPs per subnet:**
  1. Network address (first IP)
  2. Default gateway (second IP)
  3. Second-to-last IP (reserved for future use)
  4. Broadcast address (last IP)

**Example:** In a 10.0.0.0/29 subnet:
- Total IPs: 8
- Reserved: 10.0.0.0 (network), 10.0.0.1 (gateway), 10.0.0.6 (reserved), 10.0.0.7 (broadcast)
- Usable: 10.0.0.2, 10.0.0.3, 10.0.0.4, 10.0.0.5 (only 4 IPs)

---

## Visual Guide to CIDR Notation

### Breaking Down 10.0.0.0/8:

```
IP Address: 10.0.0.0
Binary:     00001010.00000000.00000000.00000000
            |------| <-- 8 bits for network
            |--------------------------------| <-- 32 bits total

/8 = First 8 bits are network, last 24 bits are for hosts
Network part: 00001010 (always 10)
Host part: 00000000.00000000.00000000 (can be anything: 0-255.0-255.0-255)

Valid IPs: 10.0.0.0 to 10.255.255.255
```

### Breaking Down 172.16.0.0/12:

```
IP Address: 172.16.0.0
Binary:     10101100.00010000.00000000.00000000
            |----------| <-- 12 bits for network
            |--------------------------------| <-- 32 bits total

/12 = First 12 bits are network, last 20 bits are for hosts
Network part: 10101100.0001 (172.16-31)
Host part: 0000.00000000.00000000

Valid IPs: 172.16.0.0 to 172.31.255.255
```

---

## Subnet Planning Strategies for GCP

### Strategy 1: Single Large Subnet (Simple)
Good for small organizations or simple architectures.

```
VPC: my-vpc (10.0.0.0/8)
└── Subnet: us-central1 (10.128.0.0/16)
    - 65,536 IPs
    - All resources in one subnet
```

### Strategy 2: Regional Subnets (Recommended)
One subnet per region for better organization.

```
VPC: my-vpc (10.0.0.0/8)
├── Subnet: us-central1 (10.1.0.0/16) - 65,536 IPs
├── Subnet: us-east1 (10.2.0.0/16) - 65,536 IPs
├── Subnet: europe-west1 (10.3.0.0/16) - 65,536 IPs
└── Subnet: asia-southeast1 (10.4.0.0/16) - 65,536 IPs
```

### Strategy 3: Environment-Based Subnets
Separate subnets for different environments.

```
VPC: my-vpc (10.0.0.0/8)
├── Subnet: prod-us-central1 (10.0.0.0/16) - 65,536 IPs
├── Subnet: staging-us-central1 (10.1.0.0/16) - 65,536 IPs
├── Subnet: dev-us-central1 (10.2.0.0/16) - 65,536 IPs
└── Subnet: gke-us-central1 (10.10.0.0/16) - 65,536 IPs
    ├── Primary range: 10.10.0.0/20 (4,096 IPs for nodes)
    ├── Secondary range (pods): 10.20.0.0/14 (262,144 IPs)
    └── Secondary range (services): 10.24.0.0/20 (4,096 IPs)
```

---

## Quick Reference Cheat Sheet

### Converting CIDR to Number of IPs:

| CIDR | Calculation | IPs              | Memory Aid                    |
|------|-------------|------------------|-------------------------------|
| /8   | 2^24        | 16,777,216       | ~16 million (Class A)         |
| /12  | 2^20        | 1,048,576        | ~1 million                    |
| /16  | 2^16        | 65,536           | ~64K (Class B)                |
| /20  | 2^12        | 4,096            | ~4K                           |
| /24  | 2^8         | 256              | 256 (Class C)                 |
| /28  | 2^4         | 16               | 16                            |
| /29  | 2^3         | 8                | 8 (GCP minimum)               |

### Easy Memory Trick:
- **/24 = 256 IPs** (most common, easy to remember)
- Each /1 smaller **doubles** the IPs: /23 = 512, /22 = 1,024, /21 = 2,048
- Each /1 larger **halves** the IPs: /25 = 128, /26 = 64, /27 = 32

---

## Common Exam Scenarios

### Scenario 1: "Which is the largest subnet?"
**Answer:** The one with the **smallest /PREFIX number**
- /8 > /12 > /16 > /24

### Scenario 2: "You need 1,000 IPs. What CIDR?"
**Answer:** Find next power of 2 above 1,000
- 2^10 = 1,024, so 32 - 10 = /22
- Use /22 (gives 4,096 IPs, well above 1,000)

### Scenario 3: "Subnets can't overlap"
If you have 10.1.0.0/16, you **cannot** create:
- ❌ 10.1.5.0/24 (overlaps with 10.1.0.0/16)
- ✅ 10.2.0.0/16 (doesn't overlap)

### Scenario 4: "GKE needs large IP ranges"
For a GKE cluster with 100 nodes and 10,000 pods:
- Node subnet: /24 (256 IPs, enough for 100 nodes)
- Pod secondary range: /14 (262,144 IPs, enough for 10,000 pods)
- Service secondary range: /20 (4,096 IPs for services)

---

## gcloud Examples

### Create a VPC with custom subnets:
```sh
# Create custom VPC (no auto subnets)
gcloud compute networks create my-vpc --subnet-mode=custom

# Create a large subnet (/8 range)
gcloud compute networks subnets create large-subnet \
    --network=my-vpc \
    --region=us-central1 \
    --range=10.0.0.0/8

# Create a medium subnet
gcloud compute networks subnets create medium-subnet \
    --network=my-vpc \
    --region=us-east1 \
    --range=172.16.0.0/12

# Create a standard subnet
gcloud compute networks subnets create standard-subnet \
    --network=my-vpc \
    --region=europe-west1 \
    --range=192.168.0.0/16
```

### Create a GKE subnet with secondary ranges:
```sh
gcloud compute networks subnets create gke-subnet \
    --network=my-vpc \
    --region=us-central1 \
    --range=10.1.0.0/20 \
    --secondary-range=pods=10.4.0.0/14 \
    --secondary-range=services=10.8.0.0/20
```

### Expand an existing subnet:
```sh
# Expand subnet from /24 to /20 (cannot shrink, only expand!)
gcloud compute networks subnets expand-ip-range my-subnet \
    --region=us-central1 \
    --prefix-length=20
```

---

## Key Takeaways for the Exam

1. **Lower /PREFIX = More IPs**
   - /8 has MORE IPs than /16
   - /16 has MORE IPs than /24

2. **RFC 1918 Ranges** (memorize these):
   - 10.0.0.0/8 (largest)
   - 172.16.0.0/12 (medium)
   - 192.168.0.0/16 (smallest)

3. **GCP Subnet Limits:**
   - Minimum: /29 (8 IPs)
   - Maximum: Depends on VPC range
   - Reserved: 4 IPs per subnet

4. **Cannot Overlap:**
   - Subnets in the same VPC cannot have overlapping CIDR ranges

5. **Can Expand, Cannot Shrink:**
   - You can expand a subnet to a smaller /PREFIX (e.g., /24 → /20)
   - You cannot shrink a subnet

6. **GKE Needs Large Ranges:**
   - Node subnet: /20 typical
   - Pod range: /14 typical (secondary range)
   - Service range: /20 typical (secondary range)

---

**Exam Tip:** When you see "as large as possible" for subnet ranges, choose the RFC 1918 range with the **smallest /PREFIX number**. That's 10.0.0.0/8!

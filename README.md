# BGP-Lab-1
# 📌 Lab Objective
This lab demonstrates the configuration and validation of Internal BGP (iBGP) within AS 254 using Cisco IOS. Key features include:
- Route reflector setup
- BGP communities (RFC 1997 format)
- NEXT_HOP modification via route-maps
- Default route origination
- Secure BGP session parameters
## 🌐 Lab Topology Overview
- Routers: R1, R2, R3, R4
- AS Number: 254
- Peerings via physical interfaces
- /30 links interconnecting backbone
- IP subnet assignments per topology diagram
## 🔧 Configuration Summary

### 🎯 Task 1: Basic Setup
- Hostnames and IP addressing configured based on topology
- Interfaces activated and verified

### 🔐 Task 2: BGP Core Configuration
- Static BGP Router-ID: `R1 → 1.1.1.1`, etc.
- iBGP peer sessions:
  - R1 ↔ R2, R3
  - R2 ↔ R1
  - R3 ↔ R1, R4
  - R4 ↔ R3
- TCP MD5 password: `CCNP`
- Hello interval: `5s`, Hold time: `15s`
- Communities: `send-community standard` enabled

### 🏷️ Task 3: Prefix Advertisement
- Networks advertised:
  - R1 → `150.1.1.0/24` with `254:111`
  - R2 → `150.2.2.0/24` with `254:222`
  - R3 → `150.3.3.0/24` with `254:333`
- Route-maps used under `network` command
- Verified with `show ip bgp` and community formats enabled via `ip bgp-community new-format`

---

## 📡 Task 4: Reachability & Path Resolution

### 🚀 Step 1: Default Route Origination
- R4 configured with `neighbor <R3> default-originate`
- Verified at R3 via `show ip bgp 0.0.0.0`

### 🧭 Step 2: Route Reflectors
- R1 → RR for R2
- R3 → RR for R4
- Solves iBGP non-advertisement issue

### 🛠️ Step 3: NEXT_HOP Attribute Adjustment
- Route-maps used to set `next-hop` on outbound updates
- Addresses inaccessible NEXT_HOP due to reflector behavior
## 🧪 Verification

- `show ip bgp summary` → Peer status
- `show ip bgp` → Routing table and community tagging
- `show ip bgp neighbors` → Community attribute propagation
- `extended ping` tests:
  - All routers confirm full LAN-to-LAN reachability
  - Source IPs tied to respective loopback/subnet interfaces
  ## ✅ Completion Criteria
- All iBGP peerings up and stable
- Communities correctly propagated
- Full routing table visibility across routers
- Successful extended ping tests across all 150.x.x.x/24 subnets
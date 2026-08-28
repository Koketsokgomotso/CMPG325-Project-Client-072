# Network Design

## 1. Design goals

1. Segment traffic by department/function using VLANs.
2. Guarantee internet access for Management even when other staff traffic is restricted.
3. Extend coverage to a new floor (CR2) without changing the assigned addressing block.
4. Use VLSM to allocate address space efficiently, leaving room for future growth.
5. Keep the design testable end-to-end in Cisco Packet Tracer.

## 2. Physical topology

```
Internet
   │
Edge Router  (WAN link, extended ACL: Management always permitted)
   │
Core Switch  (Layer 3 — inter-VLAN routing, DHCP)
   │
   ├── Ground floor switch
   │      ├── Management & admin VLAN
   │      ├── Design & engineering VLAN
   │      ├── Sales & client liaison VLAN
   │      ├── Site & warehouse operations VLAN
   │      └── Server VLAN
   │
   └── First floor switch  (CR2 — additional floor)
          ├── Floor 2 expansion VLAN
          └── Guest Wi-Fi (access point)
```

Each floor switch trunks all its VLANs back to the core switch. The core
switch (or the edge router, if a router-on-a-stick design is used instead of
an L3 switch) handles inter-VLAN routing and issues DHCP leases per VLAN.

## 3. Logical topology / VLAN plan

VLAN numbering follows a tens-based convention grouped by function, in line
with standard Cisco addressing practice: each department gets a distinct
ID incrementing by 10, and VLAN 1 (the default VLAN) is left unused on all
access ports as a security baseline. A dedicated native VLAN is used on all
trunk links so that untagged traffic never lands on a live data VLAN.

| VLAN ID | Name | Switch | Subnet | Default gateway |
|---|---|---|---|---|
| 10 | MGMT | Ground floor | 172.30.46.192/27 | 172.30.46.193 |
| 20 | DESIGN | Ground floor | 172.30.46.160/27 | 172.30.46.161 |
| 30 | SALES | Ground floor | 172.30.46.224/28 | 172.30.46.225 |
| 40 | SITE_OPS | Ground floor | 172.30.46.128/27 | 172.30.46.129 |
| 50 | SERVERS | Ground floor | 172.30.46.240/29 | 172.30.46.241 |
| 60 | GUEST | First floor | 172.30.46.0/26 | 172.30.46.1 |
| 70 | FLOOR2 | First floor | 172.30.46.64/26 | 172.30.46.65 |
| 99 | NATIVE | All trunks | Not assigned to hosts | — |

Notes on the convention:

- **VLAN 1** is never assigned to an access port — left as the unused factory default, per standard hardening practice.
- **VLAN 99 (NATIVE)** carries no host traffic; it is configured as the native VLAN on every trunk port between switches and the core, so accidental untagged frames don't land on a real subnet.
- The default gateway for each VLAN is the first usable address in its subnet, and is configured as the router subinterface / switch virtual interface (SVI) address.

## 4. Meeting the design constraint (management always online)

Implemented as an **extended ACL on the edge router's WAN-facing interface**:

1. `permit` — Management subnet (172.30.46.192/27) → any destination, any time.
2. `deny`/`permit` (time-based or general policy) — remaining staff VLANs, per the
   restriction policy in effect.
3. Rule order matters: the Management `permit` statement must be evaluated
   **before** any broader deny rule, otherwise Management traffic would be
   caught by the restriction too.

This should be demonstrated in Packet Tracer by testing internet reachability
from a Management PC and a restricted-VLAN PC simultaneously, with the
restriction active.

## 5. Meeting the change request (CR2 — additional floor)

Rather than requesting new address space, the first-floor expansion:

- Uses a subnet carved from the *existing* 172.30.46.0/23 block (172.30.46.64/26).
- Connects via a new access switch trunked back to the core switch, so it
  inherits the same inter-VLAN routing, DHCP, and ACL policy already in place.
- Does not alter any existing ground-floor VLAN or subnet — the original
  design continues to function unchanged.

## 6. Key design decisions (for the video defence)

- **VLSM over a single flat subnet**: departments have very different host
  counts (10–50), so VLSM avoids wasting addresses on small departments while
  still giving Guest Wi-Fi and the CR2 floor enough room.
- **L3 core switch / router-on-a-stick**: chosen to keep inter-VLAN routing
  centralised and to simplify ACL placement at the WAN edge.
- **Guest Wi-Fi isolated to its own VLAN**: keeps visiting clients off the
  internal departmental subnets entirely.

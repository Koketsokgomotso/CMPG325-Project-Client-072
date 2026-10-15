# IP Addressing Plan (VLSM)

## 1. Assigned block

**172.30.46.0/23** (172.30.46.0 – 172.30.47.255, 512 total addresses)

## 2. Method

Subnets were sized to each group's estimated host count, then allocated in
descending order of size (largest first) — the standard VLSM approach that
minimises wasted address space compared with a single fixed subnet mask
across all VLANs.

## 3. Host requirements

| Group | Estimated hosts |
|---|---|
| Guest Wi-Fi | 50 |
| Floor 2 expansion (CR2) | 40 |
| Site & warehouse operations | 30 |
| Design & engineering | 25 |
| Management & admin | 20 |
| Sales & client liaison | 10 |
| Servers | 6 |
| WAN link (router–ISP) | 2 |

## 4. VLSM allocation

VLAN IDs follow the tens-based convention defined in the Network Design
document (Section 3) — each subnet below is tied to a specific VLAN, not a
placeholder.

| VLAN ID | Group | Hosts needed | Mask | Subnet | Usable range | Broadcast |
|---|---|---|---|---|---|---|
| 60 | Guest Wi-Fi | 50 | /26 (62 usable) | 172.30.46.0 | 172.30.46.1 – .62 | 172.30.46.63 |
| 70 | Floor 2 expansion (CR2) | 40 | /26 (62 usable) | 172.30.46.64 | 172.30.46.65 – .126 | 172.30.46.127 |
| 40 | Site & warehouse ops | 30 | /27 (30 usable) | 172.30.46.128 | 172.30.46.129 – .158 | 172.30.46.159 |
| 20 | Design & engineering | 25 | /27 (30 usable) | 172.30.46.160 | 172.30.46.161 – .190 | 172.30.46.191 |
| 10 | Management & admin | 20 | /27 (30 usable) | 172.30.46.192 | 172.30.46.193 – .222 | 172.30.46.223 |
| 30 | Sales & client liaison | 10 | /28 (14 usable) | 172.30.46.224 | 172.30.46.225 – .238 | 172.30.46.239 |
| 50 | Servers | 6 | /29 (6 usable) | 172.30.46.240 | 172.30.46.241 – .246 | 172.30.46.247 |
| — | WAN link (router–ISP) | 2 | /30 (2 usable) | 172.30.46.248 | 172.30.46.249 – .250 | 172.30.46.251 |
| 99 | Native (trunk only, no hosts) | — | — | — | — | — |

## 5. Unused address space

`172.30.46.252/30` through the entire `172.30.47.0/24` remains unallocated —
one whole spare /24 plus a /30. This is a deliberate design choice, not an
oversight: it keeps the plan efficient today while leaving clear room for
further growth (e.g. if the client expands beyond the CR2 floor in future),
which directly supports the appropriateness argument required by the brief.

## 6. Why this is appropriate

- Each subnet is sized close to its actual requirement (no group is given a
  mask larger than it needs), which is the core justification VLSM requires
  over fixed-length subnetting.
- The two largest groups (Guest Wi-Fi and the CR2 floor) — both areas expected
  to grow or fluctuate in headcount — were given the most headroom (/26, 62
  usable each).
- The WAN link uses a /30, the smallest usable subnet, since a point-to-point
  link only ever needs two addresses.
- Significant free space remains in the block for future subnets without
  needing to touch the client's assigned 172.30.46.0/23 range.

## 7. How to verify (for testing evidence / video demo)

1. Confirm each VLAN interface (SVI or subinterface) is configured with the
   correct network address and subnet mask from the table above.
2. Confirm DHCP pools per VLAN exclude the gateway address and match the
   usable range.
3. `ping` between two hosts within the same VLAN — should succeed.
4. `ping` between hosts in different VLANs — should succeed only via the
   router/L3 switch (confirms inter-VLAN routing works).
5. `show ip route` on the router/L3 switch — confirm all subnets appear as
   connected routes.
6. Capture `ipconfig`/`show running-config` screenshots as evidence for the
   GitHub portfolio.

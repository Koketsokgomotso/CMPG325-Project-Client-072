# Client Requirements Analysis

## 1. Client background

**Organisation:** Thabang Building Contractors (Klerksdorp)
**Industry:** Construction
**Client ID:** CLI-072
**Project ID:** CMPG325-2026-072

Thabang Building Contractors operates from an office building in Klerksdorp,
housing management, administration, design/engineering staff, sales/client
liaison, and site/warehouse operations, in addition to a general contracting
workforce that spends most of its time on client construction sites. The
office network must support day-to-day administrative and design work, provide
guest access for visiting clients, and now extend coverage to an additional
floor the client has taken over.

## 2. Stated requirements

- Assigned addressing block: **172.30.46.0/23**
- Provide appropriate connectivity and network services for the assigned scenario
- Accommodate the stated design constraint and change request (see below)
- Produce a working, testable Packet Tracer implementation
- Permit successful data exchange between the appropriate nodes
- Demonstrate the assigned networking challenge (IPv4 subnetting / VLSM)

## 3. Design constraint

> Management requires internet access even when the staff network is restricted.

**Interpretation:** the network must be able to apply internet restrictions to
general staff (for example, during working hours, or as a general policy) while
guaranteeing that the Management VLAN retains internet access at all times.

**Design implication:** this cannot be solved by VLAN separation alone — it
requires an access control list (ACL) on the edge router that explicitly
permits the Management subnet's traffic to the WAN interface *before* any
deny/restriction rule that applies to other staff VLANs. See
[`02-network-design.md`](02-network-design.md) for the ACL approach.

## 4. Change request — CR2

> The client takes over an additional floor/area of the building and needs
> coverage there.

**Interpretation:** the existing network design must be extended to cover a new
physical floor, without changing the client's assigned addressing block
(172.30.46.0/23) and without disrupting the existing ground-floor VLANs.

**Design implication:** the VLSM plan was built with this in mind — the
addressing plan allocates a dedicated subnet for the new floor from the same
/23 block, and enough of the block is left unallocated to accommodate further
growth if needed. See [`03-ip-addressing-plan.md`](03-ip-addressing-plan.md).

## 5. Identified departments / traffic groups

Based on a typical construction contractor's office structure, the following
functional groups were identified for VLAN segmentation. (Adjust these if your
lecturer-issued brief specifies different department names or headcounts.)

| Group | Function |
|---|---|
| Management & admin | Directors, finance, HR — requires guaranteed internet access |
| Design & engineering | Site engineers, draughtsmen, project planning |
| Sales & client liaison | Client-facing sales and quoting staff |
| Site & warehouse operations | Equipment, materials, site coordination staff |
| Servers | File server / shared resources |
| Guest Wi-Fi | Visiting clients and contractors — restricted access |
| Floor 2 (CR2 expansion) | New floor taken over by the client |

## 6. Assigned technical challenge

**IPv4 Subnetting (VLSM addressing plan)** — Intermediate difficulty. The
project must configure, verify, and demonstrate a VLSM addressing plan within
the client network, and be able to explain what was configured, why it is
appropriate, and how it was verified.

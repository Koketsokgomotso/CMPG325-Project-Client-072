CMPG 325 - Computer Networks: Thabang Building Contractors

Student Name: MENGWAI, KKE
Student Number: 44419465
Project ID: CMPG325-2026-072
Client ID: CLI-072
Submission Date: 16 October 2026
Project Overview

This repository serves as my complete Portfolio of Evidence for the CMPG 325 semester project.

The project involves designing, simulating, and documenting a fully functional computer network for Thabang Building Contractors, a construction company based in Klerksdorp. The client requires a robust, scalable, and secure network built from the assigned addressing block (172.30.46.0/23). The final solution is simulated and tested using Cisco Packet Tracer.

Client Requirements & Key Challenges
   Assigned Feature: IPv4 Subnetting using VLSM (Variable Length Subnet Masking).

   Design Constraint: Provide internet access to the Management department even when the general Staff network is restricted (achieved via VLAN segmentation and ACLs).

   Change Request (CR2): The client has taken over an additional floor/area. The network design must be scalable to accommodate this new space with minimal disruption.

🛠️ Technologies & Protocols Used

    Simulation Tool: Cisco Packet Tracer

    Routing: Inter-VLAN Routing (Router-on-a-Stick)

    Switching: VLANs, Trunking (802.1Q), Access Ports

    Addressing: VLSM (Variable Length Subnet Masking) for efficient IP allocation.

    Security: Access Control Lists (ACLs) to restrict staff internet access while allowing management.

    Services: DHCP for dynamic IP allocation to end devices.


Repository Structure
This repository is organized to clearly document every phase of the project lifecycle, from initial analysis to final testing.
Folder	Description
   01_Client_Requirements	Analysis of the client brief, background, constraints, and Change Request CR2.
   02_Network_Design	Physical and Logical topology diagrams, VLAN design, and justification of design decisions.
   03_IP_Addressing_Plan	Detailed VLSM calculation tables, subnet allocations, and the addressing scheme.
   04_Packet_Tracer	The final working .pkt file and exported configurations for routers and switches.
   05_Testing_Evidence	Screenshots of ping tests, extended ACL verifications, and troubleshooting logs.

Testing Summary
    Connectivity: End-to-end connectivity verified across all VLANs.

    VLSM Verification: All subnets strictly adhere to the allocated address block.

    Design Constraint: Management VLAN successfully retains internet access while Staff VLAN is restricted via ACLs.

    Change Request (CR2): The new floor's switch and hosts are seamlessly integrated into the existing topology.

Academic Integrity & AI Policy

I, MENGWAI, KKE (44419465), confirm that this project is my own original work.

    All configurations, designs, and documentation were created by me.

    Any AI-assisted coding or research has been used purely as a learning tool and has been thoroughly reviewed, tested, and understood by me.

    This repository complies with the NWU AI Policy and the CMPG 325 Academic Integrity guidelines.

Project Timeline

    Start Date: 14 August 2026

    Milestone 1 (Design): 28 August 2026

    Milestone 2 (Implementation): 02 October 2026

    Final Submission: 16 October 2026
06_Video_Demonstration	Link to the 15–20 minute inset video demonstration.
07_Reflection	Personal reflection on the project, challenges faced, and lessons learned.

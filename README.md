# CMPG325-2026-114 — Mafikeng Football Club Network Design

**Student:** Ramatlapeng, M (39433080)
**Course:** CMPG 325 — Computer Networks, NWU
**Project ID:** CMPG325-2026-114
**Client ID:** CLI-114
**Client:** Mafikeng Football Club (Mahikeng)
**Industry:** Sports

## Project Overview

This repository documents the design, simulation, and testing of a computer network for Mafikeng Football Club, built in Cisco Packet Tracer as an individual semester project for CMPG 325.

The club is relocating into a shared office building and expanding onto a second floor to house a new Youth Academy and Sports Science department, funded by a recent sponsorship deal. The network is designed to support this growth from the start, using the assigned addressing block **10.43.0.0/16**.

## Client Requirements Summary

- Administration, Coaching/Technical, and Media/Ticketing departments on the ground floor
- Youth Academy and Sports Science departments on the first floor (Change Request CR2)
- Guest wireless access on both floors, isolated from staff networks
- A file server and web server reachable by staff departments
- Design must accommodate a further floor being occupied in the next financial year (design constraint)
- Assigned networking challenge: **Network Troubleshooting — fault isolation scenario** (Advanced difficulty)

Full detail is in [`01-requirements/client-requirements.md`](01-requirements/client-requirements.md).

## Network Design Summary

| VLAN | Name | Subnet | Gateway |
|---|---|---|---|
| 10 | Administration | 10.43.10.0/24 | 10.43.10.1 |
| 20 | Coaching/Technical | 10.43.20.0/24 | 10.43.20.1 |
| 30 | Media/Ticketing | 10.43.30.0/24 | 10.43.30.1 |
| 40 | Guest Wi-Fi (Ground Floor) | 10.43.40.0/24 | 10.43.40.1 |
| 50 | Youth Academy/Sports Science (CR2) | 10.43.50.0/24 | 10.43.50.1 |
| 60 | Wi-Fi (First Floor, CR2) | 10.43.60.0/24 | 10.43.60.1 |
| 99 | Management | 10.43.99.0/24 | 10.43.99.1 |

Routing uses **router-on-a-stick**: a single router (R1) with one sub-interface per VLAN, trunked to a core switch (Core-SW), which in turn trunks to one access switch per floor (SW-GF, SW-FF).

Full addressing detail is in [`03-ip-addressing/ip-plan.md`](03-ip-addressing/ip-plan.md).

## Repository Structure

```
/README.md                          — this file
/01-requirements/
    client-requirements.md          — full client requirements writeup
/02-topology/
    physical-topology.png           — device layout and cabling
    logical-topology.png            — VLAN structure and IP allocation
/03-ip-addressing/
    ip-plan.md                      — full IP addressing plan
/04-packet-tracer/
    mafikeng-fc-network.pkt         — working Packet Tracer file
/05-config/
    r1-running-config.txt           — router configuration export
    core-sw-running-config.txt      — core switch configuration export
    sw-gf-running-config.txt        — ground floor switch configuration export
    sw-ff-running-config.txt        — first floor switch configuration export
/06-testing/
    connectivity-tests.md           — ping/traceroute evidence, screenshots
/07-troubleshooting/
    fault-isolation-writeup.md      — the assigned networking challenge, demonstrated
/08-reflection/
    reflection.md                   — short reflection on the completed project
```

## Build Notes

The Packet Tracer simulation uses a representative subset of end devices (two PCs per department, one wireless client per floor) rather than the full staff complement listed in the client requirements. The VLAN and subnet design supports the full staff numbers; the reduced device count keeps the simulation manageable in Packet Tracer while still proving connectivity, inter-VLAN routing, and the fault isolation challenge across every VLAN.

## Project Milestones

| Milestone | Date | Status |
|---|---|---|
| Project commencement | 14 Aug 2026 | Complete |
| Milestone 1 — Client Design Review | 28 Aug 2026 | In progress |
| Milestone 2 | 2 Oct 2026 | Not started |
| Final submission | 16 Oct 2026 | Not started |

## Academic Integrity

This solution is built specifically for the assigned client (Mafikeng Football Club, CLI-114) and has not been substituted with or copied from another student's project scenario. AI assistance was used in line with the NWU AI Policy; the author remains responsible for the correctness, understanding, and verification of everything submitted.

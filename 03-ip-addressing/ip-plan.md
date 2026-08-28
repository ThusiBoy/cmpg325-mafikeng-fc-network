# IP Addressing Plan — Mafikeng Football Club

**Assigned block:** 10.43.0.0/16

All subnets are drawn from the assigned block. A /24 is used per VLAN — the block is large enough that VLSM efficiency is not necessary, and /24s keep the scheme easy to read and troubleshoot during the fault isolation demonstration.

| VLAN | Name | Subnet | Gateway | DHCP Range |
|---|---|---|---|---|
| 10 | Administration | 10.43.10.0/24 | 10.43.10.1 | .10 – .100 |
| 20 | Coaching/Technical | 10.43.20.0/24 | 10.43.20.1 | .10 – .100 |
| 30 | Media/Ticketing | 10.43.30.0/24 | 10.43.30.1 | .10 – .100 |
| 40 | Guest Wi-Fi (Ground Floor) | 10.43.40.0/24 | 10.43.40.1 | .10 – .200 |
| 50 | Academy/Sports Science (CR2) | 10.43.50.0/24 | 10.43.50.1 | .10 – .100 |
| 60 | Wi-Fi (First Floor, CR2) | 10.43.60.0/24 | 10.43.60.1 | .10 – .100 |
| 99 | Management | 10.43.99.0/24 | 10.43.99.1 | Static only |

## Static Assignments (outside DHCP)

- File server — 10.43.10.250
- Web/ticketing server — 10.43.10.251
- Router sub-interfaces — first usable address (.1) in each VLAN subnet
- Switch management interfaces — 10.43.99.0/24, assigned statically

## Reserved for Future Growth

10.43.70.0/24 through 10.43.90.0/24 are held back for the design constraint (a further floor next financial year), so expansion does not require renumbering existing VLANs.

## Devices Deployed (Packet Tracer Build)

A representative subset of two end devices per department VLAN, plus one wireless client per floor:

| Device | VLAN | IP |
|---|---|---|
| Admin-PC1 | 10 | 10.43.10.10 |
| Admin-PC2 | 10 | 10.43.10.11 |
| Coach-PC1 | 20 | 10.43.20.10 |
| Coach-PC2 | 20 | 10.43.20.11 |
| Media-PC1 | 30 | 10.43.30.10 |
| Media-PC2 | 30 | 10.43.30.11 |
| Guest-Laptop | 40 | 10.43.40.10 |
| Academy-PC1 | 50 | 10.43.50.10 |
| Academy-PC2 | 50 | 10.43.50.11 |
| SciSci-PC1 | 50 | 10.43.50.12 |
| Boardroom-Laptop | 60 | 10.43.60.10 |
| File-Server | 10 | 10.43.10.250 |
| Web-Server | 10 | 10.43.10.251 |

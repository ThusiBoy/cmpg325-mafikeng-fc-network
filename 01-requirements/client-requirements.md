# Client Requirements — Mafikeng Football Club

**Project ID:** CMPG325-2026-114 | **Client ID:** CLI-114

## Background

Mafikeng Football Club, a semi-professional club competing in the ABC Motsepe League, is relocating its administrative operations into a shared office building near the club stadium. The club has been assigned addressing block **10.43.0.0/16** for the new network.

## Departments and User Groups

- **Administration** (4 staff) — club records, finance, HR, general correspondence
- **Coaching/Technical** (6 staff, 4 shared workstations) — match analysis, training schedules, player performance tracking
- **Media/Ticketing** (3 staff) — social media, match-day ticketing, club website content
- **Reception/Guest access** — front-desk kiosk plus guest wireless for sponsors and visiting scouts
- **Server room** — file server for club records and player contracts, web server hosting the club site and ticketing backend

## Design Constraint

An additional floor in the same building may be occupied by the club in the next financial year. The network must be built with spare address space and physical capacity to accommodate this without a redesign.

## Change Request CR2

The club has secured a sponsorship deal and is expanding onto the first floor to house:

- **Youth Academy administration** (2 staff)
- **Sports Science department** (3 staff — physiotherapist, data analyst, nutritionist)
- **Boardroom** for sponsor meetings

The final network must provide full coverage and connectivity to this floor.

## Assigned Networking Challenge

**Network Troubleshooting (fault isolation scenario)** — classified Advanced for CMPG 325. The network design supports a realistic fault being introduced and diagnosed using standard troubleshooting commands.

## Connectivity Requirements

- All departments must reach the file server and web server
- Guest wireless (both floors) must reach the internet only, not internal department VLANs
- Staff VLANs must be able to route to each other and to the servers, subject to normal inter-VLAN routing
- The design must be extensible: adding a further floor or department should only require a new VLAN and subnet, not a redesign of the addressing scheme

## Scope

This solution addresses only the Mafikeng Football Club scenario as assigned (CLI-114) and has not been substituted with any other client scenario.

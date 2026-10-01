# Fault Isolation Troubleshooting — Missing VLAN on Trunk Link

**Assigned challenge:** Network Troubleshooting (fault isolation scenario), Advanced CMPG 325

## Scenario

While performing routine maintenance on Core-SW, a technician accidentally removed VLAN 50 (Youth Academy / Sports Science, first floor — Change Request CR2) from the trunk link connecting Core-SW to SW-FF. All other VLANs on the link were unaffected. This is a realistic, common misconfiguration: a single `switchport trunk allowed vlan` edit that drops one VLAN from an otherwise-correct trunk.

## Fault Introduced

On Core-SW, the trunk port facing SW-FF (Fa0/5) had VLAN 50 explicitly removed:

```
interface fastEthernet 0/5
switchport trunk allowed vlan remove 50
```

**Evidence:** `fault-01-trunk-missing-vlan50.png` — `show interfaces trunk` on Core-SW shows Fa0/5's allowed VLANs as `1-49,51-1005`, confirming VLAN 50 is excluded while every other VLAN remains allowed.

## Symptom

Academy-PC1 (VLAN 50, IP 10.43.50.10) could no longer reach the file server (10.43.10.250):

```
ping 10.43.10.250
Request timed out. (x4) — 100% loss
```

**Evidence:** `fault-02-symptom-ping-fails.png`

## Diagnosis

Diagnosis followed a methodical, step-by-step process, narrowing the fault location at each stage rather than guessing.

**Step 1 — Check the PC can reach its own gateway.**

```
ping 10.43.50.1
Request timed out. (x4) — 100% loss
```

Even the default gateway was unreachable. This immediately ruled out a routing issue on R1 and pointed to a Layer 2 path problem between Academy-PC1 and R1.

**Evidence:** `fault-03-diagnosis-gateway-unreachable.png`

**Step 2 — Check SW-FF's VLAN assignment.**

```
show vlan brief
```

VLAN 50 still correctly listed Fa0/2, Fa0/3, Fa0/4 (Academy-PC1, Academy-PC2, SciSci-PC1). This ruled out SW-FF's access-port configuration as the cause.

**Evidence:** `fault-04-swff-vlan-unaffected.png`

**Step 3 — Check SW-FF's side of the trunk.**

```
show interfaces trunk
```

SW-FF's trunk port (Fa0/1, facing Core-SW) still showed VLAN 50 in its allowed list. This ruled out SW-FF's side of the link entirely, leaving only one remaining possibility: Core-SW's side of the same link.

**Evidence:** `fault-05-swff-trunk-vlan50-present.png`

**Step 4 — Check Core-SW's side of the trunk.**

```
show interfaces trunk
```

Core-SW's Fa0/5 (facing SW-FF) showed its allowed VLAN list as `1-49,51-1005` — VLAN 50 explicitly missing. Root cause confirmed.

**Evidence:** `fault-06-coresw-trunk-vlan50-missing.png`

## Fix

On Core-SW:

```
interface fastEthernet 0/5
switchport trunk allowed vlan add 50
```

Using `add` rather than re-stating the full VLAN list avoided disturbing any other VLAN already permitted on the trunk.

**Evidence:** `fault-07-coresw-trunk-fixed.png` — `show interfaces trunk` now shows Fa0/5 with the full VLAN list restored, including 50.

## Verification

```
ping 10.43.10.250
Request timed out.
Reply from 10.43.10.250: bytes=32 time=1ms TTL=127
Reply from 10.43.10.250: bytes=32 time<1ms TTL=127
Reply from 10.43.10.250: bytes=32 time<1ms TTL=127
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)
```

The single initial timeout is a normal ARP resolution delay, consistent with every other first-ping result observed throughout this project. All subsequent replies succeeded, confirming VLAN 50 connectivity to the file server was fully restored.

**Evidence:** `fault-08-verified-fixed.png`

## Summary

| Step | Command | Result |
|---|---|---|
| Symptom | `ping 10.43.10.250` from Academy-PC1 | Failed, 100% loss |
| Narrow to Layer 2 | `ping 10.43.50.1` (own gateway) from Academy-PC1 | Failed, 100% loss |
| Rule out SW-FF VLAN config | `show vlan brief` on SW-FF | VLAN 50 correctly assigned |
| Rule out SW-FF trunk | `show interfaces trunk` on SW-FF | VLAN 50 present on Fa0/1 |
| Root cause found | `show interfaces trunk` on Core-SW | VLAN 50 missing on Fa0/5 |
| Fix applied | `switchport trunk allowed vlan add 50` on Core-SW Fa0/5 | VLAN 50 restored |
| Verified | `ping 10.43.10.250` from Academy-PC1 | Succeeded |

This demonstrates the assigned fault isolation challenge: a realistic fault was introduced, diagnosed through a systematic elimination process rather than guesswork, and resolved with a minimal, targeted fix — restoring full connectivity without disrupting any other VLAN on the network.

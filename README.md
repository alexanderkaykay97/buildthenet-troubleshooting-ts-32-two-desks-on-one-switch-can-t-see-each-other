# Troubleshooting #32: Two Desks on One Switch Can't See Each Other

_Level: Easy — BuildTheNet 3D Sandbox troubleshooting — VLANs_

**Status: Verified.** Completed; every requirement was machine-checked against the network built in BuildTheNet and passed.

Snapshot taken 2026-11-14. Built in a simulation — no physical or production equipment was involved.

## The ticket

**Reported symptom:** PC-A and PC-B — same switch, same address range — can't reach each other since yesterday's patching.

Helpdesk Ticket #132: PC-A and PC-B sit on SW-Floor1 in 192.168.10.0/24 and share files every day. Since a patching job yesterday they cannot see each other. Both are STAFF desks; the GUEST VLAN on that switch is for visitor kiosks only.

_Area: VLANs_

## Required resolution

Get PC-A and PC-B talking again, with both desks in the STAFF VLAN.

## Checks

| # | Check | Status | Checked by |
|---|---|---|---|
| 1 | PC-A to PC-B works | Passed | Sandbox check |
| 2 | PC-A keeps its documented addressing | Passed | Sandbox check |
| 3 | PC-B keeps its documented addressing | Passed | Sandbox check |
| 4 | The ticket's service requirement is met | Passed | Sandbox check |

## Root cause

SW-Floor1 Fa0/2 was left in VLAN 20 while Fa0/1 is in VLAN 10.

## The fix

What was changed to resolve the ticket, compared with the network as it was reported:

Compared with the starting topology, the learner changed the configuration of 1 device.

**SW-Floor1**

```diff
  interface Fa0/2
-  switchport access vlan 20
+  switchport access vlan 10
```

## Result

Every check passed on this snapshot:

> Both desks are back in the STAFF VLAN and reach each other.

## Topology

![Topology diagram](topology.svg)

## Addressing

| Device | Interface | Address | Mask | Mode | Gateway |
|---|---|---|---|---|---|
| PC-A | FastEthernet0 | 192.168.10.11/24 | 255.255.255.0 | VLAN 10 |  |
| PC-B | FastEthernet0 | 192.168.10.12/24 | 255.255.255.0 | VLAN 10 |  |

## Device configurations

Running-configs as `show running-config` prints them in the BuildTheNet sandbox. Passwords, secrets, keys and SNMP communities are replaced with `<removed>`.

- [SW-Floor1](configs/SW-Floor1.txt)

---

Built on [BuildTheNet](https://www.buildthenet.io/sandbox)

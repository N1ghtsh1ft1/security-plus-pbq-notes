# Switching, Routing and Network Segmentation

## The task

Design or repair a segmented topology: assign VLANs, decide which ports are access and which are trunk, place the routing, and apply the control that fixes a stated problem. Or read a diagram and say why two hosts cannot reach each other.

![Segmented network](../diagrams/segmentation.svg)

## Switching fundamentals

A switch forwards on **MAC address** at layer 2 and keeps a CAM table mapping MAC to port. Each switch port is its own collision domain; by default the whole switch is one broadcast domain.

A **VLAN** splits that single broadcast domain into several logical ones. Hosts in different VLANs cannot reach each other at layer 2 at all — they need a router or a layer 3 switch. That isolation is the security value, and it is why VLANs answer "contain the blast radius" scenarios.

**Access port** — belongs to exactly one VLAN, carries untagged frames, connects to an endpoint. **Trunk port** — carries multiple VLANs between switches using 802.1Q tags, plus one untagged native VLAN.

## Segmentation patterns

| Pattern | Purpose |
|---|---|
| VLAN per function | Separate users, servers, voice, management, guest |
| Screened subnet (DMZ) | Internet-facing services in their own zone; no direct path from internet to internal |
| Management network | Out-of-band administrative access, unreachable from user VLANs |
| Air gap | No network connection at all — the strongest and least usable option |
| Microsegmentation | Per-workload policy, typically enforced in software rather than by VLAN |
| Guest / IoT VLAN | Internet only, no route to internal resources |

The right answer for "an infected user workstation reached the domain controller" is almost always segmentation plus ACLs between segments, not a bigger endpoint agent.

## Routing

Routers forward on **IP address** at layer 3 and separate broadcast domains by definition. Inter-VLAN routing is done either by a **router on a stick** — one physical link carrying a trunk, with a subinterface per VLAN — or by a **layer 3 switch** with switched virtual interfaces, which is what production actually uses.

Route selection: longest prefix match wins first, then administrative distance between protocols, then metric within a protocol. A `/32` host route beats a `/24` beats a `/0` default, regardless of how the routes were learned.

## Switch hardening

Disable unused ports rather than leaving them live. Set the native VLAN on trunks to an unused VLAN ID and never carry user traffic on it — that is the defence against double-tagging VLAN hopping. Disable DTP so ports cannot negotiate themselves into trunks, which is the defence against switch spoofing. Use **port security** to limit MAC addresses per port against CAM table flooding. Turn on **DHCP snooping** against rogue DHCP servers, and **dynamic ARP inspection** — which depends on the snooping binding table — against ARP poisoning. Enable **BPDU guard** on access ports so an endpoint cannot influence spanning tree. Use **802.1X** for port-based authentication so a physical jack is not itself an authorisation.

## Attacks this defends against

**VLAN hopping** by switch spoofing (host negotiates a trunk — kill DTP) or double tagging (two 802.1Q tags, the outer stripped at the first switch — fix the native VLAN). **MAC flooding** overflows the CAM table so the switch fails open and floods frames like a hub — port security. **ARP poisoning** puts the attacker between two hosts by forging ARP replies — dynamic ARP inspection. **Rogue DHCP** hands out an attacker gateway — DHCP snooping.

## Traps

Two hosts in different VLANs on the same switch cannot ping each other, and that is correct behaviour, not a fault. A trunk carries tagged frames; putting a workstation on a trunk port usually breaks it. The native VLAN is untagged, which is exactly why it must not be VLAN 1 and must not carry data. Dynamic ARP inspection without DHCP snooping has no binding table to check against and does nothing. A layer 3 switch does route, so "switches cannot route" is not universally true.

## Self-check

- Sales is VLAN 10 and Engineering is VLAN 20 on the same switch. Neither can reach the other. What is required, and give two ways to provide it.
- What two configuration changes defeat the two forms of VLAN hopping, and which change defeats which?
- Why does DAI need DHCP snooping enabled first?
- Given routes 0.0.0.0/0, 10.0.0.0/8 and 10.1.2.0/24, which carries traffic to 10.1.2.55, and what rule decides it?

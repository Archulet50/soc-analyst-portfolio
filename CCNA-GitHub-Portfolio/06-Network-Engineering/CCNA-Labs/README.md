# CCNA Network Engineering Labs

Five hands-on Cisco Packet Tracer projects demonstrating foundational switching, routing, network services, and traffic-control skills. Each lab was configured from the Cisco IOS CLI, tested end to end, and troubleshot using operational show commands and packet-level reasoning.

## Lab portfolio

| # | Project | Core skills | Validation |
|---:|---|---|---|
| 1 | [VLANs and Trunking](01-VLANs-Trunking/) | VLAN creation, access ports, 802.1Q, native VLANs, STP | Same-VLAN traffic passed across the trunk; cross-VLAN traffic remained isolated |
| 2 | [Inter-VLAN Routing](02-Inter-VLAN-Routing/) | Router-on-a-stick, subinterfaces, gateways | VLAN 10 reached VLAN 20 through R1 |
| 3 | [OSPF Multi-Router Network](03-OSPF-Multi-Router-Network/) | /30 links, OSPF area 0, neighbors, passive interfaces | PC1 reached PC3 across R1–R2–R3 |
| 4 | [DHCP and NAT/PAT](04-DHCP-NAT-PAT/) | DHCP pools, exclusions, default route, NAT overload | Two private clients shared one inside-global address |
| 5 | [ACL Network Security](05-ACL-Network-Security/) | Named extended ACLs, placement, match counters | PC2 ICMP/HTTP denied while other traffic remained permitted |

## Environment

- Cisco Packet Tracer
- Cisco 2911 routers
- Cisco 2960-24TT switches
- IPv4, Cisco IOS CLI, ICMP, HTTP

## How to use

1. Install Cisco Packet Tracer.
2. Download the `.pkt` file from the desired project folder.
3. Open it in Packet Tracer.
4. Review the project README before inspecting device configurations.
5. Use the documented verification commands and repeat the tests.

## Troubleshooting demonstrated

- Interpreted temporary Spanning Tree PVID inconsistency during mismatched trunk configuration.
- Identified an automatically selected switch port with CDP and moved trunk configuration to the actual connected interface.
- Distinguished expected ARP-related first-packet loss from persistent routing failure.
- Isolated an incorrect server subnet one layer at a time; corrected `198.151.100.10` to `198.51.100.10`.
- Verified ACL operation with per-entry match counters and NAT/PAT with live translation entries.

## Author

Brent Archuleta — cybersecurity and network engineering portfolio.

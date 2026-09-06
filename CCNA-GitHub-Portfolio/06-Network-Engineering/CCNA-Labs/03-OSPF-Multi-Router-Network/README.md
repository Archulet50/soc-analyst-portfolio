# Project 3 — OSPF Multi-Router Network

## Objective

Build a three-router topology and use single-area OSPF to exchange LAN routes dynamically across point-to-point links.

## Addressing plan

| Device/interface | Address | Purpose |
|---|---|---|
| R1 G0/0 | `192.168.1.1/24` | LAN 1 gateway |
| R1 G0/1 | `10.0.12.1/30` | R1–R2 |
| R2 G0/0 | `192.168.2.1/24` | LAN 2 gateway |
| R2 G0/1 | `10.0.12.2/30` | R2–R1 |
| R2 G0/2 | `10.0.23.1/30` | R2–R3 |
| R3 G0/0 | `192.168.3.1/24` | LAN 3 gateway |
| R3 G0/1 | `10.0.23.2/30` | R3–R2 |

PC1, PC2, and PC3 use `.10` in their respective `/24` LANs.

## OSPF design

| Router | Router ID | Advertised networks |
|---|---|---|
| R1 | `1.1.1.1` | `192.168.1.0/24`, `10.0.12.0/30` |
| R2 | `2.2.2.2` | `192.168.2.0/24`, both `/30` links |
| R3 | `3.3.3.3` | `192.168.3.0/24`, `10.0.23.0/30` |

All networks are in area 0. Each `G0/0` LAN interface is passive so the LAN is advertised without sending unnecessary OSPF hellos toward endpoints.

## Example configuration

```cisco
router ospf 1
 router-id 1.1.1.1
 passive-interface gigabitethernet 0/0
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
```

## Verification

```cisco
show ip protocols
show ip ospf neighbor
show ip route ospf
```

R2 formed full adjacencies with R1 and R3. PC1 reached PC3 after OSPF convergence. A returned TTL of 125 confirmed traversal through three routers; only the first probe was lost during ARP resolution.

## Lab file

[Download the Packet Tracer lab](CCNA-Project-03-OSPF-Multi-Router-Network.pkt)

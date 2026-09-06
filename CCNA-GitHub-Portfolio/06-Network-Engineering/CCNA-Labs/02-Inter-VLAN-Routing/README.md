# Project 2 — Inter-VLAN Routing

## Objective

Extend the VLAN lab with router-on-a-stick so ADMIN and SECURITY hosts can communicate through a Layer 3 gateway while retaining VLAN separation at Layer 2.

## Addressing

| VLAN | Purpose | Router subinterface | Gateway |
|---:|---|---|---|
| 10 | ADMIN | R1 `G0/0.10` | `192.168.10.1/24` |
| 20 | SECURITY | R1 `G0/0.20` | `192.168.20.1/24` |
| 99 | Native/management | R1 `G0/0.99` | `192.168.99.1/24` |

R1 `G0/0` connects to SW1 `Fa0/3`. The switch port is an 802.1Q trunk allowing VLANs 10, 20, and 99.

## Key router configuration

```cisco
interface gigabitethernet 0/0
 no ip address
 no shutdown

interface gigabitethernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface gigabitethernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface gigabitethernet 0/0.99
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0
```

## Key switch configuration

```cisco
interface fastethernet 0/3
 description TRUNK_TO_R1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
```

## Verification

```cisco
show ip interface brief
show interfaces trunk
```

ADMIN-PC1 successfully reached `192.168.20.11` after its gateway was set to `192.168.10.1`. The routed response used TTL 127, showing that the traffic crossed R1.

## Troubleshooting note

Packet Tracer automatically connected R1 to SW1 `Fa0/3`, while the initial trunk configuration targeted unused `Gi0/2`. `show cdp neighbors` exposed the actual local interface, and moving the trunk configuration to `Fa0/3` restored connectivity.

## Lab file

[Download the Packet Tracer lab](CCNA-Project-02-Inter-VLAN-Routing.pkt)

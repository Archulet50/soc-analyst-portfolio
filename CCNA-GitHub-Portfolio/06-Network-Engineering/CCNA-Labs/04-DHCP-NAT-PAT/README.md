# Project 4 — DHCP and NAT/PAT

## Objective

Provide automatic IPv4 addressing to an inside LAN and allow multiple private clients to reach a simulated public server through one translated address using PAT.

## Addressing plan

| Segment/device | Address |
|---|---|
| R1 inside G0/0 | `192.168.10.1/24` |
| R1 outside G0/1 | `203.0.113.2/30` |
| ISP G0/0 | `203.0.113.1/30` |
| ISP G0/1 | `198.51.100.1/24` |
| PUBLIC-SERVER | `198.51.100.10/24` |
| PC1 lease | `192.168.10.21/24` |
| PC2 lease | `192.168.10.22/24` |

## DHCP configuration

```cisco
ip dhcp excluded-address 192.168.10.1 192.168.10.20
ip dhcp pool OFFICE-LAN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 198.51.100.10
 domain-name aclab.local
```

## NAT/PAT configuration

```cisco
interface gigabitethernet 0/0
 ip nat inside

interface gigabitethernet 0/1
 ip nat outside

access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface gigabitethernet 0/1 overload
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

## Verification

```cisco
show ip dhcp binding
show ip nat translations
show ip nat statistics
```

Observed evidence included both inside-local addresses (`192.168.10.21` and `.22`) translated to the same inside-global address (`203.0.113.2`) with unique ICMP identifiers. NAT statistics recorded dynamic extended translations and translation hits.

## Troubleshooting note

End-to-end traffic initially failed even though every interface was up. Hop-by-hop testing isolated the public-server segment. The server had been assigned `198.151.100.10`; correcting it to `198.51.100.10` restored connectivity.

## Lab file

[Download the Packet Tracer lab](CCNA-Project-04-DHCP-NAT-PAT.pkt)

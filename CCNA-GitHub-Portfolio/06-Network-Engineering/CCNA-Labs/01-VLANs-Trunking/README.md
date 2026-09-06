# Project 1 — VLANs and 802.1Q Trunking

## Objective

Segment four endpoints into ADMIN and SECURITY broadcast domains and extend both VLANs across two switches using an IEEE 802.1Q trunk.

## Topology and addressing

| Endpoint | Switch port | VLAN | Address |
|---|---|---:|---|
| ADMIN-PC1 | SW1 Fa0/1 | 10 | `192.168.10.11/24` |
| SECURITY-PC1 | SW1 Fa0/2 | 20 | `192.168.20.11/24` |
| ADMIN-PC2 | SW2 Fa0/1 | 10 | `192.168.10.12/24` |
| SECURITY-PC2 | SW2 Fa0/2 | 20 | `192.168.20.12/24` |

SW1 `Gi0/1` connects to SW2 `Gi0/1`. VLAN 99 is the native and management VLAN. No default gateway is configured because this is a Layer 2 isolation lab.

## Key configuration

```cisco
vlan 10
 name ADMIN
vlan 20
 name SECURITY
vlan 99
 name MANAGEMENT

interface fastethernet 0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast

interface fastethernet 0/2
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast

interface gigabitethernet 0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
```

The configuration is mirrored on SW2 with endpoint-specific descriptions.

## Verification

```cisco
show vlan brief
show interfaces trunk
```

| Test | Result |
|---|---|
| ADMIN-PC1 → ADMIN-PC2 | Pass |
| SECURITY-PC1 → SECURITY-PC2 | Pass |
| ADMIN-PC1 → SECURITY-PC1 | Fail as designed |

## Troubleshooting note

While one side was configured as a native-VLAN-99 trunk and the other remained a VLAN 1 access port, STP reported a PVID inconsistency and temporarily blocked forwarding. Matching both trunk configurations allowed normal convergence without disabling STP.

## Lab file

[Download the Packet Tracer lab](CCNA-Project-01-VLANs-Trunking.pkt)

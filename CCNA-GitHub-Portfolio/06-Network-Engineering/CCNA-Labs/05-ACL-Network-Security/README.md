# Project 5 — ACL Network Security

## Objective

Apply a named extended ACL close to the traffic source to deny one DHCP client’s ICMP and HTTP access to a public server while permitting all other IP traffic.

## Security policy

| Source | Destination/service | Action |
|---|---|---|
| PC2 `192.168.10.22` | PUBLIC-SERVER ICMP | Deny |
| PC2 `192.168.10.22` | PUBLIC-SERVER TCP/80 | Deny |
| Any | Any other IP traffic | Permit |

## ACL configuration

```cisco
ip access-list extended INSIDE_SECURITY
 remark Block PC2 ICMP and HTTP access to public server
 deny icmp host 192.168.10.22 host 198.51.100.10
 deny tcp host 192.168.10.22 host 198.51.100.10 eq 80
 permit ip any any

interface gigabitethernet 0/0
 ip access-group INSIDE_SECURITY in
```

The ACL is inbound on R1’s inside interface, placing an extended ACL close to its source. The explicit final permit prevents the implicit deny from blocking unrelated traffic.

## Verification evidence

```text
deny icmp host 192.168.10.22 host 198.51.100.10 (4 matches)
deny tcp host 192.168.10.22 host 198.51.100.10 eq www (51 matches)
permit ip any any (9 matches)
```

```cisco
show access-lists INSIDE_SECURITY
show ip interface gigabitethernet 0/0
```

The interface output confirmed `INSIDE_SECURITY` as the inbound access list. In production, the device-specific rule would be paired with a DHCP reservation or other stable identity mechanism.

## Lab file

[Download the Packet Tracer lab](CCNA-Project-05-ACL-Network-Security.pkt)

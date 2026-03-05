# NAT

NAT is used for two things in this lab:
1) outbound Internet access for internal networks (SNAT/PAT)
2) publishing a DMZ web service to the WAN (DNAT/port-forward)

## Outbound NAT (SNAT / PAT)

Internal subnets are translated to the pfSense WAN address for Internet access:
- LAN: `192.168.10.0/24`
- DMZ: `192.168.20.0/24`
- OpenVPN: `10.8.0.0/24`

pfSense performs PAT using high source ports (1024–65535) on the WAN interface.

## Port forward (DNAT)

A single public service is exposed:

- **WAN TCP/80** on pfSense (`10.0.2.3:80`)
  → forwarded to **DMZ nginx** (`192.168.20.100:80`)

This is validated using:
- `curl http://10.0.2.3` from the WAN-side host
- packet capture/Wireshark (SYN/SYN-ACK + HTTP response)
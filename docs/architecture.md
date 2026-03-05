# Architecture

This lab simulates a realistic edge gateway in VirtualBox using pfSense as the firewall/router between WAN, LAN and a DMZ. A remote-access VPN (OpenVPN) is also configured to validate secure connectivity into the LAN.

## Virtual machines
- **pfSense-edge**: gateway/firewall (WAN/LAN/DMZ + OpenVPN)
- **LAN-client (Debian)**: internal client used to validate LAN egress rules
- **DMZ-server (Debian + nginx)**: web service published to WAN via port-forward
- **attacker-wan (Debian)**: WAN-side testing box (nmap/tcpdump) and OpenVPN client

## Networks and interfaces (pfSense)
- **WAN (em0)**: `10.0.2.3/24` (VirtualBox gateway: `10.0.2.1`)
- **LAN (em1)**: `192.168.10.1/24`
- **DMZ (em2)**: `192.168.20.1/24`
- **OpenVPN (ovpns1)**: `10.8.0.1/24` (tunnel network)

## IP plan (main hosts)
- pfSense WAN: `10.0.2.3`
- pfSense LAN: `192.168.10.1`
- pfSense DMZ: `192.168.20.1`
- LAN-client: `192.168.10.101`
- DMZ nginx: `192.168.20.100`
- attacker-wan: `10.0.2.5`
- OpenVPN client: `10.8.0.2`

## Exposed service
- **WAN TCP/80 → DMZ nginx (192.168.20.100:80)** via DNAT/port-forward.

## VPN
- **OpenVPN (UDP/1194)**
- Tunnel network: **10.8.0.0/24**
- Goal: validate controlled access from VPN clients to LAN services (ICMP/SSH/HTTP/HTTPS).
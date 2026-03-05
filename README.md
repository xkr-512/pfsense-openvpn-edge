# pfSense OpenVPN Edge Lab

A complete “edge” lab built in VirtualBox: pfSense as firewall/router with WAN/LAN/DMZ segmentation, outbound NAT, WAN→DMZ port-forward, remote-access OpenVPN, and security validation using nmap + packet captures (tcpdump/Wireshark).

## What is implemented
- **Zones / interfaces**: WAN, LAN, DMZ + OpenVPN tunnel interface
- **Firewall** (deny-by-default):
  - LAN → WAN minimal egress (DNS/HTTP/HTTPS/NTP/ICMP)
  - DMZ → LAN blocked
  - WAN exposure limited to OpenVPN and the published web service
  - OpenVPN → LAN restricted to ICMP/SSH/HTTP/HTTPS
- **NAT**
  - Outbound SNAT/PAT for LAN/DMZ/OpenVPN
  - DNAT/Port-forward: WAN TCP/80 → DMZ nginx (192.168.20.100:80)
- **OpenVPN Remote Access**
  - UDP/1194, tunnel network 10.8.0.0/24 (client example: 10.8.0.2)
- **Validation**
  - nmap open/closed/filtered
  - packet-level proof in Wireshark (SYN-ACK / RST / retransmissions)

## Repository structure
- `docs/` : configuration notes (architecture, firewall rules, NAT, OpenVPN, recon)
- `analysis/` : Wireshark filters and interpretation
- `pcaps/` : packet captures used as proof
- `evidence/nmap/` : nmap outputs (`-oA` → .nmap/.gnmap/.xml)
- `evidence/pfsense/` : pfSense exports (raw + cleaned versions)
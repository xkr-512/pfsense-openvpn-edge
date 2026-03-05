# Firewall rules

This lab follows a deny-by-default approach. Rules are kept minimal and explicit for each zone (WAN/LAN/DMZ/OpenVPN). Validation is done using pfSense logs/states and nmap/pcaps.

## Global policy
- Default: **block (drop) + log**
- Only required traffic is allowed per interface.

## WAN (edge exposure)
Allowed:
- **UDP/1194 → pfSense WAN** (OpenVPN server)
- **TCP/80 → DNAT to DMZ nginx** (published web service)

Explicit test rules (security validation):
- **TCP/81 → REJECT** (expected nmap: `closed`)
- **TCP/4444 → DROP** (expected nmap: `filtered`)

Not allowed:
- Any direct **WAN → LAN** access

## LAN (minimal egress)
Allowed (LAN → WAN / pfSense):
- DNS to pfSense resolver: **TCP/UDP 53 → 192.168.10.1**
- Web browsing: **TCP 80/443**
- Time sync: **UDP 123 (NTP)**
- ICMP (optional, used for troubleshooting)

Blocked by default:
- Anything else (example proof: outbound TCP/22 test results in timeout + pfSense log entry)

## DMZ (restricted egress + isolation)
Allowed:
- DNS to pfSense: **TCP/UDP 53 → 192.168.20.1**
- Web updates: **TCP 80/443 → WAN**
- ICMP to pfSense DMZ IP (optional)

Blocked:
- **DMZ → LAN** is blocked (proof: DMZ ping/traffic to `192.168.10.0/24` fails and is logged)

## OpenVPN (remote access)
Allowed (VPN clients → LAN):
- ICMP (ping) to validate reachability
- SSH (22) for admin access (lab)
- HTTP/HTTPS (80/443) for service access

Not allowed:
- Broad “allow any” rules (VPN access is restricted to required ports only)
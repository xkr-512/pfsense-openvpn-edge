# OpenVPN (remote access)

Goal: provide remote access into the LAN through a dedicated tunnel network, with firewall rules restricted to required services.

## Server settings (pfSense)
- Protocol: **UDP**
- Port: **1194**
- Tunnel network: **10.8.0.0/24**
- Server tunnel IP: **10.8.0.1**
- Authentication: **Local Database** (user + password) with TLS certificates (CA + server cert)

## Certificates
- Internal CA created on pfSense (Lab CA)
- Server certificate created as a **Server Certificate**
- User certificate created as a **User Certificate** for the VPN user

## Firewall rules
WAN:
- Allow **UDP/1194** to pfSense WAN address

OpenVPN interface:
- Allow **ICMP** to LAN (reachability tests)
- Allow **TCP/22** to LAN (SSH admin in lab)
- Allow **TCP/80, TCP/443** to LAN (web access)
- No broad “allow any” rule

## Validation
From the WAN-side client:
- Successful connection assigns a VPN address (example: **10.8.0.2**)
- LAN reachability through the tunnel:
  - `ping 192.168.10.1`
  - `ping 192.168.10.101`

Evidence is included in `evidence/pfsense/openvpn_tail*.txt`.
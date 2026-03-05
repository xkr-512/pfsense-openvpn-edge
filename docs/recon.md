# Recon / validation (nmap)

Goal: validate exposure on the WAN side and demonstrate the difference between open / closed / filtered states.

## Host discovery
Used to confirm WAN-side visibility of the target:
- `nmap -sn 10.0.2.0/24`

## Port states (SYN scan)
All scans are exported with `-oA` (see `evidence/nmap/`).

### Open port (expected: SYN-ACK)
- `nmap -Pn -sS -p 80 10.0.2.3 -oA evidence/nmap/wan_open_80`

Expected result:
- `80/tcp open`

Reason:
- WAN TCP/80 is forwarded to the DMZ nginx server.

### Closed port (expected: RST)
- `nmap -Pn -sS -p 81 10.0.2.3 -oA evidence/nmap/wan_closed_81`

Expected result:
- `81/tcp closed`

Reason:
- WAN TCP/81 is explicitly **REJECTed**, causing an active RST response.

### Filtered port (expected: no response / retransmissions)
- `nmap -Pn -sS -p 4444 10.0.2.3 -oA evidence/nmap/wan_filtered_4444`

Expected result:
- `4444/tcp filtered`

Reason:
- WAN TCP/4444 is explicitly **DROPped**, resulting in no reply and SYN retransmissions.

## Service detection (optional)
- `nmap -Pn -sV -p 80 10.0.2.3`

Used to fingerprint the published HTTP service.
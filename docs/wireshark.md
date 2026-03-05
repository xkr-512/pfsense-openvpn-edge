# Wireshark / packet proof

This section explains how the packet captures demonstrate the three classic nmap states: open, closed, and filtered.

PCAP files are stored in `pcaps/`.

## OPEN (TCP/80)
File: `pcaps/nmap_wan_to_dmz_open.pcap`

Filter:
- `tcp.port == 80`

What to look for:
- Client sends `SYN` to the target
- Target replies with `SYN, ACK` (**proof: open port**)
- Optional: HTTP request/response confirms application-level access (e.g., `HTTP/1.1 200 OK`)

## CLOSED (TCP/81)
File: `pcaps/nmap_81_closed.pcap`

Filter:
- `tcp.port == 81`

What to look for:
- Client sends `SYN`
- Target replies with `RST` or `RST, ACK` (**proof: closed port**)

## FILTERED (TCP/4444)
File: `pcaps/nmap_4444_filtered.pcap`

Filter:
- `tcp.port == 4444`

What to look for:
- Client sends `SYN`
- No reply from the target
- SYN retransmissions appear (**proof: filtered/drop**)
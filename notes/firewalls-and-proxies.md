# Firewalls, ACLs and Proxy Servers

## The task

Build or repair an access control list: order the rules, fill in source, destination, port, protocol and action, and explain why a given rule set does not do what its author intended. Rule *order* is what these questions are actually about.

![Firewall rule evaluation](../diagrams/firewall-acl.svg)

## How an ACL is evaluated

Rules are read **top to bottom, first match wins, and evaluation stops there**. Every consequence follows from that sentence.

A permissive rule above a restrictive one makes the restrictive rule unreachable. `permit any any` at line 10 means lines 11 onward never execute. The specific rule must sit above the general rule, always.

Most firewalls end with an **implicit deny all**. It is invisible in the configuration but it is there, and it means anything not explicitly permitted is dropped. Many administrators add an *explicit* deny-all as the final rule anyway — not to change behaviour but to get logging, since an implicit rule silently discards without a log entry.

## The fields

**Source and destination** — a host, a subnet in CIDR, a named object, or `any`. Get the direction right: for inbound web traffic, the source is the internet and the destination is the server. **Protocol** — TCP, UDP, ICMP, or a protocol number. **Port** — almost always the *destination* port, the service being reached; source ports are ephemeral and high-numbered. **Action** — permit, deny, or in some vendors reject. Deny drops silently; reject sends an RST or ICMP unreachable, which is faster for legitimate clients and more informative for scanners.

## Ports worth knowing cold

| Port | Service | | Port | Service |
|---|---|---|---|---|
| 20/21 | FTP | | 445 | SMB |
| 22 | SSH / SFTP / SCP | | 465 / 587 | SMTPS / submission |
| 23 | Telnet | | 636 | LDAPS |
| 25 | SMTP | | 993 | IMAPS |
| 53 | DNS (UDP and TCP) | | 995 | POP3S |
| 67/68 | DHCP | | 1433 | MS SQL |
| 69 | TFTP | | 1521 | Oracle |
| 80 | HTTP | | 3306 | MySQL |
| 110 | POP3 | | 3389 | RDP |
| 123 | NTP | | 5432 | PostgreSQL |
| 143 | IMAP | | 5900 | VNC |
| 161/162 | SNMP / trap | | 8080 | HTTP alt / proxy |
| 389 | LDAP | | 8443 | HTTPS alt |

DNS uses UDP 53 for ordinary queries and TCP 53 for zone transfers and responses over 512 bytes. A rule that only permits UDP 53 will break in ways that look intermittent.

## Firewall generations

**Packet filtering** works on headers alone — addresses, ports, flags — and is stateless, so it cannot tell a reply from an unsolicited packet. **Stateful inspection** keeps a connection table and permits return traffic for sessions it saw start; this is the baseline expectation. **Next-generation** adds application awareness, user identity from a directory, and integrated IPS, so it can permit a specific application rather than a port. **WAF** sits in front of web applications specifically and inspects HTTP semantics — it is the answer for SQL injection and XSS, and it is the wrong answer for a network-layer flood.

## Proxies

A **forward proxy** sits between internal clients and the internet: content filtering, URL categorisation, caching, and a single logged egress point. A **reverse proxy** sits in front of servers: TLS termination, load balancing, hiding the origin, and often the WAF's home. Which side the proxy faces determines which one the scenario needs, and the word "proxy" alone is never the full answer.

## Traps

Putting the specific rule below the general one is the classic broken ACL, and it is what the PBQ is checking. Forgetting the return-traffic implication on a stateless filter. Permitting TCP 53 only, or UDP 53 only. Confusing source and destination on inbound rules. Choosing a WAF for a volumetric DDoS, or a network firewall for SQL injection. Leaving `permit any any` in place "temporarily".

## Self-check

- Order these correctly: permit HTTPS from any to 10.0.1.10; deny all from 203.0.113.0/24; permit all internal to any. Explain the ordering.
- Why does an explicit deny-all get added when an implicit one already exists?
- Which two ports does DNS need, and what breaks if you allow only one?
- Forward or reverse proxy: TLS offload for a public web farm? Blocking employee access to gambling sites?

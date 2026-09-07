# Security+ SY0-701 — PBQ Concept Notes

Performance-based questions are the part of the Security+ exam that asks you to *do* something rather than recognise a definition: build a firewall ACL, pick a RAID level, match an authentication protocol to a scenario, read `nmap` output. This repository is my own working notes on the concepts each PBQ topic exercises, written while working through the PBQ section of a SY0-701 prep course.

**What this is not:** the questions, the answer keys, or any screenshots of the exam interface. Those belong to CompTIA and to the course vendor. What is here is my own explanation of the underlying material, structured so I can test recall against it. If you want the questions, buy the course.

## How each note is structured

Every file follows the same shape, because the shape is the study method:

**The task** — what the PBQ actually asks you to produce, in general terms. **The concepts** — what you have to know cold to produce it. **Reference tables** — the values worth memorising, since PBQs are usually decided by a number or a port you either know or do not. **Traps** — the specific ways I got it wrong. **Self-check** — questions with no answers written down, so re-reading is not the same as knowing.

## Notes

| Topic | Note |
|---|---|
| Security controls — types and application | [security-controls.md](notes/security-controls.md) |
| VPNs and remote access protocols | [vpns-remote-access.md](notes/vpns-remote-access.md) |
| Knowledge-based authentication | [knowledge-based-authentication.md](notes/knowledge-based-authentication.md) |
| Social engineering | [social-engineering.md](notes/social-engineering.md) |
| Redundancy strategies and RAID | [redundancy-and-raid.md](notes/redundancy-and-raid.md) |
| Nmap and tcpdump | [nmap-and-tcpdump.md](notes/nmap-and-tcpdump.md) |
| Firewalls, ACLs and proxy servers | [firewalls-and-proxies.md](notes/firewalls-and-proxies.md) |
| Switching, routing and segmentation | [switching-and-routing.md](notes/switching-and-routing.md) |
| Cryptography | [cryptography.md](notes/cryptography.md) |
| Cloud security | [cloud-security.md](notes/cloud-security.md) |
| Mobile devices and macOS | [mobile-and-macos.md](notes/mobile-and-macos.md) |
| Indicator correlation and incident response | [ioc-and-incident-response.md](notes/ioc-and-incident-response.md) |

## Diagrams

Hand-built SVGs, drawn to be read at a glance rather than to be pretty. They live in [`diagrams/`](diagrams/) and are embedded in the notes that use them.

## Related

Lab work, MITRE ATT&CK detection writeups and full domain notes are in my main repo: [cybersecurity_Lab](https://github.com/N1ghtsh1ft1/cybersecurity_Lab)

## Sources

Topic list follows the PBQ section of Cyberkraft's CompTIA Security+ SY0-701 course and the published CompTIA SY0-701 exam objectives. Protocol behaviour, port numbers and RAID characteristics are checked against vendor documentation and RFCs. All prose is my own.

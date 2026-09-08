# Nmap and tcpdump

## The task

Read command output and say what it shows, or build a command that produces a stated result. Both halves matter — recognising `-sS` in an answer choice is not the same as knowing what the output means.

> Run these only against hosts you own or have written authorisation to test. Everything below was run in an isolated lab.

## Nmap — scan types

| Flag | Scan | Behaviour |
|---|---|---|
| `-sS` | SYN / half-open | Sends SYN, reads the reply, never completes the handshake. Default when running as root. |
| `-sT` | TCP connect | Full three-way handshake. Used without raw-socket privileges; noisier in logs. |
| `-sU` | UDP | Slow — no handshake to confirm against, so it leans on ICMP unreachable and timeouts. |
| `-sn` | Ping sweep | Host discovery only, no port scan. |
| `-Pn` | Skip discovery | Treat every host as up. Use when ICMP is filtered. |
| `-sV` | Version detection | Banner and probe based service fingerprinting. |
| `-O` | OS detection | TCP/IP stack fingerprinting. Needs open and closed ports to be accurate. |
| `-A` | Aggressive | `-sV -O` plus scripts and traceroute. Loud. |
| `-p` | Ports | `-p 22,80,443`, `-p 1-1024`, `-p-` for all 65535. |
| `-T0`–`-T5` | Timing | `-T4` is the usual working speed; `-T0`/`-T1` are for evasion. |
| `-oN/-oX/-oG` | Output | Normal, XML, greppable. |
| `--script` | NSE | `--script vuln`, `--script smb-enum-shares`. |

## Reading port states

**open** — something accepted the connection. **closed** — the host replied but nothing is listening; the host is up. **filtered** — no reply, or an ICMP administratively-prohibited; a firewall is in the way and you cannot tell what is behind it. **open|filtered** — no reply and no way to distinguish, which is the normal UDP result.

The difference between *closed* and *filtered* is the single most useful thing in nmap output: closed proves the host is reachable, filtered proves a control is between you and it.

## tcpdump

| Flag | Effect |
|---|---|
| `-i eth0` | Interface; `-i any` for all |
| `-n` / `-nn` | Do not resolve names / nor ports |
| `-c 100` | Stop after 100 packets |
| `-w cap.pcap` | Write raw capture to file |
| `-r cap.pcap` | Read a capture back |
| `-v` `-vv` | Verbosity |
| `-A` | Print payload as ASCII |
| `-X` | Print payload as hex and ASCII |
| `-s 0` | Full packet, no snap length truncation |

Filters compose with `and`, `or`, `not`:

    tcpdump -i eth0 -nn host 10.0.0.5 and port 443
    tcpdump -i eth0 -nn 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack == 0'
    tcpdump -i eth0 -nn -w evidence.pcap net 10.0.0.0/24 and not port 22

The second one shows SYN packets with no ACK — connection attempts. A burst of those from one source across many destination ports is a port scan, and that is the pattern the PBQ usually wants named.

Always use `-n` when reading output under time pressure. Name resolution reorders and delays the display, and it also generates DNS traffic from the capturing host, which contaminates the capture.

## Why capture files matter

`-w` gives you something you can hand to an analyst or attach to a ticket. Live terminal output is gone when the buffer scrolls. For anything that might become evidence, write to a file, hash the file, and record when and where it was taken.

## Traps

`-sn` is discovery only and returns no ports; if the question wants services, it is wrong. `-Pn` does not make a scan stealthy, it makes it *thorough* against hosts that ignore ping. `-sS` is not undetectable — modern firewalls and IDS log half-open scans readily; "stealth" is a historical name. UDP scans returning `open|filtered` en masse is normal behaviour, not a broken scan. `-A` is the wrong answer any time the scenario mentions avoiding detection.

## Self-check

- One command: SYN scan ports 1–1024 on 10.10.0.0/24, skip host discovery, detect versions, save greppable output.
- What does *filtered* tell you that *closed* does not?
- Write a tcpdump filter capturing only traffic to or from 192.168.50.10 on port 445, without name resolution, into a file.
- You see 200 SYNs from one host to 200 different ports on one target in four seconds. Name it and give the tcpdump filter that isolates it.

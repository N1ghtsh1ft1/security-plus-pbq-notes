# VPNs and Remote Access Protocols

## The task

Match a remote-access requirement to the right protocol and the right VPN configuration, and know which ports and modes go with each. These questions turn on precise details — the difference between transport and tunnel mode, or between a split tunnel and a full tunnel, decides the answer.

![VPN modes](../diagrams/vpn-modes.svg)

## Tunnel shapes

**Full tunnel** sends all client traffic through the corporate gateway, including traffic bound for the public internet. Every packet is inspected and logged by corporate controls. Costs bandwidth and adds latency to ordinary browsing.

**Split tunnel** sends only corporate-destined traffic through the VPN; everything else exits the client's local internet connection directly. Faster and cheaper, but the endpoint now touches the internet outside corporate inspection while simultaneously connected to the internal network — which is why security teams push back on it.

**Site-to-site** joins two networks permanently through gateway devices. Users never run client software; the tunnel is always up. Compare to **remote access**, which is per-user and on demand.

**Always-on** brings the tunnel up before or immediately after user logon, so the device is never on a hostile network unprotected.

## IPsec

IPsec is a suite, not one protocol. Know the pieces.

| Component | Role |
|---|---|
| IKE (UDP 500) | Negotiates the security association and key material |
| IKEv2 / NAT-T (UDP 4500) | Encapsulates IPsec in UDP so it survives NAT |
| AH (protocol 51) | Authentication and integrity. **No confidentiality — AH does not encrypt.** |
| ESP (protocol 50) | Encryption plus authentication. This is what you use. |

**Transport mode** encrypts the payload but leaves the original IP header in place. Host-to-host, on a network where the endpoints are the communicating parties.

**Tunnel mode** encrypts the entire original packet and wraps it in a new IP header. Gateway-to-gateway, which is why site-to-site VPNs use it — the internal addresses are hidden inside the encrypted payload.

## Protocol reference

| Protocol | Port | Notes |
|---|---|---|
| SSH | TCP 22 | Also the transport for SFTP and SCP |
| Telnet | TCP 23 | Cleartext. Never the right answer. |
| TLS / HTTPS VPN | TCP 443 | Traverses restrictive firewalls because 443 is rarely blocked |
| L2TP | UDP 1701 | No encryption by itself — always paired as L2TP/IPsec |
| PPTP | TCP 1723 | Deprecated, broken MS-CHAPv2 authentication |
| RDP | TCP 3389 | Never expose directly to the internet; front it with a VPN or gateway |
| RADIUS | UDP 1812 auth / 1813 accounting | Encrypts only the password field |
| TACACS+ | TCP 49 | Encrypts the entire payload; separates authn from authz |

## Traps

AH versus ESP is the single most reliable trap in this topic: if the scenario says confidentiality is required, AH is wrong. L2TP alone provides no encryption. RADIUS leaves everything but the password in cleartext, which is why TACACS+ wins when the question emphasises protecting command authorisation. A split tunnel is not "more secure because less traffic crosses the VPN" — it is less secure, and the exam tests that direction.

## Self-check

- A site-to-site VPN must hide internal RFC 1918 addressing from the transit network. Which IPsec mode, and why does the other one fail?
- Why does IPsec need UDP 4500 when UDP 500 already exists?
- Name the one thing TACACS+ does that RADIUS does not.
- A remote user reports that VPN connects but internal DNS names do not resolve while public sites work fine. What tunnel configuration is in play?

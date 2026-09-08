# Social Engineering

## The task

Read a scenario or a set of message excerpts and name the specific attack, then pick the control that addresses it. The distinctions are narrow and the exam rewards precision — "phishing" is rarely the intended answer when a more specific term fits.

## Naming the attack

| Attack | The distinguishing detail |
|---|---|
| Phishing | Broad, untargeted email |
| Spear phishing | Targeted at a specific person or team, using researched detail |
| Whaling | Spear phishing aimed at an executive |
| Business email compromise | A real or spoofed internal account used to authorise a payment or data transfer |
| Vishing | Voice call |
| Smishing | SMS |
| SPIM | Instant messaging or chat platform |
| Pharming | DNS or hosts-file poisoning sends correct URLs to an attacker site |
| Typosquatting | Registering a misspelled lookalike domain |
| Watering hole | Compromising a legitimate site the target group already visits |
| Pretexting | An invented scenario that justifies the request |
| Impersonation | Claiming to be a specific person or role |
| Tailgating | Following someone through a controlled door, without their knowledge |
| Piggybacking | The same, but with their consent |
| Shoulder surfing | Observing screens or keypads |
| Dumpster diving | Recovering discarded documents or media |
| Eliciting information | Conversational extraction with no explicit ask |
| Hoax | False warning that provokes a harmful "fix" |
| Invoice scam | Fraudulent bill sent to accounts payable |
| Credential harvesting | A lookalike portal that captures a login |

## The principles being exploited

Six levers show up repeatedly, and identifying the lever is often what the question is really testing. **Authority** — the request appears to come from someone who can compel it. **Urgency** — a deadline that removes time to verify. **Intimidation** — consequences for refusing. **Social proof** — everyone else already complied. **Scarcity** — limited availability. **Familiarity or trust** — an established relationship, real or manufactured.

Urgency plus authority together is the signature of BEC, and it is the combination worth flagging on sight.

## Controls, mapped

Technical controls narrow the delivery path: SPF, DKIM and DMARC to make spoofing your own domain harder; an email gateway with attachment sandboxing and URL rewriting; DNSSEC against pharming; MFA so harvested credentials alone are insufficient; and defensive registration of common typo domains.

Procedural controls address the decision: out-of-band verification for any payment or banking-detail change — calling a number you already had, never one in the message; dual authorisation over a threshold; a clean-desk and shredding policy; and access control vestibules with badge-in badge-out to make tailgating physically awkward.

Awareness controls address the person: simulated phishing with a reporting button that is easier to press than the delete key, and a culture in which reporting a mistake fast is rewarded rather than punished. That last part is a control, not a nicety — the cost of a compromise is set by how long it stays quiet.

## Traps

Tailgating versus piggybacking is consent. Pharming versus typosquatting is whether the user typed the address correctly — in pharming they did. Whaling is about the target's seniority, not the amount of money. A hoax is social engineering even though no credentials are stolen, because the payload is the victim's own remediation action.

## Self-check

- An accounts-payable clerk gets an email from the CFO's real address asking to change a supplier's bank details before end of day. Name the attack, the two principles, and the one control that stops it regardless of how convincing the email is.
- A user types the bank URL correctly and lands on a fake site. What happened, and where?
- Why does an anti-phishing program that punishes simulation failures make an organisation less safe?

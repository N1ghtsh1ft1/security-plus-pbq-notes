# Knowledge-Based Authentication

## The task

Configure or evaluate an authentication scheme, decide whether it actually constitutes multi-factor, and identify why a knowledge-based control is weak.

## The factors

| Factor | Meaning | Examples |
|---|---|---|
| Something you **know** | Knowledge | Password, PIN, security question, passphrase |
| Something you **have** | Possession | Hardware token, smart card, phone with an authenticator app, certificate |
| Something you **are** | Inherence | Fingerprint, face, iris, voice |
| Somewhere you **are** | Location | GPS, network segment, geofence |
| Something you **do** | Behaviour | Typing cadence, gait, signature dynamics |

**Multi-factor means factors from different categories.** A password plus a security question is single-factor — both are knowledge. This is the classic PBQ trap and it appears in some form on nearly every attempt.

## Static versus dynamic KBA

**Static KBA** uses answers the user registered in advance: mother's maiden name, first pet, high school. The weakness is structural — the answers are often public, guessable, or already breached, and they never change. A user cannot rotate their mother's maiden name after a leak.

**Dynamic KBA** generates questions at the moment of authentication from data the provider holds — "which of these four addresses have you lived at?" — drawn from credit or public records. Harder to prepare for, but it depends on a third-party data broker and it fails for people with thin records.

## Why KBA keeps failing

Answers leak in breaches and are reused across sites. Social media makes many static answers trivially researchable. Users invent memorable answers that are also guessable. And crucially, KBA is frequently the *account recovery* path — so an attacker who cannot beat MFA at the front door beats KBA at the side door instead. Any assessment of an authentication design has to look at the reset flow, not just the login flow.

## Better options and what they cost

| Control | Strength | Weakness |
|---|---|---|
| SMS OTP | Better than nothing; possession-ish | SIM swap, SS7 interception |
| TOTP app | Possession, offline, cheap | Phishable in real time; device loss |
| Push approval | Usable, possession | MFA fatigue / push bombing |
| Number matching push | Kills push bombing | Slightly more friction |
| FIDO2 / WebAuthn hardware key | Phishing-resistant — origin bound | Cost, enrolment, recovery |
| Smart card / PIV | Strong, possession + PIN | Infrastructure heavy |

FIDO2 is phishing-resistant because the credential is cryptographically bound to the origin, so a lookalike domain cannot elicit a usable response. That property is what makes it categorically different from OTP, not just stronger.

## Traps

Password plus PIN is one factor. Password plus security question is one factor. A password and a fingerprint is two. Location and behaviour are recognised factors but are usually used to *adjust risk*, not to stand alone. Requiring longer or more complex passwords does not convert knowledge into a second factor.

## Self-check

- A helpdesk resets passwords after verifying the last four of an employee ID and a birth date. What is wrong with calling this two-factor?
- Why is FIDO2 phishing-resistant in a way TOTP is not?
- Name the attack that number matching is designed to defeat.
- Where is KBA most dangerous in a well-designed MFA deployment, and why?

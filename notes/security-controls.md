# Security Controls — Types and Application

## The task

You are given a set of scenarios or a set of named controls and asked to place each one into a category and a function. It is a two-axis classification, and the exam expects both axes, not one.

## The two axes

**Category** answers *what kind of thing is it?* — Technical (implemented in hardware or software), Managerial (a decision or process about risk), Operational (carried out by people day to day), Physical (something you can touch).

**Function** answers *what does it do relative to the incident?* — Preventive (stops it happening), Deterrent (discourages the attempt), Detective (notices it happened), Corrective (fixes it after), Compensating (stands in for a control you could not implement), Directive (tells people what to do).

The same control can appear in different functions depending on how it is described. A camera you can see from the parking lot is deterrent. The same camera reviewed after a break-in is detective. Read the scenario, not the noun.

## Worked classifications

| Control | Category | Function |
|---|---|---|
| Firewall rule blocking inbound SMB | Technical | Preventive |
| Security guard at the lobby desk | Physical / Operational | Deterrent + Preventive |
| Visible camera signage | Physical | Deterrent |
| SIEM alert on failed logons | Technical | Detective |
| Backup restore after ransomware | Technical | Corrective |
| Acceptable use policy | Managerial | Directive |
| Onboarding security awareness training | Operational | Preventive |
| MFA where a legacy app cannot support it, replaced by IP allow-listing | Technical | Compensating |
| Bollards outside a datacenter entrance | Physical | Preventive |
| Quarterly access review | Managerial | Detective |
| Incident response plan | Managerial | Corrective |
| Mantrap / access control vestibule | Physical | Preventive |

## Traps

Training is not technical just because it is about computers — it is operational, because people execute it. Policies are managerial even when they describe technical requirements; the policy is the decision, the firewall is the implementation. Compensating is not a synonym for "backup plan" — it specifically means an alternative control standing in for a required one that could not be implemented, and it must reduce the same risk. Deterrent controls work on the attacker's decision to try; preventive controls work on their ability to succeed. A lock is preventive. A sign saying the door is alarmed is deterrent.

## Self-check

- Give one control that is simultaneously deterrent and preventive, and say why it is both.
- A legacy SCADA host cannot run an EDR agent, so it is placed on an isolated VLAN with strict ACLs. Category and function?
- Why is an incident response *plan* corrective when the plan itself does not fix anything?
- Classify: log retention policy, log aggregation server, analyst reviewing logs. Three different answers.

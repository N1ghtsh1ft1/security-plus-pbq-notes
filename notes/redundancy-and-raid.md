# Redundancy Strategies and RAID

## The task

Given a requirement — capacity, fault tolerance, read or write performance, a disk budget — pick the RAID level and compute usable capacity and failure tolerance. These are arithmetic questions and they are free marks if the table is memorised.

![RAID levels](../diagrams/raid-levels.svg)

## RAID reference

| Level | Min disks | Usable capacity | Survives | Notes |
|---|---|---|---|---|
| 0 | 2 | N | **0 disks** | Striping only. Pure performance, negative reliability. |
| 1 | 2 | N/2 | 1 per mirror pair | Mirroring. Fast reads, write penalty of 2. |
| 5 | 3 | N−1 | 1 disk | Striping with distributed parity. Write penalty of 4. |
| 6 | 4 | N−2 | 2 disks | Dual distributed parity. Write penalty of 6. |
| 10 (1+0) | 4 | N/2 | 1 per mirror set | Mirror then stripe. Best all-round for databases. |

*N* is the number of disks; capacity assumes equal disk sizes.

**Worked example.** Six 4 TB disks. RAID 5 gives (6−1) × 4 = 20 TB and survives one failure. RAID 6 gives (6−2) × 4 = 16 TB and survives two. RAID 10 gives 6/2 × 4 = 12 TB and survives one failure per mirror pair — up to three, but only if the failures land in different pairs, which is not something you can promise.

**Why RAID 6 exists.** Rebuilding a large RAID 5 array hammers every remaining disk for hours, exactly when those disks are the same age and model as the one that just died. A second failure during rebuild loses everything. RAID 6 buys survivability through the rebuild window. That reasoning, not the parity maths, is usually what the scenario is testing.

## RAID is not backup

RAID protects against **disk failure**. It does not protect against deletion, ransomware, corruption, fire, or theft — every one of those is faithfully mirrored or striped to all members instantly. The exam likes to offer RAID as an answer to a data-loss scenario that RAID cannot address.

## Wider redundancy

Beyond disks, redundancy shows up at every layer, and the scenario tells you which layer is failing. Power: dual supplies, UPS for ride-through, generator for duration, dual utility feeds. Network: NIC teaming, redundant switches and uplinks, multiple ISPs, dynamic routing. Compute: clustering, load balancers, virtualisation with live migration. Geography: multiple sites, replication, geographic dispersal so one regional event cannot take everything.

## Recovery sites

| Site | Ready in | Cost | Data |
|---|---|---|---|
| Hot | Minutes | Highest | Live replicated |
| Warm | Hours to days | Middle | Recent, needs restore |
| Cold | Days to weeks | Lowest | Nothing on site |

Match the site to the **RTO**. If the scenario states an RTO of four hours, cold is out on time and hot may be over-engineered — warm is the fit.

## The two metrics

**RPO — Recovery Point Objective:** how much data you can afford to lose, measured backwards from the incident. It sets backup *frequency*. **RTO — Recovery Time Objective:** how long you can afford to be down, measured forwards. It sets recovery *capability*.

Nightly backups mean an RPO of up to 24 hours no matter how fast the restore is. Also worth knowing: **MTBF** (mean time between failures, for repairable systems), **MTTF** (mean time to failure, for non-repairable), **MTTR** (mean time to repair, which feeds RTO).

## Traps

RAID 0 has no redundancy despite being in the RAID family. RAID 10 needs four disks minimum, not three. Usable capacity questions specify disk size for a reason — read whether they want capacity or disk count. RPO and RTO get swapped constantly; RPO looks backwards at data, RTO looks forwards at time.

## Self-check

- Eight 2 TB disks. Usable capacity and failure tolerance for RAID 5, 6 and 10.
- Explain in one sentence why RAID 6 is preferred over RAID 5 for large-capacity arrays.
- A business states RPO 15 minutes, RTO 8 hours. What does each number constrain?
- Ransomware encrypts a file server on a RAID 6 array. How much did RAID 6 help?

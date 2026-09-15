# IT 520 — Performance and Systems: L05 — Which part to improve

- Course: IT 520 — Performance and Systems
- Lecture: Week 03 / L05 — Which part to improve
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `73d58e30645ad2ab9173fb690adea540ef0b6d621aedc45409fab58ab0384b6d`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What evidence would tell us which part to improve?

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c01`
- Citations: `approved_storyboard`

- Read a server's own logs, name the part that limits it, and tell your boss which fix to buy.

**Citations:** `approved_storyboard`

## Your first week: the branch report is slow and the boss has a quote

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c02`
- Citations: `approved_storyboard`

### Diagram explanation

A report request enters one server, uses the CPU, the disk and the network, and the finished report is delivered.

- Reports requested leads to CPU: the app
- CPU: the app leads to Disk: the orders database
- CPU: the app leads to Network: pages out
- Network: pages out leads to Reports delivered

**Citations:** `approved_storyboard`

- Parts distributor. Branch managers run a stock-and-orders report before the 4 pm shipping cutoff.
- One server: the app runs on the CPU, the orders database lives on the disk, pages go out over the network.
- Every afternoon: the report takes forever.
- The boss forwards a vendor quote for a server with twice the processor speed and asks: do I sign this?

**Citations:** `approved_storyboard`

## The app's own log says how long each report took

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c03`
- Citations: `approved_storyboard`

### Code state 1

the app's log, 2 to 3 pm

```text
user      started  finished  took
jmorales  14:02    14:17     15.3 min   branch report, Concord
kpatel    14:04    14:18     14.1 min   branch report, Manchester
jmorales  14:09    14:25     16.0 min   branch report, Nashua
   ... 9 more lines in 14:00-15:00 ...
reports finished: 12    average time: 15 min
```

**Citations:** `approved_storyboard`

- Busy hour, 2 to 3 pm: 12 reports finished, average 15 minutes.
- Quiet morning, same report: about 6 minutes.
- No stopwatch on anyone. The app wrote this itself.

**Citations:** `approved_storyboard`

## Throughput and latency, read straight from the log

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c04`
- Citations: `approved_storyboard`, `lazowska_ch3`

- **Throughput X**: reports finished per unit time: X = C ÷ T.
- **Latency R**: one report's time, from requested to delivered.

**Citations:** `approved_storyboard`, `lazowska_ch3`

### Chart data

Read from the log: the busy hour

| Measure | Value |
| --- | --- |
| Reports finished in the hour (X) | 12 |
| Average report, minutes (R) | 15 |

Scale 0 to 15

**Citations:** `approved_storyboard`

- From the log: X = 12 reports ÷ 60 min, one every 5 minutes.
- From the log: R = 15 min. Slow means R.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## Little's Law: three reports in flight, nine minutes of waiting

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c05`
- Citations: `approved_storyboard`, `lazowska_ch3`

- **Little's Law**: reports inside at once = throughput × time inside: N = X · R.
- **Waiting**: time inside minus time being worked on.

**Citations:** `approved_storyboard`, `lazowska_ch3`

### Chart data

One report's 15 minutes

| Part of the report | Minutes |
| --- | --- |
| Being worked on | 6 |
| Waiting | 9 |

Scale 0 min to 15 min

**Citations:** `approved_storyboard`

- On this server: N = 0.2 per min × 15 min = 3 reports in flight at once.
- wait = 15 − 6 = 9 min. Most of forever is waiting for something.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## The resource report says how busy each part was

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c06`
- Citations: `approved_storyboard`, `lazowska_ch3`

- **Utilization U**: the share of the hour a part was busy: U = B ÷ T.

**Citations:** `approved_storyboard`, `lazowska_ch3`

### Code state 1

the system's resource report, same hour

```text
window 14:00-15:00  (60 min)
CPU      busy 24 min   40%
disk     busy 48 min   80%
network  busy  1 min    2%
```

**Citations:** `approved_storyboard`

- Second file, same hour. The system counts busy minutes for every part.
- Nothing is at 100%. Busy says where the hour went, not what one report costs.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## Service demand: the busy time shared over the reports

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c07`
- Citations: `approved_storyboard`, `lazowska_ch3`

- **Service demand D**: the minutes of a part that one finished report used: D = B ÷ C.

**Citations:** `approved_storyboard`, `lazowska_ch3`

> Image prompt: Flat vector diagram. A long horizontal strip representing one hour, with the left four fifths shaded as busy and the right fifth left pale as idle. Below it, the shaded busy stretch is drawn again on its own and divided into twelve equal slices. One slice is pulled out beneath and enlarged, with a small bracket under it. No words or numbers anywhere in the image.

**Citations:** `approved_storyboard`

- On this server: disk 48 min busy ÷ 12 reports = 4 min of disk per report.
- This is Monday's service time, once per part. It belongs to the report, not to the afternoon.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## The ledger: what one report costs each part

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c08`
- Citations: `approved_storyboard`, `lazowska_ch3`

### Chart data

What one report costs each part

| Part | Minutes per report |
| --- | --- |
| CPU | 2 |
| Disk | 4 |
| Network | 0.1 |

Scale 0 min to 4 min

**Citations:** `approved_storyboard`, `lazowska_ch3`

- CPU 24 ÷ 12 = 2 min; disk 48 ÷ 12 = 4 min; network a few seconds.
- Check: 2 + 4 = 6 min, the quiet-morning report from the log. The model already predicts something true.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## The bottleneck is the tallest bar

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c09`
- Citations: `approved_storyboard`, `lazowska_ch5`

- **Bottleneck**: the part with the largest service demand.
- **Bound class**: the bottleneck's kind: compute (CPU), memory, or I/O (disk, network).

**Citations:** `approved_storyboard`, `lazowska_ch5`

### Chart data

What one report costs each part, tallest marked

| Part | Minutes per report |
| --- | --- |
| CPU | 2 |
| Disk (tallest) | 4 |
| Network | 0.1 |

Scale 0 min to 4 min

**Citations:** `approved_storyboard`, `lazowska_ch5`

- On this server: the disk, 4 minutes per report. This server is I/O-bound.
- The kind says where the fixes live: at the disk and in how much we read, not at the processor.

**Citations:** `approved_storyboard`, `lazowska_ch5`

## The ceiling: one report per four minutes of disk, fifteen an hour

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c10`
- Citations: `approved_storyboard`, `lazowska_ch5`

- **Throughput ceiling**: the most reports the server can finish per hour: 1 ÷ tallest demand.

**Citations:** `approved_storyboard`, `lazowska_ch5`

> Image prompt: Flat vector diagram. A long horizontal ruler representing one hour, with small tick marks along its top edge. The ruler is filled end to end with fifteen identical rectangular blocks placed side by side with no gaps. Beneath the ruler, a small triangular marker points up at the boundary after the twelfth block. No words or numbers anywhere in the image.

**Citations:** `approved_storyboard`

- On this server: one report per 4 min of disk, so 15 an hour at most.
- That is the Utilization Law at 100%: the disk has 60 minutes an hour to give, and each report takes four.

**Citations:** `approved_storyboard`, `lazowska_ch5`

## Three reports short of the wall is why six minutes becomes fifteen

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c11`
- Citations: `approved_storyboard`, `lazowska_ch5`

### Chart data

Reports per hour

| When | Reports per hour |
| --- | --- |
| Ceiling (1 ÷ 4 min) | 15 |
| This afternoon | 12 |
| Quiet morning | 3 |

Scale 0 /hour to 15 /hour

**Citations:** `approved_storyboard`

- At 12 an hour the disk is 80% busy, three reports short of its wall.
- Near the wall, reports arrive faster than the disk clears them: N grows, and by Little's Law R = N ÷ X grows with it.
- That is the afternoon problem: the disk approaching its ceiling.

**Citations:** `approved_storyboard`, `lazowska_ch5`

## The vendor's faster CPU leaves the ceiling where it was

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c12`
- Citations: `approved_storyboard`, `lazowska_ch5`

### Chart data

Minutes per report, before and after the vendor's CPU

| Part | Minutes per report |
| --- | --- |
| CPU: now | 2 |
| CPU: vendor's faster CPU | 1 |
| Disk: both | 4 |
| Network: both | 0.1 |

Scale 0 min to 4 min

**Citations:** `approved_storyboard`

- The quote: twice the processor speed. CPU 2 → 1 min per report.
- Tallest bar: still the disk, 4 min. Ceiling: still 15 an hour.
- Afternoons still 15-minute reports. A quiet-morning report gets one minute faster, which nobody asked for.

**Citations:** `approved_storyboard`, `lazowska_ch5`

## Halving the disk reads doubles the ceiling, for free

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c13`
- Citations: `approved_storyboard`, `lazowska_ch5`

### Chart data

Minutes per report, before and after caching the order totals

| Part | Minutes per report |
| --- | --- |
| CPU: both | 2 |
| Disk: now | 4 |
| Disk: with the cache | 2 |
| Network: both | 0.1 |

Scale 0 min to 4 min

**Citations:** `approved_storyboard`

- The report re-reads the same order totals every time. Keep them in memory: half the disk reads, nothing bought.
- Disk 4 → 2 min per report. Tallest bar now 2 min. Ceiling 30 an hour.
- A faster disk would do the same for money. Either way, the next tallest bar is the CPU.

**Citations:** `approved_storyboard`, `lazowska_ch5`

## Measure the same hour again: the new log shows the new ledger

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c14`
- Citations: `approved_storyboard`, `lazowska_ch5`

### Code state 1

the same two files, same hour, after the cache

```text
reports finished: 12    average time: 5 min

window 14:00-15:00  (60 min)
CPU      busy 24 min   40%
disk     busy 24 min   40%
network  busy  1 min    2%
```

**Citations:** `approved_storyboard`

- Same hour, same 12 reports: disk 24 ÷ 12 = 2 min, 40% busy, far from its wall.
- Average report 5 minutes; wait = 5 − 4 = 1 min, down from 9.
- If the new log had not moved, the change did not do what we thought.

**Citations:** `approved_storyboard`, `lazowska_ch5`

## The note to the boss

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c15`
- Citations: `approved_storyboard`

### Code state 1

the note

```text
What was slow:   the branch report, 15 min in the afternoon, 6 in the morning
Which part:      the disk, 4 of the 6 minutes; 80% busy at 2 pm, near its limit
The quote:       a faster CPU does not touch the disk; afternoons stay at 15
What I changed:  cached the order totals; disk work halved
Measured after:  same hour, 5-minute reports, disk 40% busy
```

**Citations:** `approved_storyboard`

- Five lines, every one a number from a log.
- The quote is declined with evidence, not opinion.

**Citations:** `approved_storyboard`

## The method: log, cost per report, tallest bar, ceiling, change, measure again

- Source lineage: `it520-fall-2026-week03-l05-which-part#sections.c16`
- Citations: `approved_storyboard`

### Diagram explanation

Turn the complaint into a number, read busy time per part, divide by reports for the cost per report, find the tallest bar and its kind, compute the ceiling, change one thing and measure the same hour again.

- Complaint to a number leads to Busy time per part
- Busy time per part leads to Cost per report D = B ÷ C
- Cost per report D = B ÷ C leads to Tallest bar and its kind
- Tallest bar and its kind leads to Ceiling 1 ÷ D
- Ceiling 1 ÷ D leads to Change one thing, measure again

**Citations:** `approved_storyboard`

- Before anyone buys hardware: two logs, one division per part, one tallest bar.
- The tallest bar names the bottleneck, its kind, and the ceiling.
- A fix counts when the tallest bar shrinks and the next log shows it.

**Citations:** `approved_storyboard`

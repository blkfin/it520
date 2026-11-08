# IT 520: Amdahl bounds

- Course: IT 520 - Foundations of IT
- Lecture: 11 / November 9, 2026
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `86754f030dc32ddc65860a1a5fdc1d59c3fd685f98773f1bf7de23f159aa5397`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What limits the speedup we get from adding workers?

- Source lineage: `lecture#sections.card01`
- Citations: none

By the end, you can identify which steps workers can share, estimate how long the job takes, explain slower runs, and choose a step to improve.

## The sensor job must produce one final count

- Source lineage: `lecture#sections.card02`
- Citations: none

- Worker: one thread running on its own core.
- Assume enough identical cores and evenly divided chunks.
- One file contains sensor readings.
- We need the count above a threshold.
- One worker finishes in 10 seconds.
- More workers get separate chunks to count.

## Speedup compares elapsed times for the same job

- Source lineage: `lecture#sections.card03`
- Citations: none

- Elapsed time: time from starting the job to getting its result.
- Speedup: one-worker elapsed time divided by elapsed time with more workers.

### Diagram explanation

Illustrative comparison, not the sensor prediction.

- Number line from 0 to 10: Elapsed time (seconds)
- Ticks at 0, 5, 10
- Point at 5: Illustration: more workers
- Point at 10: One worker

## Will ten workers finish ten times faster?

- Source lineage: `lecture#sections.card04`
- Citations: none

Predict the elapsed time before calculating.

### Diagram explanation

Two chunks illustrate dependencies; the same pattern supports ten chunks. Read + combine: 1 s; all counting on one worker: 9 s

- Arrow from Read file (wait before counting) to Count chunk A
- Arrow from Read file (wait before counting) to Count chunk B
- Arrow from Count chunk A to Combine counts (wait for every count)
- Arrow from Count chunk B to Combine counts (wait for every count)

Read + combine: 1 s; counting on one worker: 9 s. Two chunks show the pattern; ten workers use ten chunks.

## Ten workers still need the unchanged second

- Source lineage: `lecture#sections.card05`
- Citations: none

Counts can overlap; reading and combining still take time. We separate the unchanged work from the work workers can share.

- Serial part: work whose time does not shrink with more workers.
- Parallel part: work divided among workers to reduce its time.

### Diagram explanation

Ten-worker sensor job: 1 + 0.9 = 1.9 s elapsed.

- Record, left to right: 2 fields, 1.9 seconds in total
- Field 1, Read + combine: serial; 1 of 1.9 seconds; value 1 s
- Field 2, Counting: parallel; 0.9 of 1.9 seconds; value 9 ÷ 10 = 0.9 s

Total elapsed time: 1 + 0.9 = 1.9 s.

## Ten workers give about five times speedup

- Source lineage: `lecture#sections.card06`
- Citations: none

The whole job improves less than its counting step.

### Speedup = 10 ÷ 1.9 ≈ 5.26×

| Run | Elapsed time |
| --- | --- |
| One worker | 10 s |
| Ten workers | 1.9 s |

## Amdahl's law keeps the serial time unchanged

- Source lineage: `lecture#sections.card07`
- Citations: none

Amdahl's law: the part that does not speed up limits the whole job's speedup.

### Chart data

One-worker seconds

| Part | Seconds |
| --- | --- |
| Serial (S = 0.1) | 1 |
| Parallel (P = 0.9) | 9 |

Scale 0 s to 10 s

S and P are shares of the original elapsed time. S + P = 1. With c workers, speedup = 1 / (S + P/c). Each worker has one core.

## A hundred workers leave almost one second

- Source lineage: `lecture#sections.card08`
- Citations: none

Reading and combining still take the same time.

### Sensor job: same file, same answer

| Part | Time with 100 workers |
| --- | --- |
| Read + combine | 1 s |
| Count | 9 ÷ 100 = 0.09 s |
| Whole job | 1.09 s |

## The serial fraction sets the speedup ceiling

- Source lineage: `lecture#sections.card09`
- Citations: none

Speedup ceiling: the limit approached as the parallel time shrinks toward zero.

### Diagram explanation

Speedup ceiling = 1/S. Here: 10 s ÷ 1 s = 10×.

- Number line from 0 to 10: Sensor job elapsed time (seconds)
- Ticks at 0, 1, 10
- Point at 1: Time floor: 1 s
- Point at 10: One worker: 10 s
- Interval from 0 to 1: Serial time remains

Speedup ceiling = 1/S. Here: 10 s ÷ 1 s = 10×.

## Can a million cores remove the serial part?

- Source lineage: `lecture#sections.card10`
- Citations: none

Predict the speedup ceiling, not the exact runtime.

### Chart data

One-worker seconds

| Part | Seconds |
| --- | --- |
| Serial (1%) | 1 |
| Parallel (99%) | 99 |

Scale 0 s to 100 s

Different job: 100 s on one worker; S = 0.01. Use ceiling = 1/S. One million cores are available.

## One percent serial caps speedup at one hundred

- Source lineage: `lecture#sections.card11`
- Citations: none

The unchanged work still takes one second.

### The exact million-core speedup remains below the ceiling.

| Quantity | Value |
| --- | --- |
| One-worker elapsed time | 100 s |
| Serial time that remains | 1 s |
| Speedup ceiling | 100 ÷ 1 = 100× |

## Real workers also spend time organizing and waiting

- Source lineage: `lecture#sections.card12`
- Citations: none

- Ideal time omits the costs of coordinating real workers.
- Overhead: extra time spent organizing parallel execution.
- Contention: workers compete for a shared resource.

### Diagram explanation

Sensor model extended with real execution costs: its ideal time omits these costs.

- Arrow from Read file to Assign chunks (extra work)
- Arrow from Assign chunks (extra work) to Count chunks (shared memory may cause waiting)
- Arrow from Count chunks (shared memory may cause waiting) to Combine counts

## Measured counting improves less than worker count

- Source lineage: `lecture#sections.card13`
- Citations: `dis_14_4_1`

The ideal predicts a ceiling; these measured runs achieve less.

### A separate counting experiment, not our 10-second sensor model.

| Threads | All-parallel ideal speedup | Measured speedup |
| --- | --- | --- |
| 2 | 2× | 1.68× |
| 4 | 4× | 2.36× |
| 8 | 8× | 3.08× |

**Citations:** `dis_14_4_1`

Measured machine: four cores, up to eight threads. Ideal column assumes one core per thread.

## Faster code writing leaves human review unfinished

- Source lineage: `lecture#sections.card14`
- Citations: `anthropic_rsi`

- An unchanged step limits a computer job's speedup.
- A team's dependent steps can face the same limit.
- Bottleneck: the step that constrains the overall pace.

### Diagram explanation

Anthropic reports human code review became a bottleneck as code generation accelerated.

- Arrow from Write code (can accelerate) to Person reviews (still needed)
- Arrow from Person reviews (still needed) to Merge

**Citations:** `anthropic_rsi`

Anthropic reports that human code review became a bottleneck as code generation accelerated.

**Attributed to:** Anthropic, When AI builds itself

**Citations:** `anthropic_rsi`

## Instant writing still leaves four hours of review

- Source lineage: `lecture#sections.card15`
- Citations: none

Example: one change, unchanged review, merge time omitted.

### Speedup ceiling = 10 ÷ 4 = 2.5×

| Step | Original time | With writing nearly free |
| --- | --- | --- |
| Write | 6 h | Nearly 0 h |
| Review | 4 h | 4 h |
| Whole change | 10 h | At least 4 h |

## Which improvement can lower the remaining time floor?

- Source lineage: `lecture#sections.card16`
- Citations: none

Writing is already nearly free. Choose and explain.

Which improvement can lower the remaining time floor?

1. Improve writing again
2. Improve review

## The part that does not speed up caps total speedup

- Source lineage: `lecture#sections.card17`
- Citations: none

Improve review to lower this change's remaining time floor.

### Sensor job

- Read and combine remain when counting shrinks

### Software change

- Review remains when writing shrinks

Speed up the remaining constraint to lower the floor.

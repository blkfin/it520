# IT 520 — Performance and Systems: L3 — Will a twice-as-fast processor clear a ten-hour delay?

- Course: IT 520 — Performance and Systems
- Lecture: Week 02 / L3 — Will a twice-as-fast processor clear a ten-hour delay?
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `683150a7ddc466bc79b685cb9f761381f570b1ae6e16665aa8e1fa471cc319ce`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## The nightly report job takes ten hours, and the boss just bought a faster processor

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.nightly_report_job`
- Citations: `l3_storyboard`

The job starts at 8:00 PM. It finishes ten hours later. Nobody can start their morning review until the output file exists. The boss is tired of it, so he buys a new server; the salesperson says the processor is twice as fast. It is not cheap.

**Citations:** `l3_storyboard`

When do the workers get the file? Write a time and one reason. Keep this answer; do not erase it.

**Response mode:** written

**Time:** 90 seconds

## This job begins and ends as a file, so the board gains storage

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.board_gains_storage`
- Citations: `l3_storyboard`, `c1_1_course_knowledge`

### One job token, four states

| State | Where it is | What is happening |
| --- | --- | --- |
| stored | STORAGE | The input data file is on disk and nothing has started |
| loaded | STORAGE to MEMORY | Data movement: bytes travel into memory |
| computed | CPU | Processor work: the ALU actually calculates |
| saved | MEMORY to STORAGE | Data movement: the output file is written, and only now can a worker open it |

**Constraints:** preserve_exact_values

Takeaway: A job's elapsed time is spent moving data and computing, in that order and back again.

## Every hour is either data movement or processor work

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.two_kinds_of_time`
- Citations: `l3_storyboard`

- **data movement**: getting bytes between storage and memory
- **processor work**: the CPU actually calculating

**Citations:** `l3_storyboard`

This model isolates one effect. It does not predict a real machine's elapsed time.

Takeaway: The ten hours now has two named buckets, and their ratio is what the worked cases turn on.

## The boss bought a processor twice as fast and saved one hour

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.condition_a_disappointment`
- Citations: `l3_storyboard`, `l3_companion`

### Condition A, before and after the purchase; each M or P is one hour

| State | Data movement (M) | Processor work (P) | Total |
| --- | --- | --- | --- |
| Before | M M M M M M M M | P P | 10 hours |
| After | M M M M M M M M | P | 9 hours |

**Constraints:** preserve_exact_values

Eight hours stay unchanged; processor work is halved.

**Citations:** `l3_storyboard`

8 + 2 = 10 becomes 8 + 1 = 9. This job is I/O-bound: most of its time goes to moving data, not to computing.

**Citations:** `l3_storyboard`

Takeaway: Only the stage the purchase touches changes; the other eight hours are untouched.

## The same processor saves four hours when processor work holds most of the job

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.condition_b_reversal`
- Citations: `l3_storyboard`, `l3_companion`

### Condition B, before and after the same purchase; each M or P is one hour

| State | Data movement (M) | Processor work (P) | Total |
| --- | --- | --- | --- |
| Before | M M | P P P P P P P P | 10 hours |
| After | M M | P P P P | 6 hours |

**Constraints:** preserve_exact_values

Two hours unchanged; processor work is halved.

**Citations:** `l3_storyboard`

2 + 8 = 10 becomes 2 + 4 = 6. The saving moved from one hour to four with no change to the hardware claim. This job is CPU-bound: most of its time goes to computing.

**Citations:** `l3_storyboard`

Takeaway: The benefit of a faster processor depends on which stage holds the time.

## A processor has a clock, and its tick rate is measured in hertz

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.clock_and_cycle_time`
- Citations: `l3_storyboard`, `c1_3_course_knowledge`

- **clock**: the source of the processor's ticks
- **cycle**: one tick, the unit in which the processor advances its work
- **hertz**: cycles per second: 1 Hz is one cycle per second, 1 MHz is a million, 1 GHz is a billion

**Citations:** `l3_storyboard`

- cycle time = 1 ÷ clock rate. At 2 GHz one cycle takes 0.5 nanoseconds; at 4 GHz, 0.25 nanoseconds.
- time = work ÷ rate

**Citations:** `l3_storyboard`

Takeaway: The faster the clock ticks, the less time each single cycle takes.

## One operation takes 8 cycles on a 2 GHz processor

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.warm_up_eight_cycles`
- Citations: `l3_storyboard`

One operation takes 8 cycles on a 2 GHz processor. One cycle is 0.5 nanoseconds, so 8 × 0.5 ns = 4 nanoseconds.

**Citations:** `l3_storyboard`

Takeaway: Multiply the fixed work by the time for each cycle.

## The same 14.4 trillion cycles take one hour at 4 GHz

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.same_cycles_new_rate`
- Citations: `l3_storyboard`

### Old server, 2 GHz

- Processor work: 2 hours = 7,200 seconds
- Cycles the job needs: 7,200 × 2,000,000,000 = 14.4 trillion cycles

### New server, 4 GHz

- Same job, same instructions, same 14.4 trillion cycles
- 14,400,000,000,000 ÷ 4,000,000,000 = 3,600 seconds = 1 hour

A fixed 14.4 trillion-cycle workload takes half as long when the clock rate doubles from 2 GHz to 4 GHz.

**Citations:** `l3_storyboard`

What we are assuming to say that: same program, same instructions, same average number of cycles per instruction. Only the clock rate changes. That assumption is what lets us carry the 14.4 trillion across; a genuinely different processor could need a different number of cycles for the same job. GHz is a rate, not a speed.

**Citations:** `l3_storyboard`

Takeaway: Holding the cycle count fixed is a declared assumption, and it is what makes the halving true.

## Doubling processor speed makes this whole job 1.11× faster

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.component_versus_whole_job`
- Citations: `l3_storyboard`, `l3_companion`

The processor got 2× faster. The job fell from 10 hours to 9, so whole-job speedup is 10 ÷ 9 = 1.11×, about 11% more performance. Workers wait 10% less.

**Citations:** `l3_storyboard`

- **CPU time**: the 2 hours the processor was computing: what the salesperson was quoting
- **elapsed time (wall time)**: all 10 hours the workers waited: what the boss was paying for

**Citations:** `l3_storyboard`

Takeaway: Measure performance with old elapsed time ÷ new elapsed time; measure waiting reduction separately.

## No processor that will ever exist saves more than two hours on this job

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.infinite_processor_ceiling`
- Citations: `l3_storyboard`

Suppose the new processor were infinitely fast: the processor stage drops to zero. 8 + 0 = 8 hours. The boss bought 2× and got one of those two hours. No processor can get this job to five hours; eight hours is the floor.

**Citations:** `l3_storyboard`

Takeaway: 8 + 0 = 8 hours. The unchanged stage sets the floor.

## Each specification needs a matching workload fact to predict time

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.three_specs_as_a_purchase`
- Citations: `l3_storyboard`, `c1_3_course_knowledge`

### The boss has money left and three things he could buy

| What the spec says | Its unit | Can it give you a time? |
| --- | --- | --- |
| Processor clock: rated maximum 4 GHz (4 billion cycles per second under stated conditions) | cycles ÷ second | Yes, with the job's cycle count: cycles ÷ (cycles/sec) = seconds. The rated maximum is a ceiling, not the sustained rate your job gets |
| Storage transfer rate: 600 million bytes per second | bytes ÷ second | Yes, with the byte count: bytes ÷ (bytes/sec) = seconds |
| Memory capacity: 64 billion bytes (amount that fits; workload need unknown) | bytes | No timing result: capacity is an amount, and whether this job is short of memory is unknown |

**Constraints:** preserve_exact_values

Takeaway: Units decide whether a specification can answer a timing question.

## A rated maximum is a ceiling, not the rate this job sustains

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.maximum_and_bottleneck_boundary`
- Citations: `l3_storyboard`, `c1_3_course_knowledge`

time = work ÷ rate. A rate has a "per second" in it; an amount does not. Only a rate can turn into a time, and only if you also know the amount of work. Memory capacity answers how much fits. Judging more memory for this job requires a missing workload fact: whether this job is short of memory right now. Hold that thought.

**Citations:** `l3_storyboard`

Read the first row exactly as printed: rated maximum 4 GHz under stated conditions. Treat that as a ceiling. A timing prediction also requires the sustained rate while the job runs and the number of cycles the job needs.

**Citations:** `l3_storyboard`

There is a word for the stage that holds most of the elapsed time: the bottleneck. We are only naming it today; finding one in a real system needs measurements we have not taken yet, and that is later in the course.

**Citations:** `l3_storyboard`

Takeaway: Predicting time requires workload facts and the rate sustained while the job runs.

## Reducing the work can lower elapsed time without raising a hardware rate

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.work_versus_rate`
- Citations: `l3_storyboard`, `l3_companion`

### time = work ÷ rate; rate is on the bottom and work is on top

| Lever | Supplied case | Effect |
| --- | --- | --- |
| Rate | Same work at a higher processor or storage transfer rate | Less time |
| Work | But suppose this job is short of memory and reads the file in three passes; enough memory lets it fit in one pass | One-third as much data because the job got smaller |

**Constraints:** preserve_exact_values

Two of the three upgrades raise a rate. Memory is the odd one: more memory does not move bytes any faster.

**Citations:** `l3_storyboard`

- Read the file once instead of three times.
- Skip records the report never uses.
- Stop recomputing the same total for every row.

**Citations:** `l3_storyboard`

Changing the procedure is free of hardware purchase costs and can reduce the work.

**Citations:** `l3_storyboard`

Takeaway: Elapsed time falls when the work decreases or the rate increases.

## Go back to the answer you wrote at minute zero

- Source lineage: `it520-fall-2026-week02-l3-faster-processor#sections.collect_the_revision`
- Citations: `l3_storyboard`, `l3_companion`

Keep your first answer. Mark one correction or confirmation, and name what evidence changed your mind.

**Response mode:** written

**Time:** 60 seconds

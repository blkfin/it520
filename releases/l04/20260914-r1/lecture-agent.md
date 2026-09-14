# IT 520 — Performance and Systems: L04 — Latency and throughput

- Course: IT 520 — Performance and Systems
- Lecture: Week 03 / L04 — Latency and throughput
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `2c70e4a1336fda6c063a47f0915458aad42f07316515c2dd39eb478ce327128c`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What approaches can I take to increase the performance of a slow system?

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c01`
- Citations: `approved_storyboard`

- Diagnose what ‘slow’ means, then test whether a change helped.

**Citations:** `approved_storyboard`

## Another slow system: a highway charging plaza

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c02`
- Citations: `approved_storyboard`

- Five charging ports beside one highway.
- Every car needs 30 kWh; every port is rated 150 kW.
- Watched 5 to 6 pm on 8 weekday evenings.
- On average, 20 cars enter and 20 leave charged.
- Drivers call the stop slow: faster chargers, or more ports?

**Citations:** `approved_storyboard`

## How we measure the plaza

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c03`
- Citations: `approved_storyboard`

### The plaza boundary and one-hour observation.

| Measure | Plaza record |
| --- | --- |
| Workload | the requests a system serves; here, cars needing 30 kWh. |
| Interval T | how long we watched: 5 to 6 pm, 1 hour. |
| Arrivals A | requests that entered during T: 20 cars. |
| Completions C | requests that left finished during T: 20 cars. |

**Citations:** `approved_storyboard`

- A fair comparison repeats the same workload, boundary, and hour.

**Citations:** `approved_storyboard`

## Throughput counts the cars that leave charged each hour

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c04`
- Citations: `approved_storyboard`

- Throughput X: how fast finished work leaves: X = C/T.
- Counted at the exit, not the entrance.
- X = 20 cars ÷ 1 hour = 20 cars/hour.

**Citations:** `approved_storyboard`

### Chart data

Cars counted at the plaza boundary, 5 to 6 pm

| Counter | Cars |
| --- | --- |
| Entered (A) | 20 |
| Left charged (C) | 20 |

Scale 0 cars to 20 cars

**Citations:** `approved_storyboard`

## Latency is one request's time; throughput is requests finished per hour

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c05`
- Citations: `approved_storyboard`

### Chart data

One car's average stop versus the average gap between departures

| Measure | Minutes |
| --- | --- |
| One car's stop (latency R) | 18 |
| Gap between departures (1/X) | 3 |

Scale 0 min to 18 min

**Citations:** `approved_storyboard`

- Latency R: one request's time, from entering to leaving.
- R = 18 minutes per car, averaged over the hour.
- X = 20 cars/hour: departures 1/X = 3 minutes apart.
- 3 minutes is not a stop; several cars are inside at once.

**Citations:** `approved_storyboard`

## Service time is only the time on a port

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c06`
- Citations: `approved_storyboard`

### Chart data

Where the 18-minute average stop goes

| Part of the stop | Minutes |
| --- | --- |
| Charging (service time S) | 12 |
| Waiting | 6 |

Scale 0 min to 18 min

**Citations:** `approved_storyboard`

- Busy time B: port time spent charging: 240 minutes, 4 port-hours.
- Service time S: time one request holds a resource: S = B/C.
- S = 4 port-hours ÷ 20 cars = 0.2 hour = 12 minutes.
- Check: 30 kWh ÷ 150 kW = 0.2 hour = 12 minutes.
- Of the 18 minutes inside, 12 are charging and 6 are not.

**Citations:** `approved_storyboard`

## The Utilization Law: how busy the ports are

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c07`
- Citations: `approved_storyboard`, `lazowska_ch3`

### Chart data

The five ports on average

| Port state | Ports |
| --- | --- |
| Busy ports | 4 |
| Open ports | 1 |

Scale 0 ports to 5 ports

**Citations:** `approved_storyboard`

- Utilization U: the share of a resource's time spent serving.
- Utilization Law: for one resource, U = X·S.
- Across this pool, X·S = 20/hour × 0.2 hour = 4 busy ports.
- Pool utilization: 4 busy ports ÷ 5 ports = 80%.
- Concurrency: requests served at the same time; here, 4 cars.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## Little's Law: how many cars are inside

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c08`
- Citations: `approved_storyboard`, `lazowska_ch3`

### Chart data

Cars inside the plaza on average

| Where in the plaza | Cars |
| --- | --- |
| Charging | 4 |
| Waiting | 2 |
| Inside in total (N) | 6 |

Scale 0 cars to 6 cars

**Citations:** `approved_storyboard`

- Little's Law: the average number inside is throughput times time inside. N = X·R
- 18 minutes = 0.3 hour, so N = 20 × 0.3 = 6 cars.
- 4 cars charging, 2 cars waiting.
- Queueing: time inside while not being served.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## The Utilization Law and Little's Law are one relationship around two boundaries

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c09`
- Citations: `approved_storyboard`, `lazowska_ch3`

### Chart data

Cars inside each boundary on average

| Boundary | Cars |
| --- | --- |
| Box around the ports | 4 |
| Box around the plaza | 6 |

Scale 0 cars to 6 cars

**Citations:** `approved_storyboard`

- Inside a boundary = rate through it × time spent inside it.
- Utilization Law, box around the ports: 20/hour × 0.2 hour = 4 charging.
- Little's Law, box around the plaza: 20/hour × 0.3 hour = 6 inside.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## Faster chargers cut both the charge and the wait

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c10`
- Citations: `approved_storyboard`

### Chart data

Where the 6-minute stop goes with faster chargers

| Part of the stop | Minutes |
| --- | --- |
| Charging (service time S) | 6 |
| Waiting | 0 |

Scale 0 min to 18 min

**Citations:** `approved_storyboard`

- Same 5 to 6 pm, 8 evenings, 20 cars/hour, five ports.
- Meters: 6 minutes charging per car; 2 cars inside on average.
- Utilization Law: 20/hour × 0.1 hour = 2 busy ports of 5, 40%.
- Little's Law: R = N/X = 2 ÷ 20/hour = 0.1 hour = 6 minutes.
- Average wait: 6 minutes − 6 minutes = 0 minutes.

**Citations:** `approved_storyboard`

## More ports cut the wait but not the charge

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c11`
- Citations: `approved_storyboard`

### Chart data

Where the 12-minute stop goes with ten ports

| Part of the stop | Minutes |
| --- | --- |
| Charging (service time S) | 12 |
| Waiting | 0 |

Scale 0 min to 18 min

**Citations:** `approved_storyboard`

- Same 5 to 6 pm, 8 evenings, 20 cars/hour, ten ports.
- Meters: 12 minutes charging per car; 4 cars inside on average.
- Utilization Law: 20/hour × 0.2 hour = 4 busy ports of 10, 40%.
- Little's Law: R = N/X = 4 ÷ 20/hour = 0.2 hour = 12 minutes.
- Average wait: 12 minutes − 12 minutes = 0 minutes.

**Citations:** `approved_storyboard`

## Latency reveals which upgrade better fixes the slow stop

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c12`
- Citations: `approved_storyboard`

### The two upgrades observed with the same workload, boundary, and hour.

| Upgrade | Throughput | Utilization | Cars inside | Latency |
| --- | --- | --- | --- | --- |
| faster chargers | 20 cars/hour | 40% | 2 | 6 minutes |
| more ports | 20 cars/hour | 40% | 4 | 12 minutes |

**Citations:** `approved_storyboard`

- Both upgrades: 20 cars/hour, 40% utilized.
- Both beat the 18-minute stop: 6 minutes versus 12.
- Same 20 cars/hour, twice the cars inside, twice the time.
- Judge an upgrade by the measure that was slow.
- Faster chargers win here: 12 of the 18 minutes were charging, only 6 waiting.

**Citations:** `approved_storyboard`

## When most of the stop is waiting, more ports win

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c13`
- Citations: `approved_storyboard`

### Chart data

A stop that is mostly waiting

| Part of the stop | Minutes |
| --- | --- |
| Charging (service time S) | 3 |
| Waiting | 15 |

Scale 0 min to 18 min

**Citations:** `approved_storyboard`

- Suppose a stop is 3 minutes charging and 15 minutes waiting.
- Faster chargers can only shrink the 3 minutes.
- More ports shrink the waiting and take on more demand.
- Waiting grows fast as a system gets close to full.
- Faster has limits: a battery, like one CPU core, only goes so fast.

**Citations:** `approved_storyboard`

## A disk obeys the Utilization Law and Little's Law

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c14`
- Citations: `approved_storyboard`, `lazowska_ch3`

### Chart data

Requests inside each boundary on average

| Boundary | Requests |
| --- | --- |
| Box around the disk | 0.9 |
| Box around the disk and its line | 4 |

Scale 0 requests to 4 requests

**Citations:** `approved_storyboard`, `lazowska_ch3`

- A disk: X = 40 requests/second, S = 0.0225 second each.
- Utilization Law: 40/s × 0.0225 s = 0.90, so 90% busy.
- Little's Law: 4 requests inside, so R = 4 ÷ 40/s = 0.1 s.

**Citations:** `approved_storyboard`, `lazowska_ch3`

## Find where the time goes: working or waiting

- Source lineage: `it520-fall-2026-week03-l04-v3-latency-throughput#sections.c15`
- Citations: `approved_storyboard`

### All three plaza observations use the same workload and window.

| Measure | Before | Faster chargers | More ports |
| --- | --- | --- | --- |
| workload | 20 cars/hour, 30 kWh each | same | same |
| window | 5 to 6 pm | same | same |
| evenings | 8 | 8 | 8 |
| latency | 18 minutes | 6 minutes | 12 minutes |
| throughput | 20 cars/hour | 20 cars/hour | 20 cars/hour |

**Citations:** `approved_storyboard`

- First split the stop: time working versus time waiting.
- A faster resource cuts working time, and usually the wait.
- More resources cut only the waiting.
- Change one thing, then repeat the same observation.
- Claim only what the measurements show.

**Citations:** `approved_storyboard`

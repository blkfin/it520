# Will it fit?

- Course: IT 520
- Lecture: 08 / Will it fit?
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `413cdff98da3da9dfeaa1ec7a7817c82875a2bbd403268a713ab0d12477b27eb`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## Will this data fit in memory?

- Source lineage: `lecture#sections.card_01`
- Citations: none

By the end, you can count distinct records, budget their bytes, compare memory tiers, and explain why model weights alone cannot prove a run fits.

Your laptop has 16 GB of RAM. Can it hold a model called 9B?

## Our station reader revisits records within one window

- Source lineage: `lecture#sections.card_02`
- Citations: none

- A weather-station reader requests station data records.
- We keep one copy per station record.
- Observe requests from 10:00 through just before 10:01.
- How many records must memory hold for these eight requests?

Requests in this window

```text
[S1, S4, S1, S2, S4, S1, S7, S2]
```

## The working set keeps each needed record once

- Source lineage: `lecture#sections.card_03`
- Citations: `levin_sets`

Working set: the distinct records needed within a stated observation window.

| Window | Requests | Working set W |
| --- | --- | --- |
| 10:00 to before 10:01 | S1, S4, S1, S2, S4, S1, S7, S2 | {S1, S2, S4, S7} |

## Four distinct records require four stored payloads

- Source lineage: `lecture#sections.card_04`
- Citations: `levin_sets`

Cardinality: the number of distinct members in a set.

| Quantity | Value |
| --- | --- |
| W | {S1, S2, S4, S7} |
| Requests in the window | 8 |
| Cardinality, written |W| | 4 |

## Count times record size gives the payload budget

- Source lineage: `lecture#sections.card_05`
- Citations: none

- Payload means the record's data bytes, excluding storage overhead.
- Assume equal-size records; count payload only.

| Step | This window |
| --- | --- |
| Count distinct records | 4 records |
| Size of each record | 64 B per record |
| Multiply count by size | 4 records × 64 B per record = 256 B |
| Write the rule | B = |W| × record_size |

## A larger station job needs 12.8 megabytes

- Source lineage: `lecture#sections.card_06`
- Citations: none

This job supplies a new distinct-record count, separate from the eight-request log.

### 1 MB = 10⁶ B; 1 GB = 10⁹ B; 1 GiB = 2³⁰ B. Exercise capacities use decimal units.

| Quantity | Value |
| --- | --- |
| Distinct station records | 200,000 records |
| Record size | 64 B per record |
| Payload budget B | 200,000 × 64 B = 12,800,000 B |
| Decimal megabytes | 12,800,000 B ÷ 1,000,000 = 12.8 MB |

## Memory tiers trade greater capacity for longer waits

- Source lineage: `lecture#sections.card_07`
- Citations: `dis_mem`, `dis_devices`

- We know the job needs 12.8 MB. Which memory should hold it?
- Capacity: how many bytes a device can hold.
- Latency: time from a request until its data arrives.
- Memory hierarchy: tiers exist because no device is both largest and fastest.

### Station job: 12.8 MB payload; classroom machine

| Tier | Capacity | Typical latency order |
| --- | --- | --- |
| Cache | 16 MB | Shortest |
| Main memory (RAM) | 16 GB | Longer than cache |
| Storage | Larger than RAM in this example | Longest |

## Fits tests each tier against the byte budget

- Source lineage: `lecture#sections.card_08`
- Citations: none

Fits(t): true exactly when the budget B is at most tier t's capacity.

### Station job B = 12,800,000 B; Fits(t) ⇔ B ≤ capacity(t)

| Input | Mapping gives a number | Predicate test (true or false?) |
| --- | --- | --- |
| Cache | capacity(cache) = 16,000,000 B | 12,800,000 B ≤ 16,000,000 B |
| RAM | capacity(RAM) = 16,000,000,000 B | 12,800,000 B ≤ 16,000,000,000 B |

## The small station job fits both classroom tiers

- Source lineage: `lecture#sections.card_09`
- Citations: `dis_locality`, `dis_caching`

Fits means the data could be held, not that it is in the cache now.

### 200,000 distinct records × 64 B = 12.8 MB

| Tier | Capacity in MB | Test | Fits |
| --- | --- | --- | --- |
| Cache | 16 MB | 12.8 MB ≤ 16 MB | True |
| RAM | 16,000 MB | 12.8 MB ≤ 16,000 MB | True |

## Which tiers can hold three million station records?

- Source lineage: `lecture#sections.card_10`
- Citations: none

- Use the same method on a new supplied distinct count.
- Distinct station records: 3,000,000
- Each record: 64 B; equal sizes, payload only
- Cache: 16 MB; RAM: 16,000 MB
- 1 MB = 1,000,000 B

Compute payload bytes, convert to MB, then decide Fits(cache) and Fits(RAM).

**Response mode:** written

## The larger job exceeds cache but fits RAM

- Source lineage: `lecture#sections.card_11`
- Citations: `dis_locality`, `dis_caching`

- Exceeding cache capacity makes some reads wait on slower RAM.
- This payload is twelve times the cache capacity.

### 3,000,000 records × 64 B = 192,000,000 B = 192 MB

| Tier | Test | Fits |
| --- | --- | --- |
| Cache | 192 MB > 16 MB | False |
| RAM | 192 MB ≤ 16,000 MB | True |

## Model weights are another byte budget

- Source lineage: `lecture#sections.card_12`
- Citations: `qwen_counts`

- Station budgets used record count and bytes per record.
- A model needs number counts and bytes per number instead.
- Weights: stored numeric values that define a model's learned behavior.

### Station reader

- 200,000 distinct records
- 64 B per record
- 12.8 MB payload

### Qwen3.5-9B model

- The name says 9B: roughly nine billion stored values
- Most use 2 B; a small group uses 4 B
- Can its weights fit in 16 GB RAM?

## Qwen's exact weights exceed sixteen gigabytes of RAM

- Source lineage: `lecture#sections.card_13`
- Citations: `qwen_counts`

Multiply each group by its own byte size, then add.

### Weights ≈ 19.31 GB > 16 GB RAM: Fits(RAM) is false.

| Group | Value count | Bytes per value | Weight bytes |
| --- | --- | --- | --- |
| BF16 | 9,653,100,528 | 2 B | 19,306,201,056 B |
| F32 | 3,840 | 4 B | 15,360 B |
| Total | 9,653,104,368 | Mixed | 19,306,216,416 B |

## A separate GPU budget leaves about 4.69 gigabytes

- Source lineage: `lecture#sections.card_14`
- Citations: `qwen_counts`, `nvidia_l4`

- The weights fail 16 GB RAM. Try a GPU card with 24 GB of its own memory.
- RAM and GPU memory are separate budgets; do not add them.

### Qwen3.5-9B weights; assumed decimal 24 GB GPU budget

| Quantity | Bytes |
| --- | --- |
| GPU budget | 24,000,000,000 B |
| Weights | 19,306,216,416 B |
| Remaining: budget minus weights | 4,693,783,584 B ≈ 4.69 GB |

## A running model needs more than stored weights

- Source lineage: `lecture#sections.card_15`
- Citations: none

- KV cache: memory holding information computed from the text so the model can reuse it.
- Runtime allocations: other memory used while executing the model.

### Same 24,000,000,000 B GPU budget

| Allocation | Memory needed |
| --- | --- |
| Qwen3.5-9B weights | 19,306,216,416 B |
| Context/KV cache | Not supplied |
| Other runtime allocations | Not supplied |
| Combined allowance for both unknown allocations | At most 4,693,783,584 B |

## A fit claim needs the whole budget and a tier

- Source lineage: `lecture#sections.card_16`
- Citations: none

### Station reader

- 192 MB payload
- 16 MB cache: does not fit
- 16 GB RAM: fits
- Payload-only scope excludes overhead

### Qwen3.5-9B on the GPU

- About 19.31 GB of weights fits 24 GB
- About 4.69 GB remains for all other allocations
- Whole-runtime fit remains undecided

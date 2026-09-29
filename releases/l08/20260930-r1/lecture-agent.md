# IT 520 — Decimal to binary past the point

- Course: IT 520 — Performance and Systems
- Lecture: Week 05 / September 30 — Decimal to binary past the point
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `4f37fe6e37e966702177b540c67ece7ec6413d1e70590bc19e752dfb24dfba4e`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## How does decimal become binary past the point?

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c01`
- Citations: `approved_storyboard`

### By the end, you can convert whole numbers and short fractions to binary, check both by place value, and test whether every fraction's bits come to an end.

### Exact tote count

- 20 items

### Measured item weight

- 0.1 kg

**Citations:** `approved_storyboard`

## The unfinished count still needs its bits

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c02`
- Citations: `approved_storyboard`

- The tote starts at 0 items.
- Each successful scan adds 1.
- The count must reach 20.
- What bits should store 20₁₀?

| Count | Bits |
| --- | --- |
| 0 → 1 → 2 → … → 20 | ___ |

## Division-remainder builds thirteen from right to left

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c03`
- Citations: `approved_storyboard`, `dive_bases`

| Divide | Quotient | Remainder |
| --- | --- | --- |
| 13 ÷ 2 | 6 | 1 |
| 6 ÷ 2 | 3 | 0 |
| 3 ÷ 2 | 1 | 1 |
| 1 ÷ 2 | 0 | 1 |

- Division-remainder: divide by 2; the remainder can only be 0 or 1, so it is a bit.

- Read remainders last to first: 13₁₀ = 1101₂
- Check: 8 + 4 + 1 = 13

## Use division-remainder to finish the count

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c04`
- Citations: `approved_storyboard`

| Divide | Quotient | Remainder |
| --- | --- | --- |
| 20 ÷ 2 | ___ | ___ |
| ___ ÷ 2 | ___ | ___ |
| ___ ÷ 2 | ___ | ___ |
| ___ ÷ 2 | ___ | ___ |
| ___ ÷ 2 | ___ | ___ |

- Stop at quotient 0.
- Read remainders last to first.
- Check with place value.

## Twenty becomes 10100₂, and place value confirms it

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c05`
- Citations: `approved_storyboard`, `dive_bases`

| Divide | Quotient | Remainder |
| --- | --- | --- |
| 20 ÷ 2 | 10 | 0 |
| 10 ÷ 2 | 5 | 0 |
| 5 ÷ 2 | 2 | 1 |
| 2 ÷ 2 | 1 | 0 |
| 1 ÷ 2 | 0 | 1 |

- 10100₂ = 16 + 4 = 20₁₀

## Binary place value continues past the point

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c06`
- Citations: `approved_storyboard`, `dive_fractions`

| Place | 4 | 2 | 1 | . | 1/2 | 1/4 | 1/8 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 101.1₂ | 1 | 0 | 1 | . | 1 | 0 | 0 |

- Binary fraction: each place rightward is half the previous place.

- 101.1₂ = 4 + 1 + 1/2 = 5.5₁₀

## Repeated doubling pulls fraction bits out in order

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c07`
- Citations: `approved_storyboard`, `dive_fractions`

- Repeated doubling: double the fraction, copy the whole bit, then erase it; stop when Keep is 0.

| Step | Double | Bit | Keep |
| --- | --- | --- | --- |
| 1 | 0.625 × 2 = 1.25 | 1 | 0.25 |
| 2 | 0.25 × 2 = 0.5 | 0 | 0.5 |
| 3 | 0.5 × 2 = 1.0 | 1 | 0 |

- Read first to last: 0.625₁₀ = 0.101₂
- Check: 1/2 + 1/8 = 0.625

## Use repeated doubling on three eighths

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c08`
- Citations: `approved_storyboard`

| Step | Double | Bit | Keep |
| --- | --- | --- | --- |
| 1 | 0.375 × 2 = ___ | ___ | ___ |
| 2 | ___ × 2 = ___ | ___ | ___ |
| 3 | ___ × 2 = ___ | ___ | ___ |

- Double, copy the whole bit, erase it, and stop when Keep is 0.
- Read bits first to last.
- Check with halves, quarters, and eighths.

## Three eighths becomes .011₂ and checks exactly

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c09`
- Citations: `approved_storyboard`, `dive_fractions`

| Step | Double | Bit | Keep |
| --- | --- | --- | --- |
| 1 | 0.375 × 2 = 0.75 | 0 | 0.75 |
| 2 | 0.75 × 2 = 1.5 | 1 | 0.5 |
| 3 | 0.5 × 2 = 1.0 | 1 | 0 |

- 0.375₁₀ = 0.011₂
- Check: 1/4 + 1/8 = 0.375

## Will one tenth ever reach zero?

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c10`
- Citations: `approved_storyboard`

### Start repeated doubling with 0.1.

| Double | Bit | Keep |
| --- | --- | --- |
| 0.1 × 2 = 0.2 | 0 | 0.2 |
| ___ | ___ | ___ |
| ___ | ___ | ___ |
| ___ | ___ | ___ |
| ___ | ___ | ___ |

- Stop only if the kept fraction reaches 0.
- Predict: terminates or repeats?

## One tenth enters a repeating binary loop

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c11`
- Citations: `approved_storyboard`, `python_fp`

| Double | Bit | Keep |
| --- | --- | --- |
| 0.1 × 2 = 0.2 | 0 | 0.2 |
| 0.2 × 2 = 0.4 | 0 | 0.4 |
| 0.4 × 2 = 0.8 | 0 | 0.8 |
| 0.8 × 2 = 1.6 | 1 | 0.6 |
| 0.6 × 2 = 1.2 | 1 | 0.2 ↺ |

- 0.1₁₀ = 0.0001100110011…₂
- A stored value has a fixed number of bits, so this pattern is cut off at the nearest value that fits.

## Cutting off the pattern changes the arithmetic

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c12`
- Citations: `approved_storyboard`, `python_fp`

### Three items, 0.1 kg each:

### Code state 1

Equality

```python
>>> 0.1 + 0.1 + 0.1 == 0.3
False
```

### Code state 2

Sum

```python
>>> 0.1 + 0.1 + 0.1
0.30000000000000004
```

| Stored value of 0.1, written in decimal | Display |
| --- | --- |
| 0.10000000000000000555… | 0.1 |

- Answer: Whole numbers use division-remainder; fractions use repeated doubling. A repeating fraction needs a nearby finite approximation.

## What we can convert so far

- Source lineage: `it520-fall-2026-sep30-decimal-binary-reals-r2#sections.c13`
- Citations: `approved_storyboard`

- 5.5₁₀ → 101.1₂ (5 → 101, 0.5 → .1)

| Decimal | Binary | Stored as |
| --- | --- | --- |
| 20₁₀ | 10100₂ | exactly |
| 0.375₁₀ | 0.011₂ | exactly |
| 0.1₁₀ | 0.0001100110011…₂ | ___ |

- How do you store a point in a computer?

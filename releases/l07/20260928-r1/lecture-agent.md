# IT 520 — Same bits, different meaning

- Course: IT 520 — Performance and Systems
- Lecture: Week 05 / September 28 — Signed numbers and overflow
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `4a467ec2e4e4442ffce168fd9b9b89e12ef34448ebec68e55bb7f6c242813285`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## Last time: what did 1101 mean?

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c01`
- Citations: `approved_storyboard`

| Place value | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- |
| Tote count bits | 1 | 1 | 0 | 1 |

What count does the app report? What rule did you use?

- Count and rule

**Response mode:** verbal_guess

**Time:** 30 seconds

## Predict the sixteenth scan

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c02`
- Citations: `approved_storyboard`, `dive_addition`

| Items recorded | Four-bit count |
| --- | --- |
| 13 | 1101 |
| 14 | 1110 |
| 15 | 1111 |
| 16 | ???? |

What four bits will remain after adding one to 1111?

- Stored bits

**Response mode:** verbal_guess

**Time:** 10 seconds

## The field keeps four bits, not five

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c03`
- Citations: `approved_storyboard`, `dive_addition`

### Code state 1

FOUR-BIT FIELD

```text
carry out:      1     (outside the field)
four-bit field:  1111
               + 0001
               ------
stored bits:     0000
```

- The app has recorded 16 items.
- The four-bit unsigned field reports 0.
- 0000 is a valid pattern. It is the wrong result for this count.

## Unsigned overflow means the result does not fit

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c04`
- Citations: `approved_storyboard`, `dive_bases`, `dive_overflow`

| Four-bit unsigned count | Value |
| --- | --- |
| Patterns available | 2⁴ = 16 |
| Range | 0 through 15 |
| Mathematical result | 16, outside the range |
| Stored pattern | 0000, the counter wraps |

## The report now needs to record a removal

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c05`
- Citations: `approved_storyboard`

| Since the last report | Count | Last change |
| --- | --- | --- |
| Two items scanned in | 11 → 13 | +2 |
| One item taken out | 13 → 12 | −1 |

- New four-bit field: last change = count now minus count at the last report.
- Each row is a separate report, not a running total.
- Can an unsigned field report −1?

## The same 1111 can be fifteen or negative one

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c06`
- Citations: `approved_storyboard`, `dive_signed`

| Bits | Rule | Value |
| --- | --- | --- |
| 1111 | unsigned | 15 |
| 1111 | two’s complement | −1 |

- The bits did not change. The rule for reading the field did.

## Test 1111 by taking one step

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c07`
- Citations: `approved_storyboard`, `dive_signed`, `dive_overflow`

### Code state 1

FOUR-BIT RESULT

```text
bits     meaning
  1111   −1
+    1
------
  0000    0
```

- The patterns advance by one.
- −1 + 1 = 0, so 1111 sits immediately before 0000.
- Same bit addition, two readings: unsigned 15 + 1 overflows; two’s complement −1 + 1 = 0 is correct.

## Two’s complement splits the patterns around zero

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c08`
- Citations: `approved_storyboard`, `dive_signed`

| Bits | Unsigned | Two’s complement |
| --- | --- | --- |
| 0000 | 0 | 0 |
| … | … | … |
| 0111 | 7 | 7 |
| 1000 | 8 | −8 |
| … | … | … |
| 1111 | 15 | −1 |

- Shortcut: if the first bit is 1, subtract 2^width (1111: 15 − 16 = −1).

## Signed values trade positive range for negatives

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c09`
- Citations: `approved_storyboard`, `dive_addition`

| Four-bit rule | Smallest | Largest | Patterns |
| --- | --- | --- | --- |
| Unsigned | 0 | 15 | 16 |
| Two’s complement | −8 | 7 | 16 |

- Same width, same 16 patterns.
- Different assignment of patterns to values.

## Eight 1s mean 255 or −1

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c10`
- Citations: `approved_storyboard`, `dive_signed`

| Eight bits | Unsigned | Two’s complement |
| --- | --- | --- |
| 1111 1111 | 255 | −1 |

| Eight-bit rule | Range |
| --- | --- |
| Unsigned | 0 through 255 |
| Two’s complement | −128 through 127 |

- What must sender and reader agree on besides the eight bits?

## Predict 7 + 1 in four signed bits

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c11`
- Citations: `approved_storyboard`, `dive_overflow`

### Code state 1

FOUR-BIT SIGNED FIELD

```text
bits     signed value
  0111   +7
+ 0001   +1
------
  ????
```

Range: −8 through +7. Which four bits result, and what do they read as under two’s complement?

- Result bits and signed reading

**Response mode:** verbal_guess

**Time:** 30 seconds

## Diagnose the rule before changing the data

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c12`
- Citations: `approved_storyboard`

- What is the field’s width and interpretation rule?
- Does the intended value or result fit that rule’s range?

Width or rule disagreement? Diagnose each observation.

- Removal sent as 1111, dashboard shows 15
- Sixteenth item makes the four-bit unsigned count show 0
- Four-bit signed 7 + 1 reads as −8

**Response mode:** verbal_guess

**Time:** 60 seconds

## Activity: read a thermostat controller’s report

- Source lineage: `it520-fall-2026-sep28-signed-overflow#sections.c13`
- Citations: `approved_storyboard`

- A small controller stores a four-bit temperature adjustment in degrees.
- It must represent warmer and cooler adjustments, so the field is two’s complement.

On the sheet:

1. Read 0110, 1010, and 1100 under both rules.
2. The adjustment is +6. A user presses warmer three times. Predict the stored bits and their signed reading.
3. Diagnose two dashboard reports as a width problem or a signedness problem.
4. Recommend one repair for each and say what agreement or range it restores.

**Response mode:** written

**Time:** 1200 seconds

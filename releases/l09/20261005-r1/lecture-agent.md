# IT 520 October 5 — How do you store a point in a computer?

- Course: IT 520
- Lecture: 06 / October 5 — IEEE 754 32-bit float
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `78eb9af104973dfb0ef9dc9d4d6ec3090d81c7e24246d3ed08c0ebcee9913930`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## How do you store a point in a computer?

- Source lineage: `lecture#sections.cover`
- Citations: `python_floating_point`

- By the end, label a 32-bit float and trace one value into it.
- Decide what is exact, then compare approximations safely.

### Diagram explanation

The question left open by a binary fraction

- Record, left to right: 3 fields
- Field 1, Decimal; value 0.1₁₀
- Field 2, Binary; value 0.0001100110011…₂
- Field 3, Stored as; value ?

## The point can move without changing the value

- Source lineage: `lecture#sections.moving_point`
- Citations: `dive_into_systems_float`, `course_records`

- Scientific notation: mantissa × base^exponent, with one nonzero leading digit.
- The exponent says where the point goes.
- In decimal: 1385.02 = 1.38502 × 10³.
- In binary the base is 2: 101.1₂ = 1.011₂ × 2².

### Diagram explanation

The labelled parts of scientific notation

- Record, left to right: 4 fields
- Field 1, Sign; value +
- Field 2, Mantissa; value 1.38502
- Field 3, Base; value 10
- Field 4, Exponent; value 3

## A 32-bit float divides the jobs three ways

- Source lineage: `lecture#sections.three_jobs`
- Citations: `dive_into_systems_float`

- Floating point: fixed bits where the binary point floats as needed.
- Single precision: the 32-bit IEEE 754 format.
- The float sign is not two's complement: 0 is positive; 1 is negative.
- Widths 1 + 8 + 23 = 32 bits; the exponent sets the range, the mantissa sets the precision.

### Diagram explanation

IEEE 754 single-precision field layout

- Record, left to right: 3 fields, 32 bits in total
- Field 1, Sign; 1 of 32 bits; bit 31
- Field 2, Exponent; 8 of 32 bits; bits 30–23
- Field 3, Mantissa (significand); 23 of 32 bits; bits 22–0

## The exponent uses a bias; the mantissa hides one bit

- Source lineage: `lecture#sections.bias_and_hidden_bit`
- Citations: `dive_into_systems_float`, `course_records`

- Biased exponent: stored exponent = true exponent + 127, so it is never negative.
- Normalized: exactly one 1 sits before the binary point.
- The leading 1 is implicit, so the mantissa stores 23 bits but holds 24 bits of precision.

### What the fields store

| field | question | kept |
| --- | --- | --- |
| Sign | Is it negative? | 0 or 1 |
| Exponent | Where does the point go? | true exponent + 127 |
| Mantissa | Which significant bits? | bits after the hidden 1 |

## The value 5.5 becomes positive binary 101.1

- Source lineage: `lecture#sections.convert_5_5`
- Citations: `course_records`

- Positive means sign bit 0.

### Build the whole and fractional parts

| part | work | bits |
| --- | --- | --- |
| 5 | 5 = 4 + 1 | 101 |
| 0.5 | 0.5 × 2 = 1.0 | .1 |
| 5.5 | join the parts | 101.1 |

## Normalizing 5.5 determines both remaining fields

- Source lineage: `lecture#sections.normalize_5_5`
- Citations: `dive_into_systems_float`, `course_records`

- Do not store the hidden 1; pad on the right to 23 bits.

### Finish the exponent and mantissa

| move | work | result |
| --- | --- | --- |
| Normalize | 101.1 → 1.011 × 2² | true exponent 2 |
| Bias | 2 + 127 = 129 = 128 + 1 | 10000001₂ |
| Mantissa | drop 1.; pad 011 on the right | 01100000000000000000000 |

## The three fields now store 5.5 exactly

- Source lineage: `lecture#sections.assemble_5_5`
- Citations: `dive_into_systems_float`, `course_records`

- Decode check: 1.011₂ × 2² = 5.5.

### Diagram explanation

The completed 32-bit encoding of 5.5

- Record, left to right: 3 fields, 32 bits in total
- Field 1, Sign; 1 of 32 bits; bit 31; value 0
- Field 2, Exponent; 8 of 32 bits; bits 30–23; value 10000001
- Field 3, Mantissa; 23 of 32 bits; bits 22–0; value 01100000000000000000000

## Can you place 6.25 into the same layout?

- Source lineage: `lecture#sections.try_6_25`
- Citations: `course_records`

Encode 6.25 as a 32-bit float

1. Join 110 (= 6) and .01 (= 0.25)
2. Normalize
3. Add the bias
4. Drop the hidden 1 and pad right

- Sign: ___
- Binary and normalized form: ___
- Exponent bits: ___
- Mantissa bits: ___

**Response mode:** written

## The same moves encode 6.25 exactly

- Source lineage: `lecture#sections.reveal_6_25`
- Citations: `dive_into_systems_float`, `course_records`

- Decode check: 1.1001₂ × 2² = 6.25.

### Diagram explanation

The completed 32-bit encoding of 6.25

- Record, left to right: 3 fields, 32 bits in total
- Field 1, Sign; 1 of 32 bits; bit 31; value 0
- Field 2, Exponent; 8 of 32 bits; bits 30–23; value 10000001
- Field 3, Mantissa; 23 of 32 bits; bits 22–0; value 10010000000000000000000

## What happens when the digits never end?

- Source lineage: `lecture#sections.repeating_digits`
- Citations: `python_floating_point`, `course_records`

- 0.1₁₀ = 0.0001100110011…₂.
- Normalized: 1.100110011…₂ × 2⁻⁴.
- The mantissa has only 23 stored bits.

Can 32 bits store 0.1 exactly?

- Yes, exactly
- No, only approximately

## A finite mantissa stores the nearest approximation

- Source lineage: `lecture#sections.nearest_approximation`
- Citations: `python_floating_point`, `course_records`

- Precision: how many significant bits the mantissa holds.
- Exact when the bits after the leading 1 end within 23 bits; otherwise the nearest value.
- 0.3 repeats too: stored as 0.300000011920928955078125.

### Value intended

- 0.1
- exact decimal value

### Bits stored

- 100 1100 1100 1100 1100 1101
- rounded to 23 stored bits

### Value represented

- 0.10000000149…
- often displayed as 0.1

| Dimension | Value intended | Bits stored | Value represented |
| --- | --- | --- | --- |
| value | 0.1 | 100 1100 1100 1100 1100 1101 | 0.10000000149… |
| meaning | exact decimal value | rounded to 23 stored bits | often displayed as 0.1 |

Displayed value and stored value can differ.

**Citations:** `python_floating_point`

## Will ten 0.1 kg items total exactly 1 kg?

- Source lineage: `lecture#sections.tote_prediction`
- Citations: `python_floating_point`

- One tote receives ten items at 0.1 kg each.
- Predict the final Boolean before running the loop.
- The code here uses Python floats.

What will the last line return?

Prediction

```python
total = 0.0
for _ in range(10):
    total += 0.1

total == 1.0
```

## Compare approximate values with math.isclose

- Source lineage: `lecture#sections.compare_with_isclose`
- Citations: `python_floating_point`

- math.isclose: compares inexact values within a small tolerance.
- A float stores sign, significant bits, and where the point goes.
- Finite significant bits make some stored values approximate.

Use exact equality for exact values; use math.isclose for expected floating-point approximation.

Result and repair

```python
import math

total = 0.0
for _ in range(10):
    total += 0.1

print(total)                    # 0.9999999999999999
print(total == 1.0)             # False
print(math.isclose(total, 1.0)) # True
```

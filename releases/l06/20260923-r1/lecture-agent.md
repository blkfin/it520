# IT 520 — What is this scanner telling us?

- Course: IT 520 — Performance and Systems
- Lecture: Week 04 / September 23 — Data representation
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `4da4bfe44352e8c44dc155b64d1906506269439b9e001e7dc3128cbbba9c70e8`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What is this scanner telling us?

- Source lineage: `it520-fall-2026-sep23-representation#sections.c01`
- Citations: `approved_storyboard`, `scanner_photo`

### Image alternative

A real handheld barcode scanner makes the device in our teaching example concrete. The lesson’s status/count report is a simplified design, not this model’s documented protocol.

Rodrigo César / Wikimedia Commons — public domain · Public domain (PD-self)

- New tote (packing bin): 0. Scan and add an item: +1. Next tote: reset to 0.
- The packing app keeps the running total and reports it to the server.
- Busy, 13 means: filling this tote; 13 items recorded so far.

## One bit has two values; the scanner has four states

- Source lineage: `it520-fall-2026-sep23-representation#sections.c02`
- Citations: `approved_storyboard`, `dive_binary`

### Four scanner states

- Ready
- Busy
- Offline
- Fault

### Two possible values for one bit

- 0
- 1

**Citations:** `approved_storyboard`

- A bit is a binary digit: 0 or 1.
- The scanner has exactly one status at a time. Busy means the worker is filling the tote. One bit cannot distinguish all four states.

## Two bits give each of the four states a pattern

- Source lineage: `it520-fall-2026-sep23-representation#sections.c03`
- Citations: `approved_storyboard`, `dive_binary`

| Pattern | Agreed state |
| --- | --- |
| 00 | Ready |
| 01 | Busy |
| 10 | Offline |
| 11 | Fault |

- Scanner is Busy → sends 01 → server looks up 01 → recovers Busy.
- A codebook is an agreed mapping from patterns to meanings.

## A fifth state needs a fifth distinct pattern

- Source lineage: `it520-fall-2026-sep23-representation#sections.c04`
- Citations: `approved_storyboard`

| Two-bit pattern | State |
| --- | --- |
| 00 | Ready |
| 01 | Busy |
| 10 | Offline |
| 11 | Fault |
| ? | Updating |

- A field is a group of bits reserved for one piece of information. Its width is its number of bits.
- Can this two-bit status field distinguish five states?

## One more bit doubles the patterns

- Source lineage: `it520-fall-2026-sep23-representation#sections.c05`
- Citations: `approved_storyboard`, `dive_binary`

| Existing two bits | Add a leading 0 | Add a leading 1 |
| --- | --- | --- |
| 00 | 000 | 100 |
| 01 | 001 | 101 |
| 10 | 010 | 110 |
| 11 | 011 | 111 |

- Two bits: 2 × 2 = 2² = 4 patterns.
- Three bits: 2 × 2 × 2 = 2³ = 8 patterns.
- Each bit has two choices. For n bits, there are 2ⁿ patterns.

## Three bits can distinguish all five states

- Source lineage: `it520-fall-2026-sep23-representation#sections.c06`
- Citations: `approved_storyboard`

| Three-bit pattern | State |
| --- | --- |
| 000 | Ready |
| 001 | Busy |
| 010 | Offline |
| 011 | Fault |
| 100 | Updating |
| 101, 110, 111 | Not assigned |

- Five states use five of the eight patterns. Three patterns remain available.

## Position changes what a digit contributes

- Source lineage: `it520-fall-2026-sep23-representation#sections.c07`
- Citations: `approved_storyboard`, `dive_bases`

- The app has recorded 13 items in the current tote. In decimal, that is 1 ten and 3 ones.

| Number | Tens (×10) | Ones (×1) | Total |
| --- | --- | --- | --- |
| 13 | 1 × 10 = 10 | 3 × 1 = 3 | 10 + 3 = 13 |
| 31 | 3 × 10 = 30 | 1 × 1 = 1 | 30 + 1 = 31 |

- Positional notation: digit × place value, then add.
- Decimal places grow by ten moving left: … 100, 10, 1. Binary uses the same idea, with places that grow by two.

## Binary place value reads 1101 as thirteen

- Source lineage: `it520-fall-2026-sep23-representation#sections.c08`
- Citations: `approved_storyboard`, `dive_bases`

| Place value | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- |
| Bit | 1 | 1 | 0 | 1 |
| Contribution | 1 × 8 = 8 | 1 × 4 = 4 | 0 × 2 = 0 | 1 × 1 = 1 |

- 1101 (binary) = 8 + 4 + 0 + 1 = 13 (decimal).
- Unsigned means zero or positive. Four bits give 16 patterns: the values 0 through 15.
- Your turn: read 1010 using the same place values.

## One report carries status and the running tote count

- Source lineage: `it520-fall-2026-sep23-representation#sections.c09`
- Citations: `approved_storyboard`

### Image alternative

The app packs scanner status Busy and the current tote’s running total, thirteen recorded items, into one byte: status bits 7–5, spare zero at bit 4, and tote count at bits 3–0. A new tote resets the count.

## Hex writes the same eight bits as two characters

- Source lineage: `it520-fall-2026-sep23-representation#sections.c10`
- Citations: `approved_storyboard`, `dive_conversion`

### Image alternative

Hex 2 spans 0010 and hex D spans 1101; the lookup table supplies every four-bit group.

## Read the report: expand, separate, interpret

- Source lineage: `it520-fall-2026-sep23-representation#sections.c11`
- Citations: `approved_storyboard`

### Image alternative

Three numbered steps expand 2D into 0010 1101, separate the status 001 from the spare 0 and count 1101, then interpret status with the codebook and count using 8,4,2,1. The result is scanner Busy, with thirteen items recorded so far in the current tote.

## Add a zone label: how many bits are enough?

- Source lineage: `it520-fall-2026-sep23-representation#sections.c12`
- Citations: `approved_storyboard`

- The server now also needs the warehouse zone where the worker is filling the current tote.
- There are 19 zones. Each report names one zone, so each zone needs a different bit pattern.

| Candidate field width | Distinct patterns available |
| --- | --- |
| 4 bits | 2⁴ = 16 |
| 5 bits | 2⁵ = 32 |

Choose the smallest width that can distinguish all 19 zones. Explain your comparison.

- Width and reason

**Response mode:** verbal_guess

**Time:** 45 seconds

## Read a report. Size a new field.

- Source lineage: `it520-fall-2026-sep23-representation#sections.c13`
- Citations: `approved_storyboard`

| Task | What to do | Today’s result |
| --- | --- | --- |
| Read an existing report | Expand hex → locate fields → apply each rule | 2D → Busy; 13 items recorded in this tote |
| Design a new field | Count required meanings → choose enough patterns | 19 zones → 5 bits, 32 patterns |

- To read a field, use its position, width, and rule.
- On the practice sheet: A reads new reports; B compares rules; C sizes a field for a larger count.

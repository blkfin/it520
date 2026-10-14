# IT 520: Invariants and compression

- Course: IT 520
- Lecture: 07 / October 14
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `874977e0a67ddd89547e7b2479b852faff335521019d4e5a70c64d6e4fea4e7b`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What must stay true when we make data smaller?

- Source lineage: `it520-oct14-invariants-compression#sections.cover`
- Citations: `seed`

Count bit patterns, check an encoder, measure compression, and decide when it helps.

**Citations:** `seed`

| Question | Requirement |
| --- | --- |
| Recovery | Does the decoded data match? |
| Size | How much did the representation shrink? |
| Time | Did shrinking it make delivery faster? |

**Citations:** `seed`

## How many patterns can six bits hold?

- Source lineage: `it520-oct14-invariants-compression#sections.bits_predict`
- Citations: `seed`

- **Bit**: A value that is either 0 or 1.

**Citations:** `seed`

| Bits | Patterns |
| --- | --- |
| 1 | 0, 1 |
| 2 | 00, 01, 10, 11 |
| 6 | [blank] |

**Citations:** `seed`

## Each extra bit doubles the possible patterns

- Source lineage: `it520-oct14-invariants-compression#sections.bits_double`
- Citations: `seed`

This doubling will tell us whether every file can be made smaller.

**Citations:** `csfg`, `seed`

| Number of bits | Number of patterns |
| --- | --- |
| 1 | 2 |
| 2 | 4 |
| 3 | 8 |
| 4 | 16 |
| 5 | 32 |
| 6 | 64 |

**Citations:** `seed`

## Two hex digits describe one eight-bit byte

- Source lineage: `it520-oct14-invariants-compression#sections.hex_worked`
- Citations: `seed`

Hexadecimal uses 16 symbols: 0 to 9, then A to F.

**Citations:** `seed`

One byte in hexadecimal and binary

```text
0x3A = 3 × 16 + 10 × 1 = 58
0011 1010 = 32 + 16 + 8 + 2 = 58
```

**Citations:** `seed`

One byte has eight bits; one hex digit represents four bits.

**Citations:** `seed`

## Compression sends a smaller representation of the same file

- Source lineage: `it520-oct14-invariants-compression#sections.recovery_contract`
- Citations: `csfg`, `seed`

We can count file bytes. Can we represent the same contents using fewer of them?

**Citations:** `csfg`, `seed`

Compression represents data using fewer bytes to save storage or transfer work. The receiver decodes that representation to recover the data.

**Citations:** `csfg`, `seed`

- **Lossless compression**: The recovered bytes are identical to the original bytes.

**Citations:** `csfg`, `seed`

### Diagram explanation

Encode a log, send its smaller representation, and decode it at the receiver.

- Arrow from Original log to Smaller representation: Encode: represent compactly
- Arrow from Smaller representation to Recovered log: identical bytes: Send, then decode: recover

**Citations:** `csfg`, `seed`

## How we measure compression

- Source lineage: `it520-oct14-invariants-compression#sections.compression_measure`
- Citations: `seed`

| Term | Meaning |
| --- | --- |
| Original size | Bytes before compression |
| Compressed size | All output bytes, including decoding information |
| Compression ratio | compressed size / original size |
| Space savings | (original size - compressed size) / original size |

**Citations:** `seed`

Our 10 MB log becomes 2 MB

```text
ratio = 2 / 10 = 0.20
savings = (10 - 2) / 10 = 0.80 = 80%
```

**Citations:** `seed`

Ratio below 1: smaller. Ratio above 1: larger. Savings is the fraction of original bytes removed.

**Citations:** `csfg`, `seed`

## Run-length encoding replaces repetitions with count and symbol

- Source lineage: `it520-oct14-invariants-compression#sections.rle_worked`
- Citations: `csfg`, `seed`

- **Run-length encoding (RLE)**: Replace each consecutive group of identical symbols with its count and symbol.

**Citations:** `csfg`, `seed`

Model repetition in the log with a short toy sequence. Each letter is one byte; each run uses one count byte and one symbol byte. The count 6 is the byte 0x06 in hex.

**Citations:** `csfg`, `seed`

Encode from left to right

```text
input: AAAAAABBBBCCCCDD
6 A → (6,A)     4 B → (4,B)
4 C → (4,C)     2 D → (2,D)
original size: 6 + 4 + 4 + 2 = 16 bytes
compressed size: 4 records × 2 = 8 bytes
ratio: 8 / 16 = 0.5; savings: 50%
```

**Citations:** `csfg`, `seed`

## Decode by repeating each symbol its stored count

- Source lineage: `it520-oct14-invariants-compression#sections.rle_decode`
- Citations: `seed`

Eight bytes is smaller than sixteen. Check that the smaller records recover every original letter.

**Citations:** `seed`

Decode records from left to right

```text
(6,A) → AAAAAA
(4,B) → BBBB
(4,C) → CCCC
(2,D) → DD
join: AAAAAABBBBCCCCDD
original: AAAAAABBBBCCCCDD
```

**Citations:** `seed`

## An invariant checks recovery after every encoded run

- Source lineage: `it520-oct14-invariants-compression#sections.rle_checkpoint`
- Citations: `cornell`, `seed`

The full decode matched. During encoding, checkpoint comparisons locate the first faulty record and let us stop there.

**Citations:** `cornell`, `seed`

- **Invariant**: A condition that must hold at every chosen checkpoint.

**Citations:** `seed`

Before starting

```text
original consumed: empty
records: none
decoded records: empty
```

After the first run is recorded

```text
original consumed: AAAAAA
record: (6,A)
decoded record: AAAAAA
```

**Citations:** `cornell`, `seed`

At each checkpoint: decoded records equal the original input consumed through this run. At the end: decoded output equals the whole original input.

**Citations:** `cornell`, `seed`

## Which record first breaks the recovery invariant?

- Source lineage: `it520-oct14-invariants-compression#sections.rle_try`
- Citations: `seed`

The encoder miscounted a run. Compare decoded records with the original input consumed so far.

**Citations:** `seed`

Records written by the faulty encoder

```text
input:   AAAAAABBBBCCCCDD
records: (5,A),(4,B),(4,C),(2,D)
check after each recorded run
```

**Citations:** `seed`

Decode the first record, compare it with the first input run, and identify the bad count.

**Response mode:** verbal_guess

**Citations:** `seed`

## A checkpoint locates the bad step; a final check tests the whole result

- Source lineage: `it520-oct14-invariants-compression#sections.rle_answer`
- Citations: `seed`

The first record fails. Repair its count before continuing to encode the remaining runs.

**Citations:** `seed`

| Check | Result |
| --- | --- |
| First checkpoint | Original first run: AAAAAA; decoded (5,A): AAAAA. Failure. |
| Repair | Replace (5,A) with (6,A) |
| Invariant | After every recorded run: decoded records = original input consumed through this run |
| Final recovery | Checked at the end: decoded output = whole original input |

**Citations:** `seed`

## Correct RLE can still make a file larger

- Source lineage: `it520-oct14-invariants-compression#sections.rle_expansion`
- Citations: `seed`

We can check exact recovery. Compression also needs fewer bytes: repetitions helped the first input, but ABAB has no repeated neighbours.

**Citations:** `seed`

| Check | Result |
| --- | --- |
| Original input | ABAB: 4 bytes |
| Records | (1,A),(1,B),(1,A),(1,B) |
| Decoded output | ABAB: exact recovery |
| Compressed size | 4 records × 2 = 8 bytes |
| Compression ratio | 8 / 4 = 2.0: expansion |

**Citations:** `seed`

## Can every byte shrink?

- Source lineage: `it520-oct14-invariants-compression#sections.universal_predict`
- Citations: `rfc`, `seed`

- **Code**: The complete compressed bit string for one original byte. Opening-table doubling: 8 bits give 256 inputs.

**Citations:** `rfc`, `seed`

| Code length (bits) | Codes of that length |
| --- | --- |
| 0 (empty string) | 1 |
| 1 | 2 |
| 2 | 4 |
| 3 | 8 |
| 4 | 16 |
| 5 | 32 |
| 6 | 64 |
| 7 | 128 |
| Total shorter codes | [blank] |

**Citations:** `rfc`, `seed`

## Lossless compression cannot make every input smaller

- Source lineage: `it520-oct14-invariants-compression#sections.universal_answer`
- Citations: `rfc`, `seed`

Count all shorter possibilities

```text
8-bit inputs: 2⁸ = 256
shorter codes: 1 + 2 + 4 + 8
               + 16 + 32 + 64 + 128 = 255
```

**Citations:** `rfc`, `seed`

There are fewer shorter codes than inputs. Reusing one code for two inputs leaves the decoder unable to tell which original to recover.

**Citations:** `rfc`, `seed`

## Lossy compression trades exact recovery for acceptable quality

- Source lineage: `it520-oct14-invariants-compression#sections.recovery_choices`
- Citations: `csfg`, `seed`

No lossless method shrinks every file. A photo preview still needs to be small. Lossy compression keeps an approximation with enough detail for the receiver, discarding fine detail to reduce size.

**Citations:** `csfg`, `seed`

- **Lossy compression**: An encoding that discards information and does not promise exact recovery.

**Citations:** `csfg`, `seed`

| File | Required recovery |
| --- | --- |
| Signed contract | Identical original bytes |
| Photo preview | Looks acceptable within a stated quality limit |

**Citations:** `csfg`, `seed`

## Does a smaller lossless log arrive sooner?

- Source lineage: `it520-oct14-invariants-compression#sections.transfer_predict`
- Citations: `seed`

First choose the recovery requirement. For this log, exact recovery is required. Even a smaller lossless file may take longer to deliver. Its complete size shrinks from 10 MB to 2 MB. Encoding takes 1 s; decoding takes 0.5 s.

**Citations:** `seed`

Transfer seconds = size in MB / speed in MB/s. Compressed total = encode + transfer + decode. MB means decimal megabytes.

**Citations:** `seed`

| Route | 2 MB/s | 20 MB/s |
| --- | --- | --- |
| Send original 10 MB | 10 / 2 = 5 s | [blank] |
| Encode, send 2 MB, decode | 1 + (2 / 2) + 0.5 = 2.5 s | [blank] |

**Citations:** `seed`

## Send the original when processing costs exceed time saved

- Source lineage: `it520-oct14-invariants-compression#sections.transfer_choices`
- Citations: `seed`

The fast connection saves little sending time. Encoding and decoding still take the same time, so the smaller output arrives later.

**Citations:** `seed`

| Route | 2 MB/s | 20 MB/s |
| --- | --- | --- |
| Send original 10 MB | 10 / 2 = 5 s | 10 / 20 = 0.5 s |
| Encode, send 2 MB, decode | 1 + (2 / 2) + 0.5 = 2.5 s | 1 + (2 / 20) + 0.5 = 1.6 s |

**Citations:** `seed`

## Choose by required recovery, complete size, then total time

- Source lineage: `it520-oct14-invariants-compression#sections.close`
- Citations: `seed`

| File | Requirement and evidence | Route |
| --- | --- | --- |
| Log at 2 MB/s | Exact bytes, verified; compressed 2.5 s, original 5 s | [blank] |
| Log at 20 MB/s | Exact bytes, verified; compressed 1.6 s, original 0.5 s | [blank] |
| Photo preview | Looks acceptable within a stated quality limit | [blank] |
| Signed contract | Identical original bytes required | [blank] |

**Citations:** `seed`

Choose lossless or no compression for each log. For the other files, name the allowed recovery promises and the evidence still needed.

**Response mode:** verbal_guess

**Citations:** `seed`

## Preserve what is required, then check size and total time

- Source lineage: `it520-oct14-invariants-compression#sections.close_answer`
- Citations: `seed`

| File | Requirement and evidence | Route |
| --- | --- | --- |
| Log at 2 MB/s | Exact bytes, verified; compressed 2.5 s, original 5 s | Lossless compression |
| Log at 20 MB/s | Exact bytes, verified; compressed 1.6 s, original 0.5 s | No compression |
| Photo preview | Looks acceptable within a stated quality limit | Lossy within quality limit; costs still needed |
| Signed contract | Identical original bytes required | Exact bytes: lossless or unchanged; costs still needed |

**Citations:** `seed`

Required recovery → complete size → total time.

**Citations:** `seed`

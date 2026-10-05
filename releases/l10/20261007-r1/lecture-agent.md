# IT 520: text and files

- Course: IT 520
- Lecture: 10 / Text and files
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `82d55f9dde2a4470e61388ff8c79a4ebeef54da18cb9463a0f1ae7a0df724933`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What must two programs agree on to read the same file?

- Source lineage: `lecture#sections.cover`
- Citations: none

By the end, you can predict text's byte length, diagnose a failed read, and separate a file's name from its contents.

## Can an unchanged file show the wrong text?

- Source lineage: `lecture#sections.card_01`
- Citations: none

- A warehouse exports tote 7's packing record.
- The receiving site opens the same file.
- The destination looks wrong.

### Packing app

- File: tote7.csv
- Destination: Montréal

### Receiving app

- File: tote7.csv
- Destination: MontrÃ©al

## A reader gives the bytes their meaning

- Source lineage: `lecture#sections.card_02`
- Citations: none

- File: an ordered sequence of bytes.
- ASCII: a character set with 128 values.

| Same byte | Reader's rule | Interpretation |
| --- | --- | --- |
| 5A | Unsigned integer | 90 |
| 5A | ASCII text | Z |

## Unicode gives characters shared numbers across writing systems

- Source lineage: `lecture#sections.unicode_open`
- Citations: none

- ASCII covers 128 values, including English letters and controls.
- Our destination needs é; other text needs symbols and emoji.
- Unicode: a shared standard assigning numbers to characters.
- Both programs can identify characters beyond ASCII.

## Unicode assigns numbers to the characters we send

- Source lineage: `lecture#sections.card_03`
- Citations: none

- Character set: characters and their assigned numbers.
- Code point: a character's assigned Unicode number.

| Character | Code point (hex) | Number (decimal) |
| --- | --- | --- |
| A | U+0041 | 65 |
| é | U+00E9 | 233 |
| € | U+20AC | 8364 |

## UTF-8 stores Unicode text compactly while preserving ASCII

- Source lineage: `lecture#sections.utf8_open`
- Citations: none

- Unicode gives character numbers; a file must store bytes.
- Encoding: a rule for storing character numbers as bytes.
- UTF-8 can encode every Unicode character.
- ASCII text is already valid UTF-8, with identical bytes.
- Many common characters need only one or two bytes.

## UTF-8 uses different byte counts for different numbers

- Source lineage: `lecture#sections.card_04`
- Citations: none

- Encoding: rules that turn Unicode text into bytes.
- UTF-8: an encoding using one to four bytes per code point.

| Code point range (hex) | Byte pattern | Bytes |
| --- | --- | --- |
| U+0000 to U+007F | 0xxxxxxx | 1 |
| U+0080 to U+07FF | 110xxxxx 10xxxxxx | 2 |
| U+0800 to U+FFFF | 1110xxxx 10xxxxxx 10xxxxxx | 3 |
| U+10000 to U+10FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx | 4 |

## Montréal takes nine bytes when encoded in UTF-8

- Source lineage: `lecture#sections.card_05`
- Citations: none

Look up each code point, choose its byte count, then add.

| Text | Code points (U+hex) | UTF-8 bytes (hex) | Byte count |
| --- | --- | --- | --- |
| Montr | 004D 006F 006E 0074 0072 | 4D 6F 6E 74 72 | 5 × 1 = 5 |
| é | 00E9 | C3 A9 | 1 × 2 = 2 |
| al | 0061 006C | 61 6C | 2 × 1 = 2 |
| Montréal | 8 code points | 4D 6F 6E 74 72 C3 A9 61 6C | 5 + 2 + 2 = 9 |

## How many UTF-8 bytes do A, é and € need together?

- Source lineage: `lecture#sections.card_06`
- Citations: none

- U+0000 to U+007F: 1 byte
- U+0080 to U+07FF: 2 bytes
- U+0800 to U+FFFF: 3 bytes

| Character | Code point (hex) | Bytes |
| --- | --- | --- |
| A | U+0041 | [blank] |
| é | U+00E9 | [blank] |
| € | U+20AC | [blank] |
| Aé€ | Total | [blank] |

## Three characters can occupy six UTF-8 bytes

- Source lineage: `lecture#sections.card_07`
- Citations: none

Byte count depends on which characters the text contains.

| Text | UTF-8 bytes (hex) | Bytes |
| --- | --- | --- |
| A | 41 | 1 |
| é | C3 A9 | 2 |
| € | E2 82 AC | 3 |
| Aé€ | 41 C3 A9 E2 82 AC | 1 + 2 + 3 = 6 |

## The wrong encoding garbles an unchanged destination

- Source lineage: `lecture#sections.card_08`
- Citations: none

Decoding: turning bytes back into text using an encoding rule.

| Destination bytes (hex) | Reader uses | Displayed text |
| --- | --- | --- |
| 4D 6F 6E 74 72 C3 A9 61 6C | UTF-8 | Montréal |
| 4D 6F 6E 74 72 C3 A9 61 6C | Windows-1252 | MontrÃ©al |

## CSV groups decoded characters into records and fields

- Source lineage: `lecture#sections.csv_open`
- Citations: none

- We can decode bytes into characters.
- The packing app must group characters into usable data.
- Record: related values describing one entry, such as tote 7.
- Field: one value in a record, such as its destination.
- CSV: text records using commas to separate fields.

## CSV quotes keep an item's comma inside its field

- Source lineage: `lecture#sections.card_09`
- Citations: none

- Format: the agreement about how file bytes are organized.
- CSV: text records with fields separated by commas.

Without quotes: 7 | Mug | blue | Montréal (4 fields)

tote7.csv

```text
tote,item,dest
7,"Mug, blue",Montréal
```

## Renaming a PNG does not make it CSV

- Source lineage: `lecture#sections.card_10`
- Citations: none

- Extension: the filename's label after the dot.
- File signature: fixed leading bytes that identify a format.

| Name | First eight bytes (hex) | Format indicated |
| --- | --- | --- |
| label.png | 89 50 4E 47 0D 0A 1A 0A | PNG |
| tote7-label.csv | 89 50 4E 47 0D 0A 1A 0A | PNG |

## SHA-256 computes a repeatable digest from every file byte

- Source lineage: `lecture#sections.sha_open`
- Citations: none

- Names and reading rules cannot tell us whether bytes changed.
- Digest: a fixed-length result calculated from a file’s bytes.
- The same bytes always produce the same SHA-256 digest.
- Changing tote 7 to 8 scrambles this file’s digest.

### Diagram explanation

Every file byte goes through the same SHA-256 recipe to produce a digest

- Arrow from Every file byte to SHA-256: fixed public recipe
- Arrow from SHA-256: fixed public recipe to 256-bit digest (64 hex digits)

## Which change should alter the file's SHA-256 digest?

- Source lineage: `lecture#sections.card_11`
- Citations: none

SHA-256 digest: a fixed-length fingerprint computed from a file's bytes.

| Action on tote7.csv | Bytes | Digest changes? |
| --- | --- | --- |
| Rename to received.csv | [blank] | [blank] |
| Edit tote digit from 7 to 8 | 37 becomes 38 (hex) | [blank] |

## A rename preserves the digest; this edit changes it

- Source lineage: `lecture#sections.card_12`
- Citations: none

A matching trusted digest is evidence that the bytes are unchanged.

| File | Change | SHA-256 prefix |
| --- | --- | --- |
| tote7.csv | Original export | cac43431… |
| received.csv | Rename only | cac43431… |
| tote7.csv (edited) | One byte: 37 becomes 38 | 02d26492… |

## Readers need matching bytes, encoding rules and format rules

- Source lineage: `lecture#sections.card_13`
- Citations: none

Agree on the bytes, the encoding and the format.

| Observed problem | Evidence | Diagnosis |
| --- | --- | --- |
| MontrÃ©al appears | Digest matches the sender's; wrong decoding reproduces it | Wrong encoding |
| A CSV reader cannot read the label | 89 50 4E 47 0D 0A 1A 0A indicates PNG | Wrong format |
| Tote 7 becomes tote 8 | Trusted full digests differ; 37 became 38 | Changed bytes |

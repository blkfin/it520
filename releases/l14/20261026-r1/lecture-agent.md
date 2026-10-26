# IT 520 - Foundations of Information Technology — Week 9 — Byte order and caching

- Course: IT 520 - Foundations of Information Technology
- Lecture: Week 9 / Byte order and caching
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `b2b6728f6a88d6ae7d0e7e33029a9db7d205e66132491411d8e06ac4b0fec2d6`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## A number's bytes: where do they sit, and how fast are they read?

- Source lineage: `lecture#sections.c01_cover`
- Citations: none

Today you will learn to place a number's bytes in both orders, say why the order matters, explain what a cache is for, and predict which of two reading orders is faster.

### Byte order

- Which byte of a number goes first in memory?

### Caching

- Why can the order we read memory change the time?

## 0xAABBCCDD splits into four bytes, highest place to lowest

- Source lineage: `lecture#sections.c02_four_bytes`
- Citations: `dis_4_7`

- **Most significant byte**: the byte in the highest place (AA).
- **Least significant byte**: the byte in the lowest place (DD).

Significant means place, not size: DD (221) is a bigger number than AA (170).

### Diagram explanation

The four bytes of 0xAABBCCDD

- Record, left to right: 4 fields
- Field 1, AA; worth ×256³
- Field 2, BB; worth ×256²
- Field 3, CC; worth ×256
- Field 4, DD; worth ×1

## Big-endian and little-endian store the same bytes in opposite orders

- Source lineage: `lecture#sections.c03_two_orders`
- Citations: `dis_4_7`

- Address: the number naming one memory box. Each box holds one byte.
- Big-endian: most significant byte at the lowest address.
- Little-endian: least significant byte at the lowest address.

### One value, 0xAABBCCDD, in two orders

| Address | Big-endian | Little-endian |
| --- | --- | --- |
| X | AA | DD |
| X+1 | BB | CC |
| X+2 | CC | BB |
| X+3 | DD | AA |

## Where does each byte of 0x12345678 go in each order?

- Source lineage: `lecture#sections.c04_try_placing`
- Citations: none

Lowest address: X.

| Address | Big-endian | Little-endian |
| --- | --- | --- |
| X | [blank] | [blank] |
| X+1 | [blank] | [blank] |
| X+2 | [blank] | [blank] |
| X+3 | [blank] | [blank] |

## The bytes arrived intact. How is the number different?

- Source lineage: `lecture#sections.c05_predict_receiver`
- Citations: none

- A little-endian machine sends these bytes, lowest address first.
- A big-endian machine receives them in that order.

### 0x12345678 in two orders

| Address | Big-endian | Little-endian |
| --- | --- | --- |
| X | 12 | 78 |
| X+1 | 34 | 56 |
| X+2 | 56 | 34 |
| X+3 | 78 | 12 |

## The receiver read the same bytes as a different number

- Source lineage: `lecture#sections.c06_receiver_reads`
- Citations: `dis_4_7`

The four bytes alone do not say which order they are in.

| Machine | Reads the bytes as | Decimal value |
| --- | --- | --- |
| Sender (little-endian) | 0x12345678 | 305,419,896 |
| Receiver (big-endian) | 0x78563412 | 2,018,915,346 |

## Machines that share data must agree on one byte order

- Source lineage: `lecture#sections.c07_agree_on_order`
- Citations: `dis_4_7`

- On one machine, you rarely notice byte order.
- It shows up when raw bytes are inspected or sent between machines.

- **Network byte order**: the order agreed for the internet, big-endian.

### Diagram explanation

Both machines convert to and from one agreed order

- Arrow from Sender (little-endian) to Message on the network (big-endian: 12 34 56 78): converts to network order
- Arrow from Message on the network (big-endian: 12 34 56 78) to Receiver (any machine): converts from network order

## A cache keeps copies near the CPU because memory is slow

- Source lineage: `lecture#sections.c08_cache_near_cpu`
- Citations: `dis_11_2`, `dis_11_4`

- We can now find any byte by its address.
- But each trip to memory is slow next to the CPU.

- **Cache**: small, fast storage near the CPU that holds copies of memory data.

### Diagram explanation

The CPU asks the cache first and goes to memory only when the copy is missing

- Arrow from CPU (you, working) to Cache (the desk): a few photocopied pages, about 5 ns: asks here first
- Arrow from Cache (the desk): a few photocopied pages, about 5 ns to Memory (the library): all the originals, about 100 ns: only if the copy is missing

**Caveat:** Times typical of a 2020 desktop.

## Every read checks the cache first, then memory if needed

- Source lineage: `lecture#sections.c09_hit_or_miss`
- Citations: `dis_11_4`

- **Hit**: the requested data is already in the cache.
- **Miss**: it is not, so it must come from memory.

### Hit

- Served from the cache
- About 5 ns
- Memory is not touched

### Miss

- Copied from memory into the cache
- About 100 ns
- The next read of it can hit

## Programs reuse data and use its neighbors, so copies pay off

- Source lineage: `lecture#sections.c10_locality`
- Citations: `dis_11_3`

### Temporal locality

- Using the same data again soon
- Example: a running total, updated at every step
- Example: a counter checked every time through a loop

### Spatial locality

- Using data stored next to data just used
- Example: the next item in a list, stored right after this one
- Example: the next character in a line of text

## A miss copies a whole block, so its neighbors then hit

- Source lineage: `lecture#sections.c11_block_copy`
- Citations: `dis_11_3`, `dis_11_4`

- **Block**: a fixed-size run of neighboring bytes, copied together. Here, 16 bytes.

- The list holds four 4-byte integers, side by side.

### Empty cache, 16-byte blocks: one trip to memory serves four reads

| Read | Its addresses | Result |
| --- | --- | --- |
| Item 0 | 0 to 3 | Miss: copy bytes 0 to 15 |
| Item 1 | 4 to 7 | Hit |
| Item 2 | 8 to 11 | Hit |
| Item 3 | 12 to 15 | Hit |

## A full cache drops an old copy to make room

- Source lineage: `lecture#sections.c12_full_cache`
- Citations: `dis_11_4`

- The cache holds far less than memory.
- Here, a full cache drops the least recently used copy.
- We only read here, so memory still has the original.

### A cache with room for two blocks

| Read | Slot 1 | Slot 2 | Row mark |
| --- | --- | --- | --- |
| Block A | A | empty |  |
| Block B | A | B |  |
| Block C | C (A dropped) | B | highlighted row |

## Same values, same sum: why can visit order change the time?

- Source lineage: `lecture#sections.c13_predict_order`
- Citations: `dis_11_3`

- Each row is one 16-byte block. The cache holds two blocks.
- Start each order with an empty cache.

### Four rows of four integers, stored row by row

| Row | Column 0 | Column 1 | Column 2 | Column 3 |
| --- | --- | --- | --- | --- |
| Row 0 (bytes 0 to 15) | 1 | 2 | 3 | 4 |
| Row 1 (bytes 16 to 31) | 5 | 6 | 7 | 8 |
| Row 2 (bytes 32 to 47) | 9 | 10 | 11 | 12 |
| Row 3 (bytes 48 to 63) | 13 | 14 | 15 | 16 |

## Row order misses 4 times; column order misses every time

- Source lineage: `lecture#sections.c14_miss_counts`
- Citations: `dis_11_2`, `dis_11_3`

- Row order: 4 × 100 + 12 × 5 = 460 ns. The 4 misses take 400 of it.
- Column order: 16 × 100 = 1,600 ns.
- Column order, when 2 is needed: the cache holds rows 2 and 3, so row 0 is gone.

### Diagram explanation

Column order, when 2 is needed next: the two slots hold rows 2 and 3; row 0, which holds 2, was dropped

- Record, left to right: 2 fields
- Field 1, Slot 1; 9, 10, 11, 12; value Row 2
- Field 2, Slot 2; 13, 14, 15, 16; value Row 3

## Real caches also decide where copies sit and when changes return

- Source lineage: `lecture#sections.c15_placement_and_writes`
- Citations: `dis_11_4`

- Where a copy may sit: in any slot, as in our two-slot cache, or in one fixed slot.
- A write changes the cached copy, so memory is older until the change goes back.

- **Write-through**: the change goes to memory right away.
- **Write-back**: the change goes to memory when the changed copy is dropped.

### Diagram explanation

A write changes the copy in the cache; memory gets the change later or at once

- Arrow from CPU to Cache (changed copy): a write changes the copy
- Arrow from Cache (changed copy) to Memory (older until updated): now or when dropped

## The order we use memory changes the value and the speed

- Source lineage: `lecture#sections.c16_order_matters`
- Citations: `dis_4_7`, `dis_11_3`

The same bytes can give a different value, or take a different time, depending on order.

### Byte order

- Decides which value the bytes mean
- Sender and receiver must agree
- Network byte order is big-endian

### Reading order

- Decides how often the cache misses
- Reuse and neighbors make hits
- A few misses can take most of the time

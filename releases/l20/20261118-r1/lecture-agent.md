# What counts as different, and when must two share?

- Course: IT 520
- Lecture: week-12 / Counting choices and forced sharing
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `45ecd9be115924e5738cd7e1198054e585c9e1f6039c56f4879a9c57b0b73bcc`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What counts as different? When must two share?

- Source lineage: `lecture#sections.cover`
- Citations: none

By the end, you can tell a set from a schedule before counting, and explain why more items than containers forces sharing.

## Does swapping A and B make a different maintenance job?

- Source lineage: `lecture#sections.card01_swap_question`
- Citations: `levin_dmoi_4e`

- Five devices, A to E. Tonight the crew services three of them, each once.
- Question: if we swap A and B, is it a different job?
- Answer it for each record below.

### Set: which three

- A, B, C
- Swapped: B, A, C

### Schedule: which three, in what order

- First A, second B, third C
- Swapped: first B, second A, third C

## Swapping changes a schedule but not a set

- Source lineage: `lecture#sections.card02_swap_changes`
- Citations: `levin_dmoi_4e`

- Combination: a set; rewriting the names in another order gives the same set.
- Permutation: a schedule; swapping positions makes a new one.

| Record | What a swap changes | Same job? |
| --- | --- | --- |
| Set: A, B, C | Only the order the names were written | Yes: the same three devices |
| Schedule: A, B, C | Which device is serviced first | No: a different first job |

## Ten or sixty: which count answers which question?

- Source lineage: `lecture#sections.card03_ten_or_sixty`
- Citations: `levin_dmoi_4e`

Same five devices, three serviced tonight. Match each question to its count: 10 or 60.

- Set: how many ways to choose which three devices?
- Schedule: how many ways to choose three devices and their order?

**Response mode:** discussion

## Both are right: sixty schedules group into ten sets

- Source lineage: `lecture#sections.card04_sixty_into_ten`
- Citations: `levin_dmoi_4e`

- Stop after three slots: all five in order would be 5! = 120.
- Check: 10 × 6 = 60. Written P(5,3) = 60, C(5,3) = 10.

| Step | Count |
| --- | --- |
| First slot | 5 devices |
| Second slot | 4 left |
| Third slot | 3 left |
| Schedules (order counts) | 5 × 4 × 3 = 60 |
| Orders of one set of three | 3 × 2 × 1 = 6 |
| Sets (order does not count) | 60 ÷ 6 = 10 |

## Two of six devices: how many sets? How many schedules?

- Source lineage: `lecture#sections.card05_two_of_six`
- Citations: `levin_dmoi_4e`

Six devices. Tonight the crew services two of them, each once. Answer both questions.

1. Describe one outcome in words.
2. Ask whether a swap changes it.
3. Count, then check: sets × orders = schedules.

- Set: how many ways to choose which two devices?
- Schedule: how many ways to choose a first job and a second job?

**Response mode:** written

## Six devices give 30 schedules and 15 sets

- Source lineage: `lecture#sections.card06_thirty_and_fifteen`
- Citations: `levin_dmoi_4e`

- Not 6 × 6: that allows the same device twice.
- Not 6!: that fills six slots, not two.

| Record | Count | Why |
| --- | --- | --- |
| Schedule: first, second | 6 × 5 = 30 | 6 choices, then 5 left |
| Set of two | 30 ÷ 2 = 15 | Each pair has 2 orders |
| Check | 15 × 2 = 30 | Sets times orders gives schedules |

## 13 students, 12 birth months: must two share a month?

- Source lineage: `lecture#sections.card07_thirteen_students`
- Citations: `levin_dmoi_4e`

- New kind of question: not how many ways, but what must happen.
- Task: try to give all 13 students different birth months.
- If no arrangement works, two must share. That is a guarantee.

### Diagram explanation

Twelve empty month boxes (Jan to Jun)

- Record, left to right: 6 fields
- Field 1, Jan
- Field 2, Feb
- Field 3, Mar
- Field 4, Apr
- Field 5, May
- Field 6, Jun

### Diagram explanation

Twelve empty month boxes (Jul to Dec)

- Record, left to right: 6 fields
- Field 1, Jul
- Field 2, Aug
- Field 3, Sep
- Field 4, Oct
- Field 5, Nov
- Field 6, Dec

## Thirteen students cannot each get a different month

- Source lineage: `lecture#sections.card08_cannot_each_differ`
- Citations: `levin_dmoi_4e`

- The thirteenth student must join a month already used.
- Yes: at least two share, for any thirteen students.

### Diagram explanation

One student in every month: twelve placed (Jan to Jun)

- Record, left to right: 6 fields
- Field 1, Jan; value 1
- Field 2, Feb; value 1
- Field 3, Mar; value 1
- Field 4, Apr; value 1
- Field 5, May; value 1
- Field 6, Jun; value 1

### Diagram explanation

One student in every month: twelve placed (Jul to Dec)

- Record, left to right: 6 fields
- Field 1, Jul; value 1
- Field 2, Aug; value 1
- Field 3, Sep; value 1
- Field 4, Oct; value 1
- Field 5, Nov; value 1
- Field 6, Dec; value 1

## In groups: sort each case. Must two share?

- Source lineage: `lecture#sections.card09_sort_your_items`
- Citations: `levin_dmoi_4e`

Sort for real: your group by birth month, 3 socks by colour, dealt cards by suit. Record whether two share this time. Then ask: could a different group, grab or deal of the same size avoid sharing? If never, two must share.

| Case | Items | Containers | Shared this time? | Must two share? |
| --- | --- | --- | --- | --- |
| Your group, by birth month | 3 or 4 people | 12 months | [blank] | [blank] |
| Socks, by colour | 3 socks | 2 colours | [blank] | [blank] |
| Cards, by suit | 5 cards | 4 suits | [blank] | [blank] |
| Cards, by suit | 4 cards | 4 suits | [blank] | [blank] |

## Comparing two counts replaces checking every grab

- Source lineage: `lecture#sections.card10_compare_two_counts`
- Citations: `levin_dmoi_4e`

- Sock case: grab 3 socks from a drawer of red and blue. Must two match?
- Left: check every grab. Right: compare two counts. Same answer.
- Listing works for 3 socks; for 13 students in 12 months it is far too long.

### Check every grab

- 3 red: pair
- 2 red, 1 blue: pair
- 1 red, 2 blue: pair
- 3 blue: pair

### Compare two counts

- 3 socks, 2 colours
- 3 is more than 2
- Some colour gets two socks

## More items than containers forces some container to hold two

- Source lineage: `lecture#sections.card11_more_items_than_containers`
- Citations: `levin_dmoi_4e`

- Pigeonhole principle: more items than containers means some container holds at least two.

| Case | Items | Containers | Must two share? |
| --- | --- | --- | --- |
| Students by month | 13 | 12 | Yes |
| Socks by colour | 3 | 2 | Yes |
| Cards by suit | 5 | 4 | Yes |
| Cards by suit | 4 | 4 | No: sharing possible, not forced |
| Group by month | 3 or 4 | 12 | No: sharing possible, not forced |

## One student per month fits only twelve of fifteen

- Source lineage: `lecture#sections.card12_twelve_of_fifteen`
- Citations: `levin_dmoi_4e`

- Now explain it in words: why must two of any 15 students share a birth month?
- One class is not a reason. The claim is about any 15 students.
- Idea: suppose no month has two, then count who fits.

### Diagram explanation

At most one per month: twelve placed, three still waiting (Jan to Jun)

- Record, left to right: 6 fields
- Field 1, Jan; value 1
- Field 2, Feb; value 1
- Field 3, Mar; value 1
- Field 4, Apr; value 1
- Field 5, May; value 1
- Field 6, Jun; value 1

### Diagram explanation

At most one per month: twelve placed, three still waiting (Jul to Dec)

- Record, left to right: 6 fields
- Field 1, Jul; value 1
- Field 2, Aug; value 1
- Field 3, Sep; value 1
- Field 4, Oct; value 1
- Field 5, Nov; value 1
- Field 6, Dec; value 1

12 students fit; 3 still need a month.

## Why must two of fifteen students share a birth month?

- Source lineage: `lecture#sections.card13_why_must_two_share`
- Citations: `levin_dmoi_4e`

15 students, 12 birth months. In one or two sentences of your own, explain why at least two must share a month.

1. Name the items and the containers.
2. Say what goes wrong if no month has two.

**Response mode:** written

## Fifteen students force at least two into one month

- Source lineage: `lecture#sections.card14_fifteen_force_two`
- Citations: `levin_dmoi_4e`

- At least two, not exactly two.
- It does not say which month.
- Check: 15 ÷ 12 = 1.25, so some month has 2 or more.

If every month had at most one student, there could be only 12 students. There are 15, so at least one month must have two or more.

## When does the pigeonhole principle apply?

- Source lineage: `lecture#sections.card15_when_does_it_apply`
- Citations: `dive_into_systems`

In computers: more memory blocks than cache lines (fast storage slots) forces two blocks to share a line.

In one sentence: when does the pigeonhole principle apply?

**Response mode:** written

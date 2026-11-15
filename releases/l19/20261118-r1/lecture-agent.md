# Combinations: Choosing a Committee

- Course: IT 520: Foundations of IT
- Lecture: 12 / Combinations: Choosing a Committee
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `75c6092e96234e149435dd14265e196460400be56e8e9b2e59e4e4b98f2c212c`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## How many different committees can we choose?

- Source lineage: `lecture#sections.cover`
- Citations: `supplied_plan`

By the end, you can count committees, explain the smaller count, and find it in Pascal’s triangle.

## A committee stays the same when names move

- Source lineage: `lecture#sections.parade_and_committee`
- Citations: `supplied_plan`

- **Combination**: A selection in which order does not matter.

| Job | First naming | Second naming | Same result? |
| --- | --- | --- | --- |
| Parade: positions matter | A, B, C | B, A, C | No: a different parade |
| Committee: membership matters | A, B, C | B, A, C | Yes: the same committee |

## How many committees could your group pick?

- Source lineage: `lecture#sections.guess_first`
- Citations: `supplied_plan`

Choose three from your group of three or four: guess before picking.

1. Write your own guess on your sheet.
2. Write why you think so.
3. Keep the guess: do not correct it yet.

**Response mode:** written

## A new committee needs a different member

- Source lineage: `lecture#sections.pick_and_list`
- Citations: `supplied_plan`

We know names can move without changing membership. A complete list must count each selection once.

Pick committees of three from your group.

1. Record each selection as initials on your sheet.
2. Check whether you already have the same members.
3. Continue until you can defend your complete list.

**Response mode:** written

## How can you show your list is complete?

- Source lineage: `lecture#sections.defend_your_list`
- Citations: `supplied_plan`

Defend your list, with no committee counted twice.

1. Write the total you found.
2. Explain why none are missing.
3. Explain why none are repeated.

**Response mode:** written

## Why did twenty of the orders disappear?

- Source lineage: `lecture#sections.compare_counts`
- Citations: `supplied_plan`

Same people, same group size: 24 orders but only 4 committees.

| Group size | Parade orders | Committees of three |
| --- | --- | --- |
| 3 | 6 | 1 |
| 4 | 24 | 4 |

## Six different orders name the same committee

- Source lineage: `lecture#sections.six_names`
- Citations: `supplied_plan`

| Order | Members |
| --- | --- |
| A B C | A, B, C |
| A C B | A, B, C |
| B A C | A, B, C |
| B C A | A, B, C |
| C A B | A, B, C |
| C B A | A, B, C |

4 committees × 6 orders each = 24 ordered picks. Nothing was lost; each committee was counted 6 times.

## Each missing person identifies exactly one committee

- Source lineage: `lecture#sections.left_out_proof`
- Citations: `supplied_plan`

| People available | Person left out | Committee |
| --- | --- | --- |
| A, B, C, D | A | B C D |
| A, B, C, D | B | A C D |
| A, B, C, D | C | A B D |
| A, B, C, D | D | A B C |

Four people can sit out, so there are exactly four committees: none missing, none repeated. A group of three has one: everyone.

## Dividing out repeated orders gives the committee count

- Source lineage: `lecture#sections.worked_count`
- Citations: `supplied_plan`

We can list four committees. Larger groups need a calculation: count ordered picks, then divide out repeats.

| Move | Calculation | What it counts |
| --- | --- | --- |
| Pick three in order | 4 × 3 × 2 = 24 | Ordered picks |
| Order one committee | 3 × 2 × 1 = 6 | Orders per committee |
| Divide out repeats | 24 ÷ 6 = 4 | Committees |

## The formula divides out the same repeated orders

- Source lineage: `lecture#sections.general_count`
- Citations: `supplied_plan`

- **Factorial**: n! multiplies the whole numbers from n down to 1.

For n people, choose k members. C(n,k) counts committees; P(n,k) counts ordered picks. k! counts the orders of one committee: the repeats we divide out.

Choosing k from n

```text
C(n,k) = P(n,k) ÷ k!
       = n! ÷ (k! × (n-k)!)
C(4,3) = 4! ÷ (3! × 1!) = 24 ÷ 6 = 4
```

## How many committees can five people choose?

- Source lineage: `lecture#sections.try_five`
- Citations: `supplied_plan`

Choose three from five. Compute before looking ahead.

1. Count the ordered picks.
2. Count the orders of one committee.
3. Divide to find the committee count.

**Response mode:** written

## Five people can choose ten committees of three

- Source lineage: `lecture#sections.five_answer`
- Citations: `supplied_plan`

| Move | Calculation |
| --- | --- |
| Ordered picks | 5 × 4 × 3 = 60 |
| Orders of one committee | 3 × 2 × 1 = 6 |
| Committees | 60 ÷ 6 = 10 |

## Fifteen people can choose 455 committees of three

- Source lineage: `lecture#sections.fifteen_example`
- Citations: `supplied_plan`

| Move | Calculation |
| --- | --- |
| Ordered picks | 15 × 14 × 13 = 2,730 |
| Orders of one committee | 3 × 2 × 1 = 6 |
| Committees | 2,730 ÷ 6 = 455 |

## Adding neighboring entries builds Pascal’s triangle

- Source lineage: `lecture#sections.triangle_rule`
- Citations: `supplied_plan`

- **Pascal’s triangle**: Rows start and end with 1; each interior entry sums the two above.

Start at row zero

```text
n=0          1
n=1        1   1
```

The committee counts we computed also appear in this pattern of sums.

## Row and position locate the committee count

- Source lineage: `lecture#sections.triangle_lookup`
- Citations: `supplied_plan`

Row n is the group size. Position k is the number chosen. Count both from 0. Row 4 reads 1, 4, 6, 4, 1: positions 0 to 3 land on 4, our committee count. Row 5, position 3 is 10.

Pascal’s triangle

```text
n=0             1
n=1           1   1
n=2         1   2   1
n=3       1   3   3   1
n=4     1   4   6   4   1
n=5   1   5  10  10   5   1
```

## Choosing who serves also chooses who stays out

- Source lineage: `lecture#sections.triangle_symmetry`
- Citations: `supplied_plan`

| From four people | Triangle lookup | Count |
| --- | --- | --- |
| Choose three to serve | Row 4, position 3 | 4 |
| Choose one to leave out | Row 4, position 1 | 4 |

Each row reads the same backwards.

## Shuffling a selection leaves its members unchanged

- Source lineage: `lecture#sections.other_combinations`
- Citations: `supplied_plan`

| Selection | Why the order changes nothing |
| --- | --- |
| Pizza toppings | Pepperoni, mushroom, onion: the same toppings |
| Lottery numbers | Same numbers in any order: the same set |
| A team | The same three people belong |

Name one more. If shuffling changes it, it is a parade.

## Ignoring order counts each selection only once

- Source lineage: `lecture#sections.closing_sentence`
- Citations: `supplied_plan`

In your own words: why does order not mattering change the count?

1. Write one sentence on your sheet.
2. Use the committee and its repeated orders to explain.

**Response mode:** written

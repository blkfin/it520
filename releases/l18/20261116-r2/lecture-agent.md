# Permutations: How Many Ways Can You Line Up?

- Course: IT 520 - Foundations of IT
- Lecture: 12 / November 16, 2026
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `4d4f9808d4dea4d155ef52f3de94d4f0d8b4554be955c0094dddef674ebe1675`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## How many ways can you line up?

- Source lineage: `lecture#sections.cover`
- Citations: `supplied_plan`

By the end, you can count lineup orders and explain when order matters.

**Citations:** `supplied_plan`

## Swapping two people makes a different parade.

- Source lineage: `lecture#sections.position_matters`
- Citations: `supplied_plan`

- **Permutation**: An ordered arrangement of people or things.

**Citations:** `supplied_plan`

| Lineup | Front | Middle | Back |
| --- | --- | --- | --- |
| First | A | B | C |
| After a swap | B | A | C |

**Citations:** `supplied_plan`

## How many orders could your group stand in?

- Source lineage: `lecture#sections.guess_first`
- Citations: `supplied_plan`

Before you stand up, write your own guess and why.

1. Form a group of three or four.
2. Write your guess on your own sheet.
3. Explain your reasoning. No moving yet.

**Response mode:** written

**Citations:** `supplied_plan`

## Written orders let you check each new lineup.

- Source lineage: `lecture#sections.stand_and_record`
- Citations: `supplied_plan`

A swap changes the parade. A written record helps catch repeats.

**Citations:** `supplied_plan`

Now stand up and test your guess.

1. Line up and record initials, front to back: A-B-C.
2. Rearrange, write the new order, then repeat.
3. Check each order against the ones already recorded.

**Response mode:** written

**Citations:** `supplied_plan`

## How do you know each order is new?

- Source lineage: `lecture#sections.check_the_record`
- Citations: `supplied_plan`

Keep making new orders and recording them.

1. How are you keeping track?
2. Compare each new row with your earlier rows.
3. Found them all? Explain how you know.

**Response mode:** written

**Citations:** `supplied_plan`

## Grouping orders helps us look for missing lineups.

- Source lineage: `lecture#sections.pool_the_records`
- Citations: `supplied_plan`

Compare your guess with what your group found. A list alone may still miss some.

**Citations:** `supplied_plan`

| Front person | Orders recorded |
| --- | --- |
| A | ABC, ACB |
| B | [blank] |
| C | [blank] |

**Citations:** `supplied_plan`

Keep the front person fixed while you check the people behind.

**Citations:** `supplied_plan`

## Each existing order gives the newcomer several positions.

- Source lineage: `lecture#sections.new_person_new_positions`
- Citations: `supplied_plan`

| One order of A, B, C gains D |
| --- |
| D A B C |
| A D B C |
| A B D C |
| A B C D |

**Citations:** `supplied_plan`

Every old order does this, so the count multiplies.

**Citations:** `supplied_plan`

## Multiplying choices counts lineups without listing every order.

- Source lineage: `lecture#sections.choices_per_spot`
- Citations: `supplied_plan`

Listing gets long. Build one lineup spot by spot instead. Each spot filled leaves one fewer person for the next spot.

**Citations:** `supplied_plan`

- **Multiplication rule**: Multiply the choices at successive spots to count complete arrangements.

**Citations:** `supplied_plan`

### Diagram explanation

Four parade spots, filled from front to back.

- Record, left to right: 4 fields
- Field 1, 1st; choices; value 4
- Field 2, 2nd; choices; value 3
- Field 3, 3rd; choices; value 2
- Field 4, 4th; choice; value 1

**Citations:** `supplied_plan`

## Four people can stand in twenty four orders.

- Source lineage: `lecture#sections.four_worked`
- Citations: `supplied_plan`

### Four distinct people: A, B, C, D

| Spot | Choices | Running count | What we have counted |
| --- | --- | --- | --- |
| 1st | 4 | 4 | 4 front people |
| 2nd | 3 | 4 × 3 = 12 | 12 front pairs |
| 3rd | 2 | 12 × 2 = 24 | 24 first-three orders |
| 4th | 1 | 24 × 1 = 24 | 24 full lineups, 1 person left |

**Citations:** `supplied_plan`

## How many orders can three people make?

- Source lineage: `lecture#sections.three_try`
- Citations: `supplied_plan`

Work out the count for A, B, C on your sheet.

1. Write the choices for the first, second and third spots.
2. Multiply them.
3. Explain why the choices decrease.

**Response mode:** written

**Citations:** `supplied_plan`

## Three people make six orders, written as 3!.

- Source lineage: `lecture#sections.three_answer`
- Citations: `supplied_plan`

- **Factorial**: n!: the number of orders of n distinct people, n × (n−1) × … × 1.

**Citations:** `supplied_plan`

3! = 3 × 2 × 1 = 6

**Citations:** `supplied_plan`

| Front | Orders |
| --- | --- |
| A | ABC, ACB |
| B | BAC, BCA |
| C | CAB, CBA |

**Citations:** `supplied_plan`

## Four front choices each leave six orders behind.

- Source lineage: `lecture#sections.four_front_check`
- Citations: `supplied_plan`

| Front | People behind | Orders behind |
| --- | --- | --- |
| A | B, C, D | 6 |
| B | A, C, D | 6 |
| C | A, B, D | 6 |
| D | A, B, C | 6 |

**Citations:** `supplied_plan`

4 × 6 = 24 = 4 × 3 × 2 × 1 = 4!

**Citations:** `supplied_plan`

## Each new person multiplies the number of orders.

- Source lineage: `lecture#sections.growth`
- Citations: `supplied_plan`

| People | Choices multiplied | Orders |
| --- | --- | --- |
| 2 | 2 × 1 | 2 |
| 3 | 3 × 2 × 1 | 6 |
| 4 | 4 × 3 × 2 × 1 | 24 |
| 5 | 5 × 4 × 3 × 2 × 1 | 120 |

**Citations:** `supplied_plan`

Guesses run low: they add when the count multiplies.

**Citations:** `supplied_plan`

## Changing the order can change the result.

- Source lineage: `lecture#sections.real_life`
- Citations: `supplied_plan`

| Situation | What changes when you reorder it? |
| --- | --- |
| Batting lineup | Who bats with which runners on base |
| Playlist | Which song opens and which closes |
| Presentations | Which talk frames the later talks |
| Recipe steps | Fold first and there is nothing whipped to fold. |

**Citations:** `supplied_plan`

## Where else does changing the order change things?

- Source lineage: `lecture#sections.your_example`
- Citations: `supplied_plan`

Name one example beyond the parade.

1. Keep the same people or things.
2. Swap their positions or steps.
3. Explain what changes.

**Response mode:** discussion

**Citations:** `supplied_plan`

## Why does order matter?

- Source lineage: `lecture#sections.close`
- Citations: `supplied_plan`

Count ordered arrangements by multiplying choices, spot by spot.

**Citations:** `supplied_plan`

Finish on your own sheet: In my own words, order matters because:

1. Write one sentence.
2. Use your own words.

**Response mode:** written

**Citations:** `supplied_plan`

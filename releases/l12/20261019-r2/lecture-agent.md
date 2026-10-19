# Sets, mappings and predicates

- Course: IT 520 Foundations of Information Technology
- Lecture: 08 / October 19, 2026
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `e311681084161405d43b7c121a6896bdcfd045a85a1f0acd817fec7cca11be47`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## How can two apps preserve different weather information?

- Source lineage: `lecture#sections.card_01`
- Citations: `levin`

By the end, you can select and count reports, test conditions, and explain what a mapping preserves.

### Full weather display

- Reproduce the selected report

### Precipitation indicator

- Show whether precipitation is reported

## How many distinct labels does this log contain?

- Source lineage: `lecture#sections.card_02`
- Citations: `levin`

- A classroom feed selects one report label at a time.
- Both apps receive that same label.
- Real weather can combine conditions; our model selects one label.

| Available labels | Received log |
| --- | --- |
| ☀ Sunny | ☂ Rain |
| ☂ Rain | ☀ Sunny |
| ❄ Snow | ☂ Rain |
| ≋ Fog | End of log |

## A set counts each distinct member once

- Source lineage: `lecture#sections.card_03`
- Citations: `levin`

- Set: an unordered collection of distinct elements.
- Cardinality: the number of members in a finite set.

| Expression | Meaning |
| --- | --- |
| S = {Sunny, Rain, Snow, Fog} | All four available labels; |S| = 4 |
| [Rain, Sunny, Rain] | Three observations |
| {Rain, Sunny, Rain} = {Sunny, Rain} | Two distinct members; cardinality 2 |
| Rain ∈ S | Rain is a member of S |
| {Rain} ⊆ S | Subset: every member of {Rain} belongs to S |

## Intersection keeps both, while union keeps either

- Source lineage: `lecture#sections.card_04`
- Citations: `levin`

- Intersection A ∩ B: members in both sets.
- Union A ∪ B: members in either set, including both.

### Supplied classroom rules: precipitation for Rain/Snow; delay for Snow/Fog.

| Collection | Members |
| --- | --- |
| Precipitation A | {Rain, Snow} |
| Fictional school delay B | {Snow, Fog} |
| A ∩ B | {Snow} |
| A ∪ B | {Rain, Snow, Fog}; cardinality 3 |
| S ∖ A: members of S outside A | {Sunny, Fog} |

## A predicate turns an input into a proposition

- Source lineage: `lecture#sections.card_05`
- Citations: `levin`

- Listing members shows a set; a condition explains selection.
- Proposition: a declarative statement with a definite truth value.
- Predicate: an input condition that becomes a proposition when filled.

### S = {Sunny, Rain, Snow, Fog}; precipitation selects Rain/Snow.

| Weather expression | What it means |
| --- | --- |
| P(x): x reports precipitation | Open predicate; x comes from S |
| P(Rain) | True proposition |
| P(Fog) | False proposition |
| Display the weather | Command, not a proposition |

## Logical operators combine the same two weather tests

- Source lineage: `lecture#sections.card_06`
- Citations: `levin`

- AND (∧): both true; OR (∨): at least one true.
- NOT (¬): reverse the truth value.

### P: precipitation (Rain/Snow). Q: fictional delay rule (Snow/Fog). T = true; F = false.

| Report x | P | Q | P ∧ Q | P ∨ Q | ¬P |
| --- | --- | --- | --- | --- | --- |
| Sunny | F | F | F | F | T |
| Rain | T | F | F | T | F |
| Snow | T | T | T | T | F |
| Fog | F | T | F | T | T |

## Set builder notation collects inputs passing a predicate

- Source lineage: `lecture#sections.card_07`
- Citations: `levin`

Set builder {x ∈ S : P(x)} collects members of S satisfying P.

### S = {Sunny, Rain, Snow, Fog}

| Condition | Selected set |
| --- | --- |
| P(x): precipitation | A = {x ∈ S : P(x)} = {Rain, Snow} |
| Q(x): fictional delay | B = {x ∈ S : Q(x)} = {Snow, Fog} |
| P(x) ∧ Q(x) | A ∩ B = {Snow} |
| P(x) ∨ Q(x) | A ∪ B = {Rain, Snow, Fog} |

## Every needs all members, while some needs one

- Source lineage: `lecture#sections.card_08`
- Citations: `levin`

- ∀x ∈ S P(x): P holds for every member of S.
- ∃x ∈ S P(x): P holds for at least one member.

### Every: false, Fog is a counterexample. At least one: true, Rain is a witness.

| Report in S | P: precipitation |
| --- | --- |
| Sunny | F |
| Rain | T |
| Snow | T |
| Fog | F |

## A function assigns every input exactly one output

- Source lineage: `lecture#sections.card_09`
- Citations: `levin`

- Predicates select reports; each app must now produce an output.
- Function (mapping): assigns every domain member exactly one codomain member.
- Domain: allowed inputs. Codomain: declared possible outputs.

### Domain S = {Sunny, Rain, Snow, Fog}. E codomain = {00, 01, 10, 11}; I codomain = {No, Yes}. Codes are two-bit strings.

| Input x | Display E(x) | Indicator I(x) |
| --- | --- | --- |
| Sunny | 00 | No |
| Rain | 01 | Yes |
| Snow | 10 | Yes |
| Fog | 11 | No |

## The range contains only outputs actually attained

- Source lineage: `lecture#sections.card_10`
- Citations: `levin`

- Range: outputs attained over the whole declared domain.
- A codomain member may remain unused.

| Mapping | Domain | Codomain | Range |
| --- | --- | --- | --- |
| Display E | {Sunny, Rain, Snow, Fog} | {00, 01, 10, 11} | {00, 01, 10, 11} |
| Indicator I | {Sunny, Rain, Snow, Fog} | {No, Yes} | {No, Yes} |
| Restricted indicator J: Rain → Yes; Snow → Yes | {Rain, Snow} | {No, Yes} | {Yes} |

## Do these drafts assign every input one output?

- Source lineage: `lecture#sections.card_11`
- Citations: `levin`

Domain S = {Sunny, Rain, Snow, Fog}; codomain = {No, Yes}.

### Proposed classroom rules, not published app codebooks.

| Input | Draft A outputs | Draft B outputs |
| --- | --- | --- |
| Sunny | No | No |
| Rain | Yes | Yes and No |
| Snow | Yes | Yes |
| Fog | No output assigned | No |

## Missing and conflicting outputs both break the definition

- Source lineage: `lecture#sections.card_12`
- Citations: `levin`

A function must cover its declared domain consistently.

### Domain S = {Sunny, Rain, Snow, Fog}; codomain = {No, Yes}

| Draft | Decisive input | Violation |
| --- | --- | --- |
| A | Fog: no output | Fails to assign every input |
| B | Rain: Yes and No | Fails exactly one output |

## Distinct display codes preserve each original report

- Source lineage: `lecture#sections.card_13`
- Citations: `levin`

- One-to-one: distinct inputs receive distinct outputs.
- The agreed dictionary decodes 01 as Rain.

### E codomain = {00, 01, 10, 11}

| Report x | Display E(x) |
| --- | --- |
| Sunny | 00 |
| Rain | 01 |
| Snow | 10 |
| Fog | 11 |

## The indicator preserves precipitation but discards report identity

- Source lineage: `lecture#sections.card_14`
- Citations: `levin`

Both apps satisfy their own receiving requirements.

| Received output | Possible original reports | Receiver can determine |
| --- | --- | --- |
| Display 01 | {Rain} | Exact report |
| Indicator Yes | {Rain, Snow} | Precipitation reported |
| Indicator No | {Sunny, Fog} | No precipitation reported |

## What are the inputs, the condition and the lost distinction?

- Source lineage: `lecture#sections.card_15`
- Citations: `levin`

Connect the objects, the test and the receiver's purpose.

Recall the weather model.

- Which objects form the input set?
- What condition selects the precipitation subset?
- What distinction does the precipitation indicator discard?

**Response mode:** verbal_guess

## Choose mappings that preserve the distinctions receivers need

- Source lineage: `lecture#sections.card_16`
- Citations: `levin`

A correct summary may discard the original report's identity.

### Full weather display

- Inputs: Sunny, Rain, Snow, Fog
- 01 identifies Rain by the agreed dictionary

### Precipitation indicator

- Precipitation selects Rain and Snow
- Yes leaves Rain or Snow possible, so exact report identity is lost

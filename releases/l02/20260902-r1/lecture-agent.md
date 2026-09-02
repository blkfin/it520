# IT 520 — Performance and Systems: L2 — One Instruction Changes the Machine

- Course: IT 520 — Performance and Systems
- Lecture: Week 01 / L2 — One Instruction Changes the Machine
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `4397b4754f7660a23469f1bb40be5673394e989e5ab6dd370f8a9594bc07124c`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## A score goes from 7 to 8

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.score_seven_to_eight`
- Citations: `l2_accepted_route`

### Before the click

- Score: 7

### After the click

- Score: 8

Something inside the computer changed between these two pictures.

## Write your prediction before the machine appears

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.commit_one_prediction`
- Citations: `trace_half_sheet`

Answer the question printed in the Opening prediction box on your half-sheet.

**Response mode:** written

**Time:** 90 seconds

## Instructions and data are both 16-bit words in addressed memory.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.stored_program_board`
- Citations: `c1_2_course_knowledge`, `c1_1_course_knowledge`, `dive_systems_arch`

### Machine board, opening state

| Region | Location | Contents |
| --- | --- | --- |
| MEMORY | address 40 | 0001 0001 11001001 (= 4553) |
| MEMORY | address 41 | 0010 0000 11001001 (= 8393) |
| MEMORY | address 201 | 0000 0000 00000111 (= 7) |
| CPU | PC | 40 |
| CPU | IR | empty |
| CPU | ALU | empty |
| I/O | display | score 7 |

**Constraints:** preserve_exact_values

An address names one memory location. PC holds the next address; IR holds the fetched instruction; ALU does arithmetic; display is output. Context, not the bits alone, gives each word its role.

## Turn to the Word classification section of your half-sheet.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.classify_instruction_or_data`
- Citations: `c1_2_course_knowledge`

Work the Word classification section printed on your half-sheet.

**Response mode:** written

**Time:** 120 seconds

## Fetch copies the word at address 40 into IR and advances the PC.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.fetch_word_40`
- Citations: `c1_2_course_knowledge`, `dive_systems_arch`

### Machine board, after fetch

| Region | Location | Contents | Changed this phase |
| --- | --- | --- | --- |
| MEMORY | address 201 | 0000 0000 00000111 (= 7) | no |
| CPU | PC | 41 | yes, from 40 |
| CPU | IR | 0001 0001 11001001 (= 4553) | yes, from empty |

**Constraints:** preserve_exact_values

Movement: PC supplies address 40; memory[40] → IR; PC 40 → 41.

Takeaway: Nothing in memory and nothing on the display has changed yet.

## Decode turns those bits into ADD 1 TO [201].

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.decode_word_40`
- Citations: `c1_2_course_knowledge`, `dive_systems_arch`

### Instruction format for this machine

| Field | Bits | Values used here |
| --- | --- | --- |
| operation | 15-12 | 0001 = ADD, 0010 = DISPLAY |
| immediate | 11-8 | 0001 = 1, 0100 = 4, 0000 = unused |
| address | 7-0 | 11001001 = 201, 11001000 = 200 |

**Constraints:** preserve_exact_values

### Machine board, after decode

| Region | Location | Contents | Changed this phase |
| --- | --- | --- | --- |
| CPU | decoded fields | ADD, immediate 1, destination 201 | yes, from empty |

**Constraints:** preserve_exact_values

Control reads the decoded instruction and directs the next state change.

Direction: IR → decoded fields → control → the next movement.

Takeaway: The same bits mean nothing until an agreed format assigns their fields.

## Execute sends the stored 7 and the immediate 1 through the ALU.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.execute_add`
- Citations: `c1_2_course_knowledge`

### Machine board, after execute

| Region | Location | Contents | Changed this phase |
| --- | --- | --- | --- |
| MEMORY | address 201 | 0000 0000 00000111 (= 7) | no |
| CPU | ALU | 7 + 1 = 8 | yes, from empty |

**Constraints:** preserve_exact_values

Movement: control → ALU; memory[201] supplies 7; immediate supplies 1.

Takeaway: The result exists in the ALU before any memory location holds it.

## Write-back changes the word at address 201 from 7 to 8.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.write_back_201`
- Citations: `c1_2_course_knowledge`

### Machine board, after write-back

| Region | Location | Contents | Changed this phase |
| --- | --- | --- | --- |
| MEMORY | address 201 | 0000 0000 00001000 (= 8) | yes, from 0000 0000 00000111 (= 7) |

**Constraints:** preserve_exact_values

Movement: control directs ALU result 8 → memory[201].

Takeaway: Memory now holds the new score, and the display still does not.

## A second instruction is needed before the screen shows 8.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.display_continuation`
- Citations: `c1_2_course_knowledge`, `dive_systems_arch`

### Reaching the display

| Step | Board change |
| --- | --- |
| fetch the word at address 41 | IR becomes 0010 0000 11001001 (= 8393) |
| decode it | DISPLAY, address 201 |
| reach I/O | display becomes score 8 |

**Constraints:** preserve_exact_values

Takeaway: Changing a value in memory and showing it to a person are separate movements.

## The four frames explain a value change, not the timing of a real processor.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.four_frames_boundary`
- Citations: `c1_2_course_knowledge`

### The frames establish

- The order in which the parts act
- Which field changes at each phase
- That the stored value changed before the display did

### The frames cannot establish

- How long any phase takes
- Whether instructions overlap in a real processor
- Where a copy of the data is kept close to the CPU
- When a pixel actually changes

**Citations:** `c1_2_course_knowledge`

Fill in the Model boundary line printed on your half-sheet.

**Response mode:** written

**Time:** 60 seconds

Takeaway: A causal model can be correct about order and silent about time.

## A finite transistor and power budget buys cores, cache, or a specialized path.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.spend_transistor_budget`
- Citations: `c1_3_course_knowledge`, `c1_2_course_knowledge`, `course_allocation`

### Three annotations on the same board

| Added to the board | What it buys | Only helps when |
| --- | --- | --- |
| a second core | another place to run work at the same time | the work can be split into independent parts |
| a cache beside the CPU | reused instructions and data kept close | the same data is used again soon |
| a specialized path | many similar operations done together | the work is many similar operations |

Fill in the Design budget line printed on your half-sheet.

**Response mode:** written

**Time:** 60 seconds

Takeaway: Each purchase helps some work and leaves other work exactly as slow.

## Your sheet starts the same machine from a new state.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.fresh_trace_setup`
- Citations: `trace_half_sheet`, `c1_2_course_knowledge`

### Machine board, fresh start state

| Region | Location | Contents |
| --- | --- | --- |
| MEMORY | address 80 | 0001 0100 11001000 (= 5320) |
| MEMORY | address 200 | 0000 0000 00000110 (= 6) |
| CPU | PC | 80 |

**Constraints:** preserve_exact_values

Record F: IR and PC; D: operation and operands; E: ALU work; W: the stored result.

Takeaway: The route is the one already on the board; only the numbers are new.

## Run the machine yourself, then compare with a partner

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.fresh_trace_work`
- Citations: `trace_half_sheet`, `course_allocation`

Work the four printed frames on your half-sheet, then compare with a partner.

1. Fill F, D, E, and W on your own before speaking to anyone.
2. Compare frame by frame with your partner.
3. Mark any change you make in a different color or with an asterisk.

**Response mode:** written

**Time:** 420 seconds

## Address 200 now holds 10 and the PC has advanced to 81.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.fresh_trace_reveal`
- Citations: `c1_2_course_knowledge`, `trace_half_sheet`

### Machine board, fresh trace complete

| Region | Location | Contents |
| --- | --- | --- |
| MEMORY | address 200 | 0000 0000 00001010 (= 10) |
| CPU | PC | 81 |
| CPU | IR | 0001 0100 11001000 (= 5320) |
| CPU | ALU | 6 + 4 = 10 |

**Constraints:** preserve_exact_values

Takeaway: The write-back landed in memory, exactly as it did in the guided trace.

## One stored value changes, and nothing else does

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.changed_input_rerun`
- Citations: `trace_half_sheet`, `c1_2_course_knowledge`

### Machine board, one changed value

| Region | Location | Contents |
| --- | --- | --- |
| MEMORY | address 80 | 0001 0100 11001000 (= 5320) |
| MEMORY | address 200 | 0000 0000 00001001 (= 9) |
| CPU | PC | 80 |

**Constraints:** preserve_exact_values

Work the Change one condition section printed on your half-sheet.

**Response mode:** annotation

**Time:** 120 seconds

## The route is unchanged and the result becomes 13.

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.changed_input_result`
- Citations: `c1_2_course_knowledge`

### Stays the same

- The four phases and their order
- The jobs of PC, IR, control, and the ALU
- The destination address 200

### Changes

- The stored input, from 6 to 9
- The written result, from 10 to 13

**Citations:** `c1_2_course_knowledge`

Address 200 ends holding 0000 0000 00001101 (= 13).

Takeaway: Different data through the same four phases is the whole difference.

## Turn to the Boundary check section of your half-sheet

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.shuffled_snapshot_order`
- Citations: `trace_half_sheet`, `course_allocation`

### Four machine-state snapshots, out of order

| Snapshot | IR / PC | Decoded fields | ALU | memory[201] |
| --- | --- | --- | --- | --- |
| #1 | 4553 / 41 | ADD 1 to [201] | 7 + 1 = 8 | 7 |
| #2 | 4553 / 41 | not yet | empty | 7 |
| #3 | 4553 / 41 | ADD 1 to [201] | 7 + 1 = 8 | 8 |
| #4 | 4553 / 41 | ADD 1 to [201] | empty | 7 |

**Constraints:** preserve_exact_values

Work the Boundary check section printed on your half-sheet against the projected numbered snapshots.

**Response mode:** written

**Time:** 120 seconds

## Close on the printed boundary line

- Source lineage: `it520-fall-2026-week01-l2-cycle-1-core#sections.one_unsupported_claim`
- Citations: `trace_half_sheet`, `course_allocation`, `c1_2_course_knowledge`

Complete the final printed line on your half-sheet, then hand it in.

**Response mode:** written

**Time:** 60 seconds

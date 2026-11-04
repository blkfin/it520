# Processes, memory regions and threads

- Course: IT 520 - Foundations of IT
- Lecture: 16 / November 4, 2026
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `9e54e541a77c12c203fe2f5350d39bfb27433ab31eb4cd71c64df31cb908e28d`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## Which parts of one job can run at the same time?

- Source lineage: `lecture#sections.card_01`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

One sensor program reads a file and counts its readings.

Takeaway: Explain one running program’s memory and executions; predict what can overlap.

## A process has its own memory for a running program

- Source lineage: `lecture#sections.card_02`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Process: a running program instance. Address space: the memory addresses it can use.

### Diagram explanation

The earlier simplified model attached memory and a next instruction to a process; the next card refines the owner of that position.

- Record, left to right: 2 fields
- Field 1, Private memory; value program and data
- Field 2, Next instruction; value whose position?

## The next instruction belongs to a thread inside the process

- Source lineage: `lecture#sections.card_03`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Thread: an execution stream within a process. Program counter (PC): its next-instruction address.

### Diagram explanation

A process starts with one thread; that thread owns the next-instruction position.

- Record, left to right: 2 fields
- Field 1, Process P; value private memory
- Field 2, Thread A; value owns the PC

## Code holds instructions and globals hold program-wide variables

- Source lineage: `lecture#sections.card_04`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Function: a named group of instructions. Code: program instructions. Globals: variables defined outside functions. Threshold: a cutoff for qualifying a reading.

### Diagram explanation

First two regions of the sensor address space: count-function instructions and the threshold global.

- Record, left to right: 2 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold

## The heap holds the readings buffer allocated at run time

- Source lineage: `lecture#sections.card_05`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Heap: memory allocated at run time. Buffer: memory holding data for use.

### Diagram explanation

The same sensor address space now includes the readings buffer allocated when the program runs.

- Record, left to right: 3 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings

## The stack holds local values during a function call

- Source lineage: `lecture#sections.card_06`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

A call runs a function. Local variable: a value belonging to one call. Stack: active calls and their local values.

i: loop counter marking the reading position. count: the local running tally.

### Diagram explanation

One address space: code, globals and heap, and one stack.

- Record, left to right: 4 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack; value i, count

## Two threads share memory and keep separate execution state

- Source lineage: `lecture#sections.card_07`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

CPU: processor executing instructions. Registers: its storage for current working values. Shared: the same memory locations used by both threads.

Each thread has its own PC, registers and stack.

### Diagram explanation

One address space: code, globals and heap shared; two separate stacks.

- Record, left to right: 5 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack A; value i, count
- Field 5, Stack B; value i, count

Thread execution state, outside the memory-region map:

### Diagram explanation

Execution state belongs to threads; these boxes are not memory regions.

- Record, left to right: 2 fields
- Field 1, Thread A; value PC + registers
- Field 2, Thread B; value PC + registers

## Two processes have separate address spaces and their own threads

- Source lineage: `lecture#sections.card_08`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Process P: its own address space and thread A.

### Diagram explanation

P: One address space: code, globals and heap, and one stack.

- Record, left to right: 4 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack; value i, count

Process Q: another address space and thread B.

### Diagram explanation

Q: One address space: code, globals and heap, and one stack.

- Record, left to right: 4 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack; value i, count

Takeaway: Threads in one process share its memory; these two processes have separate ordinary memory.

## A completed global change is visible to the other thread

- Source lineage: `lecture#sections.card_09`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Shared means the same memory location. After A changes threshold, B reads that changed threshold.

### Diagram explanation

One address space: code, globals and heap shared; two separate stacks.

- Record, left to right: 5 fields
- Field 1, Code; value count()
- Field 2, Globals; value changed
- Field 3, Heap; value readings
- Field 4, Stack A; value i, count
- Field 5, Stack B; value i, count

## A local change leaves the other thread’s local alone

- Source lineage: `lecture#sections.card_10`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Each count-function call has its own local i and count.

### Diagram explanation

One address space: code, globals and heap shared; two separate stacks.

- Record, left to right: 5 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack A; value A changed
- Field 5, Stack B; value B unchanged

## A global change stays within its process’s address space

- Source lineage: `lecture#sections.card_11`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Process P changes its threshold global.

### Diagram explanation

P: One address space: code, globals and heap, and one stack.

- Record, left to right: 4 fields
- Field 1, Code; value count()
- Field 2, Globals; value changed
- Field 3, Heap; value readings
- Field 4, Stack; value i, count

Process Q keeps its own threshold global.

### Diagram explanation

Q: One address space: code, globals and heap, and one stack.

- Record, left to right: 4 fields
- Field 1, Code; value count()
- Field 2, Globals; value unchanged
- Field 3, Heap; value readings
- Field 4, Stack; value i, count

Takeaway: P and Q have separate global variables in this example.

## Combine waits until both chunk results are ready

- Source lineage: `lecture#sections.card_12`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Dependency: a step needs another step’s result before it can start.

### Diagram explanation

Both chunk counts wait for the file; Combine waits for both separate results.

- Arrow from Read file to Count chunk A: input ready
- Arrow from Read file to Count chunk B: input ready
- Arrow from Count chunk A to Combine counts: A result ready
- Arrow from Count chunk B to Combine counts: B result ready

## Two cores can execute the ready threads together

- Source lineage: `lecture#sections.card_13`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Core: a CPU execution unit. Parallel execution: different cores execute instructions at the same time.

### Diagram explanation

Two ready threads can execute on two available compute cores.

- Record, left to right: 2 fields
- Field 1, Core A; value thread A
- Field 2, Core B; value thread B

## Which changes does the other thread observe?

- Source lineage: `lecture#sections.card_14`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

### Diagram explanation

One address space: code, globals and heap shared; two separate stacks.

- Record, left to right: 5 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack A; value i, count
- Field 5, Stack B; value i, count

A changes threshold, its local count, or its PC. Which changes affect B’s corresponding value?

**Response mode:** discussion

Takeaway: A and B belong to one process. A completes a change before B continues.

## Shared storage changes while separate thread state stays separate

- Source lineage: `lecture#sections.card_15`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

B reads the changed threshold. B keeps its own local count and PC.

### Diagram explanation

One address space: code, globals and heap shared; two separate stacks.

- Record, left to right: 5 fields
- Field 1, Code; value count()
- Field 2, Globals; value changed
- Field 3, Heap; value readings
- Field 4, Stack A; value A changed
- Field 5, Stack B; value B unchanged

### Diagram explanation

Execution state belongs to threads; these boxes are not memory regions.

- Record, left to right: 2 fields
- Field 1, Thread A; value PC + registers
- Field 2, Thread B; value PC + registers

## A process supplies memory and threads carry execution

- Source lineage: `lecture#sections.card_16`
- Citations: `ostep_threads`, `dis_multicore`, `dis_process_resources`

Process: a running program instance with its own address space. Thread: one execution stream within it.

### Diagram explanation

One address space: code, globals and heap shared; two separate stacks.

- Record, left to right: 5 fields
- Field 1, Code; value count()
- Field 2, Globals; value threshold
- Field 3, Heap; value readings
- Field 4, Stack A; value i, count
- Field 5, Stack B; value i, count

### Diagram explanation

Execution state belongs to threads; these boxes are not memory regions.

- Record, left to right: 2 fields
- Field 1, Thread A; value PC + registers
- Field 2, Thread B; value PC + registers

Takeaway: One address space can support several execution streams.

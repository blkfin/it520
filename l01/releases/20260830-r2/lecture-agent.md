# IT 520 — Performance and Systems: L1 — Why Performance Still Matters

- Course: IT 520 — Performance and Systems
- Lecture: Week 01 / L1 — Why Performance Still Matters
- Semantic source: `var/lecture-publish/previews/20260830T004520Z/build/lecture.resolved.json`
- Semantic source SHA-256: `2ce275d7fef8f34ca800fd372fecb43235f1a94f5e9a86d5bcbbed097e979fff`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## Why performance still matters

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.walk_in`
- Citations: `course_direction`

IT 520 | Performance and Systems | Week 1

Program → runtime → operating system → CPU/GPU → memory → storage → network. How does software use the machine beneath it?

## A transistor is a tiny electronic switch.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.transistor_term`
- Citations: `c1_3_course_knowledge`

- **Transistor**: A tiny electronic component that can act like a switch.

Modern chips contain billions of them.

## For decades, more transistors made computing cheaper.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.cheaper_computing`
- Citations: `computer_history_moore`, `ai_index_2026`, `c1_3_course_knowledge`

- 1965: More components fit economically on a chip while cost per component fell. (Computer History Museum · 1965 observation) [`computer_history_moore`]
- 2006–2024: Stanford's cost index for computation on GPUs (processors built for many similar calculations) fell by more than 99%. (Stanford HAI · 2006–2024) [`ai_index_2026`]

**Citations:** `computer_history_moore`, `ai_index_2026`

## Growing AI systems are increasing demand for memory and energy.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.current_pressure`
- Citations: `micron_fq3_2026`, `iea_energy_ai`, `ai_index_2026`

### Memory

- AI data centers need more capacity and bandwidth
- Micron | June 2026

### Energy

- Data-center electricity use could more than double by 2030
- IEA | 2025 base case

## Capacity and bandwidth describe different limits.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.memory_terms`
- Citations: `c1_3_course_knowledge`, `dive_systems_memory`

- **Capacity**: How much data can be held at once.
- **Bandwidth**: How fast data can move.
- **DRAM**: Fast working memory used by a running program.
- **NAND**: Flash storage that keeps data without power.

## A running program depends on seven system layers.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.course_map`
- Citations: `c1_1_course_knowledge`

### Layer and job

| Layer | Job on the course map |
| --- | --- |
| Program | Expresses the task the user wants done |
| Runtime | Software that carries out language and library operations between source code and machine execution |
| Operating system | Coordinates programs and hardware access |
| CPU/GPU | Performs instructions and calculations |
| Memory | Holds active code and data near the processor |
| Storage | Retains files beyond the current computation |
| Network | Moves data between machines |

## Memory holds active work; storage keeps data.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.memory_and_storage`
- Citations: `dive_systems_memory`, `c1_1_course_knowledge`

### Memory

- Holds code and data currently in use
- Fast access near the processor
- Contents normally disappear when power is removed

### Storage

- Keeps files and data between runs
- Larger and slower than working memory
- Retains contents without power

## A local Python program uses the system without using the network.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.local_python_program`
- Citations: `c1_1_course_knowledge`, `dive_systems_os`

A Python program opens a dataset stored on the laptop, decodes each record, computes a result, and shows it on screen. It does not call a website or network service.

## One program crosses software and hardware layers.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.python_system_route`
- Citations: `c1_1_course_knowledge`, `dive_systems_os`

### Work in the Python example

| Layer | Work in this program |
| --- | --- |
| Program | Requests the task and displays the result |
| Runtime | Carries out Python and library operations |
| Operating system | Starts the process and manages file access |
| CPU | Performs instructions and calculations |
| Memory | Holds active code and data |
| Storage | Supplies the saved dataset |
| Network | Not used in this example |

## Instructions and working data meet in memory.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.stored_program`
- Citations: `c1_2_course_knowledge`, `dive_systems_arch`, `dive_systems_os`

### From file to running program

| Location | What happens |
| --- | --- |
| Storage | The program begins as a saved file |
| Memory | The OS loads instructions and working data |
| Processor | Instructions transform data and store results |

## The processor repeatedly fetches, decodes, executes, and writes back.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.instruction_cycle`
- Citations: `c1_2_course_knowledge`, `dive_systems_arch`

### One instruction cycle

| Step | What the processor does |
| --- | --- |
| Fetch | Get the next instruction from memory |
| Decode | Identify the operation and its input values |
| Execute | Perform the operation |
| Write back | Save the result in a register or memory |

## One line of Python becomes many lower-level operations.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.python_is_not_instruction`
- Citations: `c1_2_course_knowledge`

### Source level

- total = sum(values)
- One operation expressed for the programmer

### Machine level

- Runtime and library code perform many operations
- The processor executes machine instructions, not Python text

## More transistors stopped meaning one faster program.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.single_program_slowdown`
- Citations: `c1_3_course_knowledge`, `hennessy_golden_age`

- By the mid-2000s, gains in clock speed and general-purpose performance had slowed. (Hennessy and Patterson · 2019 retrospective) [`hennessy_golden_age`]

**Citations:** `hennessy_golden_age`

## More transistors went to cores, cache, and GPUs.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.transistor_budget`
- Citations: `c1_2_course_knowledge`, `c1_3_course_knowledge`, `hennessy_golden_age`

- **More cores**: Run independent work at the same time.
- **Cache**: Small, fast memory that keeps frequently used instructions and data close to the processor.
- **GPUs**: Perform many similar calculations at once.

## Performance depends on compute and data movement.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.performance_resources`
- Citations: `c1_1_course_knowledge`, `c1_3_course_knowledge`, `dive_systems_memory`

### Four performance resources

| Resource | What matters |
| --- | --- |
| Compute | Operations per second and work per operation |
| Memory | Capacity and bandwidth |
| Storage | Delay before each read or write, and data moved per second |
| Network | Amount of data, data moved per second, and delay for a request and reply |

## This course follows time and money through the system.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.course_promise`
- Citations: `course_direction`

- Trace a workload across software and hardware layers.
- Measure where time and memory go.
- Choose compute and storage resources with evidence.

## Wednesday opens the processor.

- Source lineage: `it520-fall-2026-week01-l1-vern-bakeoff#sections.wednesday`
- Citations: `c1_2_course_knowledge`

- Control chooses and decodes the next instruction.
- Registers and arithmetic hardware transform data.
- Memory and I/O connect the processor to the rest of the system.
- One complete instruction trace connects memory to a result.

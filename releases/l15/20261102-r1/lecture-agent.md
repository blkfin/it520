# IT 520 - Foundations of IT: Monday, November 2, 2026

- Course: IT 520 - Foundations of IT
- Lecture: Week 10 / Monday, November 2, 2026
- Semantic source: `lecture.resolved.json`
- Semantic source SHA-256: `668842a9182df3221a4fe1ef6e0c70b2cfc42cb44f72cd7b2d57679f976b8259`
- Schema: `lecture/v1`

Normalized reader semantics, projected per block type from the resolved lecture document. Layout classes, presenter chrome, SVG drawing instructions and instructor-only fields are not part of this projection — they were never built.

## What does the operating system do so many programs can share one machine?

- Source lineage: `lecture#sections.cover`
- Citations: none

You will learn to say when the operating system runs, track programs through four states, explain how two programs use the same address without sharing data, and translate an address with a page table.

### Many programs

- A Python script
- A browser
- A shell

### One machine

- One CPU
- One RAM
- One disk

## Three programs share one laptop with one CPU

- Source lineage: `lecture#sections.three_programs`
- Citations: none

- A Python script reads a sensor file from disk.
- A browser loads a page.
- A shell runs a command you typed a moment ago.
- One CPU, one RAM and one disk serve all three.

## The operating system runs when a program asks or a device signals

- Source lineage: `lecture#sections.os_runs_when_asked`
- Citations: `dis_os`

- Operating system: software between programs and hardware that shares the hardware.
- System call: a program's request for an OS service, made with a trap.
- Interrupt: a device's signal that makes the OS run.

### Diagram explanation

The script asks the operating system to read its file; the OS starts the disk; the disk signals the OS when the read is done

- Arrow from Script (program) to Operating system: system call (trap): read the file
- Arrow from Operating system to Disk (hardware): start the read
- Arrow from Disk (hardware) to Operating system: interrupt: the read is done

## User mode keeps a program away from devices and other memory

- Source lineage: `lecture#sections.user_mode_limits`
- Citations: `dis_os`

- User mode: the CPU limits a program to its own memory.
- Kernel mode: OS code may use all memory and every device.

### User mode (the script)

- Runs its own instructions
- Cannot drive the disk itself
- Asks with a system call

### Kernel mode (the operating system)

- Entered on a trap or an interrupt
- Drives the disk for the script
- Returns the CPU to the script in user mode

**Citations:** `dis_os`

## The OS keeps each running program as a process with saved state

- Source lineage: `lecture#sections.process_saved_state`
- Citations: `dis_13_2`

- We know: the script asked the OS to read its file.
- A disk read is slow; the CPU would sit idle.
- Process: a running program plus the state the OS saves for it, so it can stop and resume.

### Diagram explanation

What the OS saves for the script while its disk read is under way

- Record, left to right: 4 fields
- Field 1, Next instruction; where to resume
- Field 2, Registers; values in use
- Field 3, Stack; calls in progress
- Field 4, Memory; its code and data

## The script waits for the disk: must the browser and shell wait?

- Source lineage: `lecture#sections.predict_cpu_during_read`
- Citations: none

One CPU. The script has started its disk read.

While the script waits for its disk read, what does the CPU do?

- It waits until the script's read finishes
- It runs the browser or the shell meanwhile
- It runs all three programs at the same instant

**Response mode:** verbal_guess

## While the script waits, the OS switches the CPU to the browser

- Source lineage: `lecture#sections.context_switch`
- Citations: `dis_13_2`

- Context switch: the OS saves one process's state and restores another's.
- Multiprogramming: running another process while one waits.
- Timesharing: short turns (time slices) so all seem to run.

### Diagram explanation

The script starts its disk read, the OS saves its state and restores the browser's, and the browser runs

- Arrow from Script runs to OS saves script state: system call: read
- Arrow from OS saves script state to OS restores browser state: browser chosen
- Arrow from OS restores browser state to Browser runs: back to user mode

## Every process is Ready, Running, Blocked or Exited

- Source lineage: `lecture#sections.four_states`
- Citations: `dis_13_2`, `ostep_proc`

- Running: on the CPU now.
- Ready: could run; waiting for its turn.
- Blocked: waiting for an event, such as a disk read.
- Exited: finished; will not run again.

### Diagram explanation

How a process moves between Ready, Running, Blocked and Exited

- Arrow from Ready to Running: OS gives it the CPU
- Arrow from Running to Ready: time slice ends
- Arrow from Running to Blocked: starts a disk read
- Arrow from Blocked to Ready: read finishes
- Arrow from Running to Exited: program ends

## A turn rule and an event list decide every process's state

- Source lineage: `lecture#sections.state_trace`
- Citations: `ostep_proc`

- Rule: turns go Script, Browser, Shell, then repeat; slices are 2 ms.
- Skip Blocked processes; a finished read makes a process Ready.

### Three processes on one CPU (supplied times)

| Time | Event | Script | Browser | Shell | Row mark |
| --- | --- | --- | --- | --- | --- |
| 0 ms | CPU starts the script | Running | Ready | Ready |  |
| 1 ms | Script starts a disk read | Blocked | Running | Ready |  |
| 3 ms | Browser's slice ends | Blocked | Ready | Running |  |
| 4 ms | Disk read finishes | Ready | Ready | Running | highlighted row |
| 5 ms | Shell's slice ends | Running | Ready | Ready |  |

## Two copies of one program use one address for different values

- Source lineage: `lecture#sections.same_address_puzzle`
- Citations: `same_address_specimen`, `dis_13_3`

- We know: processes take turns, and each keeps its own state.
- All of them sit in one RAM; one could overwrite another.
- Virtual address space: each process's private set of addresses, mapped by the OS to RAM.

Two runs of one program at the same time

```text
$ ./same_address 100 & ./same_address 200; wait
pid 3932632: count is at 0x404034 and holds 200
pid 3932631: count is at 0x404034 and holds 100
```

## A virtual address splits into a page number and an offset

- Source lineage: `lecture#sections.address_split`
- Citations: `dis_13_3`

- Page: a 4 KB block of a process's virtual addresses.
- Frame: a 4 KB block of RAM that holds one page.
- Offset: the byte's place inside its page; translation keeps it.
- 4 KB = 4,096 = 0x1000 bytes, so the offset is the last three hex digits.

### Diagram explanation

The virtual address 0x404034 split into page number 0x404 and offset 0x034

- Record, left to right: 2 fields
- Field 1, Page number; which 4 KB page; value 0x404
- Field 2, Offset; which byte in the page; value 034

## Each process's page table sends 0x404034 to a different place

- Source lineage: `lecture#sections.page_table_lookup`
- Citations: `dis_13_3`

- Page table: one process's function from page number to an entry: a frame, or "not in RAM".
- Split: page 0x404, offset 0x034.
- Look up page 0x404: A gives frame 0x123, B gives 0x456.
- Check the page is in RAM: both entries are frames.
- Join frame and offset: A 0x123034, B 0x456034.

### Supplied page tables (example frame numbers)

| Page | Process A's frame | Process B's frame | Row mark |
| --- | --- | --- | --- |
| 0x404 | 0x123 | 0x456 | highlighted row |
| 0x405 | 0x124 | 0x2C7 |  |
| 0x406 | 0x3B1 | not in RAM |  |

## Where in RAM does process B's address 0x405A10 land?

- Source lineage: `lecture#sections.try_translation`
- Citations: none

- Process B uses virtual address 0x405A10.
- Split it, look up B's entry, check it is in RAM, join.
- Write the physical address.

### Supplied page tables (example frame numbers)

| Page | Process A's frame | Process B's frame |
| --- | --- | --- |
| 0x404 | 0x123 | 0x456 |
| 0x405 | 0x124 | 0x2C7 |
| 0x406 | 0x3B1 | not in RAM |

## Process B's address 0x405A10 lands at 0x2C7A10

- Source lineage: `lecture#sections.translation_answer`
- Citations: `dis_13_3`

- Split: page 0x405, offset 0xA10.
- B's table: page 0x405 is in RAM, in frame 0x2C7.
- Join: 0x2C7A10; the offset A10 is unchanged.

### Supplied page tables (example frame numbers)

| Page | Process A's frame | Process B's frame | Row mark |
| --- | --- | --- | --- |
| 0x404 | 0x123 | 0x456 |  |
| 0x405 | 0x124 | 0x2C7 | highlighted row |
| 0x406 | 0x3B1 | not in RAM |  |

## A page not in RAM blocks the process until the disk loads it

- Source lineage: `lecture#sections.page_fault`
- Citations: `dis_13_3`

- Page fault: the trap when a process uses a page that is not in RAM.
- Here page 0x406 is a legal page of B's, stored on disk, and a free frame is available.

### Diagram explanation

Process B uses page 0x406, which is not in RAM; the OS blocks B, loads the page from disk, updates B's page table, makes B Ready, and later gives B the CPU to retry

- Arrow from B uses page 0x406 to Page fault: OS takes over
- Arrow from Page fault: OS takes over to B Blocked; disk loads page
- Arrow from B Blocked; disk loads page to OS updates B's page table
- Arrow from OS updates B's page table to B is Ready
- Arrow from B is Ready to B Running; retries access

## The script spent 5 ms on the clock but 1 ms on the CPU

- Source lineage: `lecture#sections.wall_and_cpu_time`
- Citations: `dis_13_2`

- We know how the OS shares the CPU and keeps memory private.
- We do not yet know how much each program got done.
- Wall time: time on the clock, including Ready and Blocked.
- CPU time: time a process spends Running.

### Who had the CPU from 0 ms to 5 ms in the trace (supplied times)

| Interval | On the CPU | Script's state |
| --- | --- | --- |
| 0 to 1 ms | Script | Running |
| 1 to 3 ms | Browser | Blocked |
| 3 to 4 ms | Shell | Blocked |
| 4 to 5 ms | Shell | Ready |

## The OS shares one machine by taking turns and keeping memory private

- Source lineage: `lecture#sections.close`
- Citations: none

- The OS runs when a program asks or a device signals.
- Ready processes wait for the CPU; Blocked processes wait for an event.
- Each process's page table keeps its memory private.
- A process that waits gives the CPU to one that is ready.

Of the three programs, which ones made progress during the script's disk read, and what was doing the work for each?

**Response mode:** discussion

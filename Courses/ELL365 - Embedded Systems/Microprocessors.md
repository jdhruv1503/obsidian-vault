# Microcontrollers

- **Microcontroller**: A device that integrates multiple components of a microprocessor system onto a single microchip.
- Combines onto the same microchip:
  - The CPU core
  - Memory (both ROM and RAM)
  - Parallel digital I/O and more.

![[Pasted image 20250222024125.png]]

---

## Components of a Microcontroller

- **Timer Module**: Allows the microcontroller to perform tasks for specific time periods.
- **Serial I/O Port**: Enables data flow between the microcontroller and other devices (e.g., PC or another microcontroller).
- **ADC (Analog-to-Digital Converter)**: Allows the microcontroller to accept analog input data for processing.

![[Pasted image 20250222024209.png]]

---

## Why Use Microcontrollers?

- **Low Cost and Small Packaging**.
- **Low Power Consumption**.
- **Programmable and Re-programmable**.
- **Extensive I/O Capabilities**.
- **Easy Integration with Circuits**.
- **Ideal for Applications** where cost, power, and space are critical.
- **Single-Purpose Design**.

---

# Basics of Processor Architecture

### Von Neumann Architecture
- Only one bus between CPU and memory.
- RAM and program memory share the same bus and memory, requiring the same bit width.
- **Bottleneck**: Fetching instructions interferes with accessing RAM.

### Harvard Architecture
- Separate program bus and data bus (can be different widths).
- Facilitates **instruction pipelining**.

---

## CISC vs. RISC

### CISC – Complex Instruction Set Computer
- **Large Number of Instructions**: Each performs a different permutation of the same operation.
- **Complex Operations**: Instructions provide for complex tasks.
- **Variable Instruction Formats and Lengths**.
- **Multiple Addressing Modes**.
- **Requires Multiple Cycles for Execution**.

### RISC – Reduced Instruction Set Computer
- **Simple Operations**: Instructions execute in a single cycle.
- **Fixed-Length Instructions**: Facilitates instruction pipelining.
- **Large General-Purpose Register Set**: Can hold data or addresses.
- **Load-Store Architecture**: No memory access for data processing instructions.

---

# ARM Architecture

### History
- Developed at Acorn Computer Limited, Cambridge, England (1983–1985).
- RISC concept introduced in 1980 at Stanford and Berkeley.
- ARM Limited founded in 1990.
- **ARM Cores**: Licensed to partners for developing and fabricating new microcontrollers.
- **Soft-Core**: Implemented in software.

### ARM Architecture Features
- Based on RISC architecture with enhancements for embedded applications.
- **Large Uniform Register File**.
- **Load-Store Architecture**: Data processing operations operate only on register contents.
- **Uniform and Fixed-Length Instructions**.
- **32-bit Processor**: Instructions are 32-bit long.
- **High Speed/Power Consumption Ratio**.
- **High Code Density**.

### Enhancements to Basic RISC
- **ALU and Shifter Control**: Maximizes usage for every data processing operation.
- **Auto-Increment and Auto-Decrement Addressing Modes**: Optimizes program loops.
- **Load and Store Multiple Instructions**: Maximizes data throughput.
- **Conditional Execution**: Maximizes execution throughput.

### Core Data Path
- Data items are placed in the register file.
- No data processing instructions directly manipulate memory data.
- Instructions typically use two source registers and one result/destination register.
- **Barrel Shifter**: Pre-processes data before it enters the ALU.
- **Increment/Decrement Logic**: Updates register content for sequential access independent of the ALU.

(Deepseek) - **What is a barrel shifter?**  
A barrel shifter is a digital circuit that can shift or rotate data by a specified number of bits in a single operation, enabling efficient data manipulation.

![[Pasted image 20250222045944.png]]

---

# Registers in ARM

- **General-Purpose Registers**: Hold either data or addresses.
- **32-bit Registers**: All registers are 32 bits wide.
- **User Mode**: 16 data registers and 2 status registers are visible.

### Special Registers
- **r13 (Stack Pointer)**: Points to the top of the stack.
- **r14 (Link Register)**: Stores the return address when a subroutine is called.
- **r15 (Program Counter)**: Holds the address of the next instruction to execute.

- **r13 (Stack Pointer)**: Manages the stack, which is used for function calls, local variables, and saving registers.
- **r14 (Link Register)**: Stores the return address for function calls, enabling the program to return to the correct location after a subroutine completes.
- **r15 (Program Counter)**: Tracks the current execution point in the program, pointing to the next instruction to execute.

- **r13 and r14**: Can also be used as general-purpose registers depending on the context.
- **Status Registers**:
  - **CPSR (Current Program Status Register)**: Monitors and controls internal operations.
  - **SPSR (Saved Program Status Register)**: Saves the CPSR when entering an exception or interrupt.

### Program Counter (r15)
- In ARM state:
  - All instructions are 32-bit wide.
  - All instructions are word-aligned.
  - PC value is stored in bits 31:2, with bits 1:0 undefined.

### Status Registers
- **CPSR**: Monitors and controls internal operations.

![[Pasted image 20250222050128.png]]

- **N (Negative)**: Set if the result of an operation is negative.
- **Z (Zero)**: Set if the result of an operation is zero.
- **C (Carry)**: Set if an operation results in a carry-out.
- **V (Overflow)**: Set if an operation results in an overflow.
- **I (IRQ Disable)**: Disables IRQ interrupts when set.
- **F (FIQ Disable)**: Disables FIQ interrupts when set.
- **T (Thumb State)**: Indicates Thumb mode execution.
- **Mode Bits**: Specify the current processor mode.

---

# Processor Modes in ARM

- **Processor Modes**: Determine which registers are active and access rights to the CPSR.
- **Privileged Modes**: Full read-write access to the CPSR.
- **Non-Privileged Modes**: Only read access to the control field of the CPSR but read-write access to the condition flags.

### ARM Modes
- **Privileged Modes**:
  - **Abort**: Triggered by a failed memory access attempt.
  - **FIQ (Fast Interrupt Request)**: Handles high-priority interrupts.
  - **IRQ (Interrupt Request)**: Handles normal interrupts.
  - **Supervisor**: State after reset; used for OS kernel execution.
  - **System**: Special user mode with full CPSR access.
  - **Undefined**: Triggered by an undefined instruction.
- **Non-Privileged Mode**:
  - **User**: Used for programs and applications.

### Banked Registers
- **Register File**: Contains 37 registers in total.
- **20 Banked Registers**: Hidden from the program at different times.
- **Banked Registers**: Available only in specific processor modes.
- Each privileged mode (except system mode) has an associated **SPSR** that saves the CPSR when entering the mode and restores it when returning to user mode.

![[Pasted image 20250222050430.png]]

### Mode Changing
- **Mode Changes**: Occur by writing directly to the CPSR or by hardware during exceptions or interrupts.
- **Return to User Mode**: A special return instruction restores the original CPSR and banked registers.

---

# ARM Memory Organisation

![[Pasted image 20250222051024.png]]

---
## ARM Vector/SIMD Instructions
A vector instruction is also known as
a SIMD instruction, since a single
instruction operates on multiple data
words.
Vector instructions implement
massively parallel operations on the
normal register file, manipulating
many registers at once.

## Pipelining

Instruction fetch (IF): The next instruction
is fetched from the I-cache, which behaves
like a synchronous SRAM, in that the
address must be provided at least one
cycle before the instruction comes out.

Instruction decode (ID) or register fetch
(RF): Designs vary according to how the
instruction decode is precisely pipelined,
but there is at least one cycle between the
instruction arriving and the register data
being input into the ALU.

Execute (EX): The ALU combines the
operands from the register file. The
register on the arithmetic and logic unit
(ALU) result bus makes this a pipeline
stage.

Memory access (MA): Again, since read
access to the data cache has a latency of
two, the memory access pipeline stage
actually takes two cycles. However, in some
designs, this is a single-cycle operation

Writeback (WB): The writeback stage is
just padding to equalise the delay from the
output of the ALU for the computed results
with the data cache load delay (assuming a
two-cycle data cache). The padding ensures
that load and arithmetic/logic instructions
can be placed back-to-back and that at
most one register in the main file needs to
be written per clock cycle.

(Deepseek)- explain writeback better

---
## Hazards
A data hazard occurs when the result
of a computation or load is not in its
intended place in the register file
until two cycles later.
This is solved for ALU operations with
the two forwarding paths from the
output of the ALU and the output of
the writeback register.

For loads, it cannot be solved for
immediate use and a pipeline bubble
is needed (the compiler optimiser
should minimise this use pattern as
far as possible), but it can be
forwarded from two cycles before.
The forwarding multiplexors use
simple pattern matching across
successive stages of the instruction
pipeline

(Deepseek)- rewrite this to  be much easier to understand

---
## Super Scalar Processor
A super-scalar processor can execute
more than one instruction per clock
cycle (IPC).
The average is around 2 or 3 with a peak
of 6, depending on the nature of the
program being run. However,
First-generation microprocessors required several
clock cycles for each bus cycle. Since the average
number of cycles per instruction was at least one,
performance was measured in clock cycles per
instruction (CPI), which is the reciprocal of the
IPC.

### Architecture

The execution unit consists of some
number of reservation stations. These
can be identical in structure, but often
are specialised to some degree.
The diagram shows four types: load,
store, integer ALU and floating-point
ALU, but designs vary greatly in detail.

Although essential for the instruction
cache, it is usual for the data cache to
also support super-scalar operation.
The diagram shows paths for three
transactions on the data cache per
clock cycle.

Instructions are placed in empty reservation stations.
Complex matching logic triggers a reservation station
to execute its saved instruction as soon as its operands
are ready.
Execution will typically take several clock cycles for a
complex instruction, such as a 64-bit multiply or
floating-point add. A reservation station is not typically
fully pipelined, so that it is busy until the result is ready.
The provisioning mix of reservation stations must
match the mix of instructions commonly found in
programs.
For a design to have a balanced throughput, this mix
must be weighted by how long a reservation station is
busy per operation. Generally, every fifth instruction is
a branch and every third is a load or store. Loads are at
least twice as common as stores.

When a result is ready, the reservation station or
stations that depend on the new result become
ready to run.
These will not necessarily be the next instructions in
the original program sequence. Hence, we achieve
out-of-order instruction execution.
In general, the execution times for different
instructions are neither equal nor predictable.
Load and store stations have a variable execution
time due to the complex behaviour of caches and
DRAM.
By executing an instruction as soon as its operands
are ready, the hardware dynamically adapts to the
behaviour of memory.

Reservation stations operate on virtual registers, not
the physical registers. A register could be updated
several times within the window of instructions
spread out within the core.
This can often be tens of instructions. This is possible
only if several virtual registers alias one P register,
which is called register renaming.
The mapping at any instant is held in complex
scoreboarding logic.
Move operations that merely transfer data between registers
can be implemented as just an update to the scoreboard
structure, if is clear from the forward instruction window that
only one copy needs to be kept.

The multiple ALUs and copies of registers in a
super-scalar processor have a correspondingly
larger number of control inputs compared with a
simple processor that only has one instance, or
fewer instances, of each component.
Rather than on-the-fly parallelism discovery within
a standard instruction stream, the main
alternative is to use a very long instruction word
(VLIW) processor. In these architectures, individual
data-path components are explicitly driven in
parallel using wide instructions for which the
concurrency has been statical

(Deepseek) - rewrite the above to be easier to understand as well. What are reservation stations? Include that when you rewrite

### MMU

A memory management unit (MMU) translates virtual addresses
into physical addresses. A simplistic TLM diagram is shown in
the top half of Figure 2.5. However, in reality, a decision has to
be made about whether each cache operates on virtual or
physical addresses. A common configuration is shown in the
bottom half of Figure 2.5. Here, the L1 caches operate on virtual
addresses whereas the L2 cache operates on physical addresses.
The figure also shows the necessary secondary connection to
allow the control and configuration registers to be updated by
the core. If an L1 cache operates on virtual addresses, the MMU
is not part of the performance-critical front-side operations.
Using Arm technology, such updates are made via the
coprocessor interface, so the MMU of a core appears as a
coprocessor.

## Multi core

To increase the number of effective processor cores in a CMP,
rather than adding further complete cores, simultaneous
multithreading can be used.
A thread is defined by the presence of a program counter in each
register file, but a full set of all architecturally visible registers is
needed. The set of reservation stations is shared over the threads
within a simultaneously multithreaded core. There are advantages
and disadvantages. Apart from requiring less silicon than a full
new core, a new thread benefits from a statistical multiplexing
gain
In the utilisation of reservation stations, since there is a higher
chance that more of them will be in use at once. A potential
disadvantage is that the L1 caches are shared.
This potentially leads to greater thrashing (capacity evictions)
unless the simultaneously multithreaded cores are running
closely coupled parts of the same program.


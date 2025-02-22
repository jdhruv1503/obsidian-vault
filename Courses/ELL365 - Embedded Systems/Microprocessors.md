# ARM Vector/SIMD Instructions

- **Vector Instructions**: Also known as **SIMD (Single Instruction, Multiple Data)** instructions.
- **Function**: A single instruction operates on multiple data words simultaneously, enabling massively parallel operations on the normal register file.
- **Use Case**: Efficiently manipulates many registers at once, ideal for tasks like image processing, signal processing, and scientific computations.

---

# Pipelining

Pipelining is a technique used to improve processor efficiency by breaking down instruction execution into multiple stages. Each stage handles a specific part of the instruction, allowing multiple instructions to be processed simultaneously.

### Pipeline Stages:
1. **Instruction Fetch (IF)**:
   - The next instruction is fetched from the instruction cache (I-cache).
   - The I-cache behaves like synchronous SRAM, requiring the address to be provided at least one cycle before the instruction is available.

2. **Instruction Decode (ID) / Register Fetch (RF)**:
   - The instruction is decoded, and operands are fetched from the register file.
   - Designs vary, but there is at least one cycle between instruction arrival and register data being input into the ALU.

3. **Execute (EX)**:
   - The ALU performs arithmetic or logic operations on the operands.
   - The result is placed on the ALU result bus, making this a pipeline stage.

4. **Memory Access (MA)**:
   - Data is read from or written to the data cache.
   - In some designs, this stage takes two cycles due to data cache latency.

5. **Writeback (WB)**:
   - The result of the ALU operation or data from memory is written back to the register file.
   - This stage ensures that load and arithmetic/logic instructions can be executed back-to-back without conflicts.

(Deepseek) - **Explain Writeback Better**:
The **Writeback (WB)** stage is the final step in the pipeline where the results of computations or data fetched from memory are written back to the register file. This stage ensures that the results are available for subsequent instructions. It also balances the pipeline by ensuring that only one register is written per clock cycle, preventing conflicts and maintaining smooth operation.

---

# Hazards

A **data hazard** occurs when the result of a computation or load is not available in the register file when needed by a subsequent instruction. This can stall the pipeline, reducing efficiency.

### Types of Hazards:
1. **ALU Operations**:
   - Solved using **forwarding paths** (also called **bypassing**).
   - Forwarding allows the result of an ALU operation to be used immediately by the next instruction, even before it is written back to the register file.

2. **Load Operations**:
   - Cannot be solved for immediate use, as the data is not available until two cycles later.
   - A **pipeline bubble** (stall) is introduced to wait for the data.
   - Forwarding can be used from two cycles before to minimize delays.

(Deepseek) - **Simplified Explanation**:
Data hazards happen when an instruction needs data that isn’t ready yet. For ALU operations, the processor uses **forwarding** to pass the result directly to the next instruction. For load operations, the processor might need to wait (insert a bubble) or use forwarding from earlier stages to avoid delays.

---

# Superscalar Processor

A **superscalar processor** can execute more than one instruction per clock cycle (IPC). The average IPC is around 2 or 3, with peaks of 6, depending on the program.

### Architecture:
- **Reservation Stations**: Specialized units that hold instructions until their operands are ready.
  - Types: Load, store, integer ALU, and floating-point ALU.
  - Each station is designed to handle specific types of instructions.
- **Execution Units**: Multiple ALUs and other functional units to execute instructions in parallel.
- **Data Cache**: Supports multiple transactions per clock cycle to keep up with the high throughput.

### How It Works:
1. **Instruction Dispatch**:
   - Instructions are placed in empty reservation stations.
2. **Operand Matching**:
   - Complex logic triggers a reservation station to execute its instruction as soon as its operands are ready.
3. **Out-of-Order Execution**:
   - Instructions are executed as soon as their operands are ready, not necessarily in the original program order.
4. **Register Renaming**:
   - Virtual registers are used to handle multiple updates to the same physical register within a window of instructions.
   - A **scoreboard** keeps track of the mapping between virtual and physical registers.

Reservation stations are temporary storage units within a superscalar processor that hold instructions until their operands are ready. They allow the processor to execute instructions out of order, improving efficiency by keeping the execution units busy.

### Key Points:
- **Balanced Throughput**: The mix of reservation stations must match the mix of instructions in programs.
- **Common Instruction Mix**:
  - Every fifth instruction is a branch.
  - Every third instruction is a load or store (loads are twice as common as stores).
- **Out-of-Order Execution**: Enables the processor to adapt dynamically to memory behavior and instruction dependencies.

---

# Memory Management Unit (MMU)

- **MMU**: Translates **virtual addresses** (used by software) into **physical addresses** (used by hardware).
- **Cache Configuration**:
  - **L1 Caches**: Often operate on virtual addresses for faster access.
  - **L2 Cache**: Operates on physical addresses.
- **Control Registers**: Updated via the coprocessor interface in ARM architectures.

---

# Multicore Processors

- **Multicore Processors**: Contain multiple processor cores on a single chip to increase performance.
- **Simultaneous Multithreading (SMT)**:
  - Allows a single core to execute multiple threads simultaneously.
  - **Advantages**:
    - Better utilization of reservation stations.
    - Requires less silicon than adding a full new core.
  - **Disadvantages**:
    - Shared L1 caches can lead to **thrashing** (frequent evictions due to limited cache capacity).

### Key Concepts:
- **Thread**: Defined by a program counter and a set of architecturally visible registers.
- **Statistical Multiplexing**: Improves utilization of resources by sharing them across multiple threads.
- **Closely Coupled Programs**: SMT works best when threads are part of the same program, reducing cache conflicts.

---

# Summary of Key Terms

- **Vector/SIMD Instructions**: Perform parallel operations on multiple data words.
- **Pipelining**: Breaks instruction execution into stages for efficiency.
- **Data Hazards**: Occur when data isn’t ready for an instruction; solved by forwarding or stalling.
- **Superscalar Processors**: Execute multiple instructions per cycle using reservation stations and out-of-order execution.
- **Reservation Stations**: Hold instructions until operands are ready, enabling out-of-order execution.
- **MMU**: Translates virtual addresses to physical addresses.
- **Multicore Processors**: Use multiple cores and SMT to increase performance.

---
# ARM Exceptions and Interrupts


Exception - Mode
Fast Interrupt
RequestFIQ
Interrupt RequestIRQ
SWI and ResetSVC
Pre-fetch Abort &
Data Abortabort
Undefined
instructionundefined

When Exception Occurs…
Exception causes mode change and
Saves the cpsr to the spsr of the
exception mode
Saves the pc to the lr of the exception
mode
Sets the cpsr to the exception mode
Sets pc to the address of the exception
handler

Vector Table
Vector table – a table of addresses that the
ARM core branches
Fixed offset for each type of exception
These addresses contain instructions of
one of the following forms:
B address: branching relative to PC
LDR pc, \[pc, \#offset\] : loads handler address
from memory to PC
MOV PC, \#immediate : loads immediate value
into PC

### Exception priorities

![[Pasted image 20250222053919.png]]

Exception Handlers
Reset handler
Initializes the system, setting up stack pointers,
memory, external interrupt sources before
enabling IRQ or FIQ
Code should be designed to avoid further
triggering of exceptions
Data Abort
Occurs when memory controller indicates that
an invalid memory address has been accessed
An FIQ exception can be raised within data
abort handler

Exception Handlers
Reset handler
Initializes the system, setting up stack pointers,
memory, external interrupt sources before
enabling IRQ or FIQ
Code should be designed to avoid further
triggering of exceptions
Data Abort
Occurs when memory controller indicates that
an invalid memory address has been accessed
An FIQ exception can be raised within data
abort handler

Prefetch Abort
Occurs when an attempt to fetch an instruction
results in memory fault
FIQ exception can be serviced
Undefined instruction
Occurs when an instruction is not in the ARM or
Thumb instruction
SWI and undefined instruction have the
same level of priority because they cannot
occur together

Returning from Exception
Handler
Exception handler must not corrupt lr
After servicing is complete, return to
normal execution occurs
By moving the correct value of link
register r14 into pc
By restoring cpsr from spsr

Interrupt Assignment
An interrupt controller connects
multiple external interrupts to either
FIQ or IRQ
IRQ are normally assigned to general
purpose interrupts
Example: periodic timer interrupt to
force a context switch
FIQ is reserved for an interrupt
source which requires fast response
time

Interrupt Latency
Hardware and software latency
Software methods to reduce latency
Nested handler which allows further
interrupts to occur even when servicing
an existing interrupt by re-enabling the
interrupts inside service routine
Program interrupt controller to ignore
interrupts of same or lower priority
Higher priority interrupts will have lower
average latency

Stack Organization
For each processor mode stack has to be
set up
To be done every time processor is reset
Change to each mode by storing CPSR bit pattern and
initialise sp
Design decisions
Location and mode (descending stack is
common)
Size
Nested interrupt handler requires larger stack

(Deepseek) - rewrite this interrupt section to be easier to read. What is the difference between different interrupts?

---
# ARM I/O

Handles all I/O devices using memory
mapped I/O
Interrupt support:
Fast interrupt
Normal interrupt
DMA Support
Large bandwidth data transfer

ARM CPU Core
Processor Core + Cache + MMU

## Designs

ARM 7 Processor Core
Low-end ARM core for applications like
mobile phones
TDMI
T : Thumb
D : On chip debug support enabling processor
to halt in response to debug request
M : Enhanced multiplier, yield a full 64-bit result
I : Embedded ICE Hardware
Von Neumann architecture
3 stage pipeline, CPI ~ 1.9

Pipeline Operation
•Not always cycle per instruction completion
–Example: LDMIA r0, \[r2,r3\] (multiple load):
–2 registers to load , instruction in execution
for two cycles
–Execution of Prefetched instruction delayed
Branch, Subroutine call, Exceptions effect
pipeline efficiency

(Deepseek) - Give FIQ pipeline example


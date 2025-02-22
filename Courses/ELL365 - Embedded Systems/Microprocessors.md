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

(Deepseek) - **Type the exact bits in CPSR for ARM and their roles**:
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

(Deepseek) - **What Are Reservation Stations?**:
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

## What Are Exceptions and Interrupts?
- **Exceptions**: Events that disrupt the normal flow of program execution, such as invalid memory access or undefined instructions.
- **Interrupts**: Signals from external devices (e.g., timers, peripherals) that request the processor's attention.

### ARM Exception Modes
- **FIQ (Fast Interrupt Request)**: High-priority interrupt for time-critical tasks.
- **IRQ (Interrupt Request)**: Normal-priority interrupt for general-purpose tasks.
- **SVC (Supervisor Call)**: Used for software interrupts (e.g., system calls).
- **Abort**: Triggered by invalid memory access (prefetch or data abort).
- **Undefined**: Triggered by an undefined instruction.

---

## When an Exception Occurs
1. **Mode Change**:
   - The processor switches to the corresponding exception mode.
2. **Save CPSR**:
   - The current program status register (CPSR) is saved to the saved program status register (SPSR) of the exception mode.
3. **Save PC**:
   - The program counter (PC) is saved to the link register (LR) of the exception mode.
4. **Set CPSR**:
   - The CPSR is updated to reflect the exception mode.
5. **Jump to Handler**:
   - The PC is set to the address of the exception handler.

---

## Vector Table
- **Definition**: A table of addresses that the ARM core branches to when an exception occurs.
- **Fixed Offsets**: Each exception type has a fixed offset in the vector table.
- **Handler Instructions**:
  - `B address`: Branch to a relative address.
  - `LDR pc, [pc, #offset]`: Load the handler address from memory.
  - `MOV PC, #immediate`: Load an immediate value into the PC.

---

## Exception Priorities
- Exceptions have predefined priorities to determine which handler runs first if multiple exceptions occur simultaneously.

![[Pasted image 20250222053919.png]]

---

## Exception Handlers

### Reset Handler
- **Function**: Initializes the system (e.g., sets up stack pointers, memory, and external interrupts).
- **Goal**: Avoid triggering further exceptions during initialization.

### Data Abort Handler
- **Trigger**: Occurs when an invalid memory address is accessed.
- **FIQ Support**: An FIQ exception can be raised within the data abort handler.

### Prefetch Abort Handler
- **Trigger**: Occurs when an instruction fetch results in a memory fault.
- **FIQ Support**: FIQ exceptions can still be serviced.

### Undefined Instruction Handler
- **Trigger**: Occurs when an instruction is not recognized by the ARM or Thumb instruction set.
- **Priority**: Same as SWI (software interrupt) since they cannot occur simultaneously.

---

## Returning from Exception Handlers
- **Restore PC**: Move the correct value from the link register (LR) to the program counter (PC).
- **Restore CPSR**: Restore the CPSR from the SPSR of the exception mode.

---

## Interrupt Assignment
- **Interrupt Controller**: Connects multiple external interrupts to either FIQ or IRQ.
- **IRQ**: Assigned to general-purpose interrupts (e.g., periodic timer interrupts).
- **FIQ**: Reserved for interrupts requiring fast response times.

---

## Interrupt Latency
- **Definition**: The time between an interrupt request and the start of its handler.
- **Reducing Latency**:
  - **Nested Handlers**: Allow higher-priority interrupts to be serviced while handling a lower-priority one.
  - **Priority Control**: Program the interrupt controller to ignore interrupts of the same or lower priority.

---

## Stack Organization
- **Stack Setup**: Each processor mode has its own stack.
- **Initialization**: Stacks must be set up after every reset.
- **Design Decisions**:
  - **Location and Mode**: Descending stacks are common.
  - **Size**: Larger stacks are needed for nested interrupt handlers.

---

# ARM I/O

## Memory-Mapped I/O
- **Definition**: All I/O devices are accessed as if they were memory locations.
- **Features**:
  - **Interrupt Support**: FIQ and IRQ for handling I/O events.
  - **DMA Support**: Enables large bandwidth data transfers without CPU intervention.

---

# ARM CPU Core

## ARM7 Processor Core
- **Target Applications**: Low-end devices like mobile phones.
- **Features**:
  - **TDMI**:
    - **T**: Thumb instruction set for improved code density.
    - **D**: On-chip debug support.
    - **M**: Enhanced multiplier for 64-bit results.
    - **I**: Embedded ICE hardware for debugging.
  - **Von Neumann Architecture**: Shared bus for instructions and data.
  - **3-Stage Pipeline**: CPI (cycles per instruction) ~1.9.

### Pipeline Operation
- **Not Always 1 CPI**: Some instructions (e.g., `LDMIA`) take multiple cycles.
- **Pipeline Stalls**: Branches, subroutine calls, and exceptions reduce pipeline efficiency.

---

## ARM9TDMI

### Harvard Architecture
- **Separate Instruction and Data Buses**: Allows simultaneous access to instruction and data memory.
- **5-Stage Pipeline**: Improves CPI to ~1.5 and increases clock frequency.

### 5-Stage Pipeline
1. **Fetch**: Fetch the next instruction from memory.
2. **Decode**: Decode the instruction and fetch operands.
3. **Execute**: Perform arithmetic or logic operations.
4. **Buffer Data**: Prepare data for memory access.
5. **Writeback**: Write results back to the register file.

![[Pasted image 20250222054257.png]]

---

## DSP Enhancements in ARM9E

### New Instructions (V5TE)
- **32x16 and 16x16 Multiply**:
  - `SMLAxy`, `SMLAWy`, `SMLALxy`, `SMULxy`, `SMULWy`.
- **Packed 16-bit Operands**: Efficient use of 32-bit registers for 16-bit data.
- **Saturating Arithmetic**:
  - `QADD`, `QSUB`, `QDADD`, `QDSUB`: Prevent overflow by saturating results.

### Count Leading Zeros (CLZ)
- **Function**: Speeds up normalization and division operations.

---

## ARM v5TEJ
- **Java Support**: Hardware and software acceleration for Java bytecode execution.

---

## ARM v6 Architecture

### SIMD Instructions
- **Function**: Perform parallel operations on multiple data words.
- **Example**: `QADD8<cond> Rd, Rn, Rm` (signed saturating 8-bit SIMD add).

### Other Features
- **Sum of Absolute Differences**: `UASAD8<cond> Rd, Rm, Rs`.
- **Dual 16x16 Multiply**: Efficient multiplication for DSP tasks.
- **Cryptographic Multiplication**: Supports encryption algorithms.
- **Multiprocessing Synchronization**: Primitives for multi-core systems.

---

# Summary of Key Concepts

1. **Exceptions and Interrupts**:
   - Exceptions handle errors and system events.
   - Interrupts handle external device requests.
   - FIQ is high-priority; IRQ is normal-priority.

2. **Vector Table**:
   - Contains addresses for exception handlers.

3. **Pipeline**:
   - ARM7: 3-stage pipeline (CPI ~1.9).
   - ARM9: 5-stage pipeline (CPI ~1.5).

4. **DSP Enhancements**:
   - New multiply and saturating arithmetic instructions.

5. **ARM v6**:
   - SIMD instructions for parallel data processing.
   - Cryptographic and multiprocessing support.
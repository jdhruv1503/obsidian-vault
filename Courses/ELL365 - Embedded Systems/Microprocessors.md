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
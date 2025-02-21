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

(Deepseek) - **What is the role of these 3 in detail?**  
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

-
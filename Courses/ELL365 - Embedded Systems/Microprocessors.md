
- Basically, a micro-controller is a device
which integrates a number of the
components of a microprocessor system
onto a single microchip.

A micro-controller combines onto the same
microchip:
The CPU core
Memory (both ROM and RAM)
Some parallel digital I/O & more

![[Pasted image 20250222024125.png]]

---

## Components of a microcontroller

- A Timer module to allow the micro-
controller to perform tasks for certain time
periods.
- A serial I/O port to allow data to flow
between the micro-controller and other
devices such as a PC or another micro-
controller.
- An ADC to allow the micro-controller to
accept analogue input data for processing.

![[Pasted image 20250222024209.png]]

Why Micro-controller?
Low cost, small packaging
Low power consumption
Programmable, re-programmable
Lots of I/O capabilities
Easy integration with circuits
For applications in which cost, power and
space are critical
Single-purpose

---
## Basics of processor architecture

VonNeuman Architecture
Only one bus between CPU and
memory
RAM and program memory share the
same bus and the same memory, and
so must have the same bit width
Bottleneck: Getting instructions
interferes with accessing RAM

Harvard Architecture
Separate program bus and data
bus: can be different widths!
Instruction Pipelining easy

### CISC and RISC

CISC – Complex Instruction
Set Computer
A large number of instructions each
carrying out a different permutation of
the same operation
Instructions provide for complex
operations
Different instructions of different format
Different instructions of different length
Different addressing modes
Requires multiple cycles for execution

RISC – Reduced Instruction
Set Computer
Instructions for simple operations that can
be executed in a single cycle
Each instruction of fixed length
Facilitates instruction pipelining
Large general purpose register set
Can contain data or address (symmetry)
Load-store Architecture
No memory access for data processing
instructions

---
## ARM

History
ARM was developed at Acron Computer
Limited of Cambridge, England between
1983 & 1985
RISC concept introduced in 1980 at Stanford
and Berkley
ARM Limited founded in 1990
ARM Cores
Licensed to partners to develop and fabricate
new micro-controllers
Soft-core

ARM Architecture
Based upon RISC Architecture with
enhancements to meet requirements of
embedded applications
A large uniform register file
Load-store architecture, where data processing
operations operate on register contents only
Uniform and fixed length instructions
32-bit processor
Instructions are 32-bit long
Good Speed/Power Consumption Ratio
High Code Density

Enhancement to Basic RISC
Features
Control over ALU and shifter for every data
processing operations to maximize their
usage
Auto-increment and auto-decrement
addressing modes to optimize program
loops
Load and Store Multiple instructions to
maximize data throughput
Conditional Execution of instruction to
maximize execution throughput

Overview: Core Data Path
Data items are placed in register file
No data processing instructions directly
manipulate data in memory
Instructions typically use two source
registers and single result or destination
registers
A Barrel shifter on the data path can pre-
process data before it enters ALU
Increment/decrement logic can update
register content for sequential access
independent of ALU

(Deepseek) - What is a barrel shifter?

![[Pasted image 20250222045944.png]]

---
## Registers in ARM

General Purpose registers hold either data or
address
All registers are of 32 bits
In user mode 16 data registers and 2 status
registers are visible

Data registers: r0 to r15
Three registers r13, r14, r15 perform special
functions
r13: stack pointer
r14: link register ( where return address is put
whenever a subroutine is called)
r15: program counter

(Deepseek)- What is the role of these 3 in detail?

Depending upon context, registers
r13 and r14 can also be used as GPR
Any instruction which use r0 can as
well be used with any other GPR (r1-
r13)
In addition, there are two status
registers
CPSR: current program status register
SPSR: saved program status register

For r15:
When the processor is executing in
ARM state
All instructions are 32 bit wide
All instructions are word aligned
PC value is stored in bits 31:2 with bits
1:0 undefined

Status registers:
CPSR: monitors and control internal
operations

![[Pasted image 20250222050128.png]]

()
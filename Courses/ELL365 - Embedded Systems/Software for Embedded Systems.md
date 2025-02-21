## OS

Exploits the hardware resources of
one or more processors
Provides a set of services to system
users
Manages memory (primary and
secondary) and I/O devices

Small, simple systems usually don't have
an OS
Instead, an application consists of an
infinite loop that calls modules (functions)
to perform various actions in the
“Background”.
Interrupt Service Routines (ISRs) handle
asynchronous events in the “Foreground”

![[Pasted image 20250222042741.png]]

---
## Multitasking

Multitasking - The ability to execute
more than one task or program at the
same time

CPU switches from one program to
another so quickly that it gives the
appearance of executing all of the
programs at the same time.

Cooperative multitasking -each program
can control the CPU for as long as it needs
it. It allows other programs to use it at
times when it does not use the CPU

Preemptive multitasking - Operating
system parcels out CPU time slices to each
program

---
## System fundamentals

Wake Up Call -On Power_ON

Execute BIOS - instructions kept in Flash
Memory - type of read-only memory
(ROM) examines system hardware

Power-on self test (POST) checks the CPU,
memory and basic input-output systems
(BIOS) for errors and stores the result in a
special memory location

![[Pasted image 20250222042918.png]]

### Board Support Package

BSP is a component that provides
board/hardware-specific details to OS
for the OS to provide hardware-
abstractions to the tasks that use its
services

BSP is specific to both the board and
the OS for which it is written

### BSP Startup

Initializes processor

Sets various parameters required by
processor for
Memory initialization
Clock setup
Setting up various components like
cache

BSP also contains drivers for
peripherals

---
## Kernel

Most frequently used portion of OS

Resides permanently in main
memory

Runs in privileged mode

Responds to calls from processes and
interrupts from devices

Responsibilities include:
Managing Processes
Context switching: alternating between the
different processes or tasks
Scheduling: deciding which task/process to
run next
Critical sections = providing adequate
memory-protection when multiple
tasks/processes run concurrently
Various solutions to dealing with critical
sections

---
## Process

A process is a unique sequential execution
of a program

Execution context of the program:
All information the operating system needs to
manage the process

Process Characteristics
Period - Time between successive executions
Rate - Inverse of Period
In a multi-rate system each process executes at
its own rate


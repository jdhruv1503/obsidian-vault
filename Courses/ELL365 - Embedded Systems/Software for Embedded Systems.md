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

### Process Control Block

OS structure which holds the pieces of
information associated with a process

Process state: new, ready, running, waited,
halted, etc.
Program counter: contents of the PC
CPU registers: contents of the CPU
registers
CPU scheduling information: information
on priority and scheduling parameters
Memory­ management information:
Pointers to page or segment tables
Accounting information: CPU and real
time used, time limits, etc.
I/O status information: which I/O
devices (if any) this process has
allocated to it, list of open files, etc.

![[Pasted image 20250222043224.png]]

---
### Multithreading

Operating system supports multiple
threads of execution within a single
process
An executing process is divided into
threads that run concurrently
Thread – a dispatchable unit of work

![[Pasted image 20250222043256.png]]

Lightweight processes are ideal for
embedded computing systems since
these platforms typically run only a
few programs

Concurrency within programs better
managed using threads

### Context switching

The CPU’s replacement of the currently
running task with a new one is called a
“context switch”

Simply saves the old context and “restores”
the new one

Actions:
Current task is interrupted
Processor’s registers for that particular task are
saved in a task-specific table
Task is placed on the “ready” list to await the
next time-slice
Task control block stores memory usage,
priority level, etc.
New task’s registers and status are
loaded into the processor
New task starts to run
Involves changing the stack pointer, the PC
and the PSR (program status register)

In ARM:
Saving context

STMIA
r13, {r0-r14}^ 
save all user registers
in space pointed to
by r13 in ascending
order

MRS r0, SPSR
get status register and put
it in r0

STMDB r13, {r0, r15}
save status register
and PC into context
block

Loading a new process
ADR r0, NEWPROC ;get address for pointer
LDR r13, r0;Load next context block in r13
LDMDB r13, {r0, r14};Get status register and PC
MSR SPSR, r0;Set status register
LDMIA r13, {r0-r14}^;Get the registers
MOVS pc, r14;Restore status register

### When does a context switch occour?

Time-slicing
Time-slice: period of time a task can run before
a context-switch can replace it
Driven by periodic hardware interrupts from
the system timer
During a clock interrupt, the kernel’s scheduler
can determine if another process should run
and perform a context-switch
However, this doesn’t mean that there is a
context-switch at every time-slice!

Preemption:
Currently running task can be halted and
switched out by a higher-priority active
task
No need to wait until the end of the
time-slice

Frequency of context switch depends upon
application

Overhead for Processor context-switch is
the amount of time required for the CPU to
save the current task’s context and restore
the next task’s context

Overhead for System context-switch is the
amount of time from the point that the
task was ready for context-switching to
when it was actually swapped in

System context-switch time is a measure
of responsiveness

Time-slicing: a time-slice period +
processor context-switch time
Preemption is mostly preferred because it
is more responsive (system context-switch
= processor context-switch)

## Scheduler

What is the scheduler?
Part of the operating system that
decides which process/task to run next
Uses a scheduling algorithm that
enforces some kind of policy that is
designed to meet some criteria

Criteria may vary
CPU utilization- ­keep the CPU as busy as
possible
Throughput- ­maximize the number of
processes completed per time unit
Turnaround time- ­minimize a process’ latency
(run time), i.e., time between task submission
and termination
Response time- ­minimize the wait time for
interactive processes
Real-time- ­must meet specific deadlines to
prevent “bad things” from happening

---
## Scheduling policies

First­come, first­served (FCFS)
The first task that arrives at the request queue
is executed first, the second task is executed
second and so on

FCFS can make the wait time for a process very
long

Shortest Job First: Schedule processes
according to their run-times
Generally difficult to know the run-time of a
process ahead of time

Priority Scheduling
Shortest­Job­First is a special case of
priority scheduling
Priority scheduling assigns a priority
to each process. Those with higher
priorities are run first.

Real time scheduling
Characteristics of Real-Time
Systems:
Event-driven, reactive.
High cost of failure.
Concurrency/multiprogramming.
Stand-alone/continuous operation.
Reliability/fault-tolerance
requirements.
Predictable behavior.

Use interrupts every time period T to evaluate and process something

More complicated control systems have
multiple sensors and actuators and must
support control loops of different rates.

Example:Helicopter flight controller

![[Pasted image 20250222043920.png]]

### Process parameters

Release time of a job: The time instant the
job becomes ready to execute.

Absolute Deadline of a job: The time
instant by which the job MUST complete
execution.

Relative deadline of a job:
Deadline - Release time

Response time of a job:
Completion time - Release time

Each job $J_i$ is characterized by its release
time $r_i$, absolute deadline $d_i$, relative
deadline $D_i$, and execution time $e_i$.


### Task types

Periodic task:
We associate a period Pi with each task Ti.
Pi is the interval between job releases.

Sporadic and Aperiodic tasks: Released at
arbitrary times.
Sporadic: Has a hard deadline.
Aperiodic: Has no deadline or a soft deadline.


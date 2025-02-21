
# OS

- Exploits the hardware resources of one or more processors.
- Provides a set of services to system users.
- Manages memory (primary and secondary) and I/O devices.

Small, simple systems usually don't have an OS. Instead, an application consists of an infinite loop that calls modules (functions) to perform various actions in the "Background". Interrupt Service Routines (ISRs) handle asynchronous events in the "Foreground".

![[Pasted image 20250222042741.png]]

---

# Multitasking

- **Multitasking**: The ability to execute more than one task or program at the same time.
- The CPU switches from one program to another so quickly that it gives the appearance of executing all programs simultaneously.

### Types of Multitasking:
1. **Cooperative Multitasking**: Each program controls the CPU for as long as it needs it. It allows other programs to use the CPU when it is idle.
2. **Preemptive Multitasking**: The operating system parcels out CPU time slices to each program.

---

# System Fundamentals

### Wake-Up Call - On Power_ON
- Execute BIOS: Instructions kept in Flash Memory (a type of read-only memory (ROM)) examine system hardware.
- **Power-On Self Test (POST)**: Checks the CPU, memory, and basic input-output systems (BIOS) for errors and stores the result in a special memory location.

![[Pasted image 20250222042918.png]]

### Board Support Package (BSP)
- **BSP**: A component that provides board/hardware-specific details to the OS for hardware abstractions.
- BSP is specific to both the board and the OS for which it is written.

### BSP Startup
- Initializes the processor.
- Sets various parameters required by the processor for:
  - Memory initialization
  - Clock setup
  - Setting up components like cache
- BSP also contains drivers for peripherals.

---

# Kernel

- The most frequently used portion of the OS.
- Resides permanently in main memory.
- Runs in privileged mode.
- Responds to calls from processes and interrupts from devices.

### Responsibilities:
- Managing Processes
- **Context Switching**: Alternating between different processes or tasks.
- **Scheduling**: Deciding which task/process to run next.
- **Critical Sections**: Providing memory protection when multiple tasks/processes run concurrently.

---

# Process

- A **process** is a unique sequential execution of a program.
- **Execution Context**: All information the OS needs to manage the process.

### Process Characteristics
- **Period**: Time between successive executions.
- **Rate**: Inverse of Period.
- In a multi-rate system, each process executes at its own rate.

### Process Control Block (PCB)
- OS structure that holds information associated with a process.
- **Contents**:
  - Process state: new, ready, running, waited, halted, etc.
  - Program counter: Contents of the PC.
  - CPU registers: Contents of the CPU registers.
  - CPU scheduling information: Priority and scheduling parameters.
  - Memory management information: Pointers to page or segment tables.
  - Accounting information: CPU and real-time used, time limits, etc.
  - I/O status information: Allocated I/O devices, list of open files, etc.

![[Pasted image 20250222043224.png]]

---

# Multithreading

- The OS supports multiple threads of execution within a single process.
- An executing process is divided into threads that run concurrently.
- **Thread**: A dispatchable unit of work.

![[Pasted image 20250222043256.png]]

- **Lightweight Processes**: Ideal for embedded systems since these platforms typically run only a few programs.
- Concurrency within programs is better managed using threads.

---

# Context Switching

- The CPU’s replacement of the currently running task with a new one is called a **context switch**.
- Simply saves the old context and "restores" the new one.

### Actions:
1. Current task is interrupted.
2. Processor’s registers for that task are saved in a task-specific table.
3. Task is placed on the "ready" list to await the next time-slice.
4. Task control block stores memory usage, priority level, etc.
5. New task’s registers and status are loaded into the processor.
6. New task starts to run.
- Involves changing the stack pointer, the PC, and the PSR (program status register).

### In ARM:
- **Saving Context**:
  ```assembly
  STMIA r13, {r0-r14}^  // Save all user registers in space pointed to by r13 in ascending order
  MRS r0, SPSR          // Get status register and put it in r0
  STMDB r13, {r0, r15}  // Save status register and PC into context block
  ```
- **Loading a New Process**:
  ```assembly
  ADR r0, NEWPROC       // Get address for pointer
  LDR r13, r0           // Load next context block in r13
  LDMDB r13, {r0, r14}  // Get status register and PC
  MSR SPSR, r0          // Set status register
  LDMIA r13, {r0-r14}^  // Get the registers
  MOVS pc, r14          // Restore status register
  ```

### When Does a Context Switch Occur?
1. **Time-Slicing**:
   - **Time-Slice**: Period of time a task can run before a context-switch replaces it.
   - Driven by periodic hardware interrupts from the system timer.
   - During a clock interrupt, the kernel’s scheduler determines if another process should run and performs a context-switch.
   - Not every time-slice results in a context-switch.

2. **Preemption**:
   - The currently running task can be halted and switched out by a higher-priority active task.
   - No need to wait until the end of the time-slice.

- **Frequency of Context Switch**: Depends on the application.
- **Overhead**:
  - **Processor Context-Switch**: Time required for the CPU to save the current task’s context and restore the next task’s context.
  - **System Context-Switch**: Time from when the task was ready for context-switching to when it was actually swapped in.
  - **System Context-Switch Time**: A measure of responsiveness.

- **Time-Slicing**: Time-slice period + processor context-switch time.
- **Preemption**: Preferred for better responsiveness (system context-switch = processor context-switch).

---

# Scheduler

- **Scheduler**: Part of the OS that decides which process/task to run next.
- Uses a scheduling algorithm to enforce policies designed to meet specific criteria.

### Criteria:
- **CPU Utilization**: Keep the CPU as busy as possible.
- **Throughput**: Maximize the number of processes completed per time unit.
- **Turnaround Time**: Minimize a process’ latency (run time).
- **Response Time**: Minimize the wait time for interactive processes.
- **Real-Time**: Meet specific deadlines to prevent failures.

---

# Scheduling Policies

1. **First-Come, First-Served (FCFS)**:
   - The first task that arrives at the request queue is executed first.
   - Can result in long wait times for processes.

2. **Shortest Job First**:
   - Schedule processes according to their run-times.
   - Difficult to know the run-time of a process ahead of time.

3. **Priority Scheduling**:
   - Assigns a priority to each process. Higher-priority processes run first.
   - Shortest-Job-First is a special case of priority scheduling.

4. **Real-Time Scheduling**:
   - **Characteristics of Real-Time Systems**:
     - Event-driven, reactive.
     - High cost of failure.
     - Concurrency/multiprogramming.
     - Stand-alone/continuous operation.
     - Reliability/fault-tolerance requirements.
     - Predictable behavior.
   - Use interrupts every time period T to evaluate and process something.
   - Example: Helicopter flight controller.

![[Pasted image 20250222043920.png]]

---

# Process Parameters

- **Release Time of a Job**: The time instant the job becomes ready to execute.
- **Absolute Deadline of a Job**: The time instant by which the job MUST complete execution.
- **Relative Deadline of a Job**: Deadline - Release time.
- **Response Time of a Job**: Completion time - Release time.
- Each job \( J_i \) is characterized by its release time \( r_i \), absolute deadline \( d_i \), relative deadline \( D_i \), and execution time \( e_i \).

---

# Task Types

1. **Periodic Task**:
   - Associated with a period \( P_i \).
   - \( P_i \) is the interval between job releases.

2. **Sporadic and Aperiodic Tasks**:
   - Released at arbitrary times.
   - **Sporadic**: Has a hard deadline.
   - **Aperiodic**: Has no deadline or a soft deadline.

---

# Scheduling for Real-Time Tasks

- **Schedulability**: The ability of tasks to meet all hard deadlines.
- **Latency**: The worst-case system response time to events.
- **Stability in Overload**: The system meets critical deadlines even if all deadlines cannot be met.

### Policies:
1. **Cyclic Executive**:
   - All tasks are fit into a common period (major frame) and executed non-preemptively.

2. **Rate Monotonic**:
   - Preemptive scheduling where tasks are assigned priorities based on rates or periods.

3. **Deadline Monotonic**:
   - Preemptive execution where tasks are assigned priorities based on fixed deadlines.

4. **Earliest Deadline First**:
   - Preemptive scheduling where tasks are assigned priorities dynamically based on the closeness of current deadlines.

5. **Least Laxity First**:
   - Preemptive scheduling where tasks are assigned priorities dynamically based on the amount of slack time remaining.

### Goals:
- Achieve high utilization while guaranteeing hard deadlines.
- Provide fast aperiodic response while guaranteeing hard deadlines.
- Low overhead: Fixed versus dynamic priorities.
  - Dynamic policies tend to have higher overhead due to reordering the run queue at each scheduling decision.
# Operating Systems --- Complete OA & Interview Notes

> **Goal:** Learn OS concepts in simple language, understand *why* they
> work, solve numerical/OA questions, and answer common interview
> questions.
>
> **How to use this README:** First understand the idea → study the
> example → memorize the comparison/formula → solve the quick OA
> pattern.

------------------------------------------------------------------------

# Table of Contents

1.  [What is an Operating System?](#1-what-is-an-operating-system)
2.  [OS Services and System
    Components](#2-os-services-and-system-components)
3.  [User Mode vs Kernel Mode](#3-user-mode-vs-kernel-mode)
4.  [Types of Operating Systems](#4-types-of-operating-systems)
5.  [OS Structures and Kernel Types](#5-os-structures-and-kernel-types)
6.  [Boot Process](#6-boot-process)
7.  [System Calls](#7-system-calls)
8.  [Process Concept](#8-process-concept)
9.  [Process Control Block](#9-process-control-block)
10. [Process States and Schedulers](#10-process-states-and-schedulers)
11. [Process Creation: fork, exec,
    wait](#11-process-creation-fork-exec-wait)
12. [Zombie, Orphan and Daemon
    Processes](#12-zombie-orphan-and-daemon-processes)
13. [Threads](#13-threads)
14. [Process vs Thread](#14-process-vs-thread)
15. [Multithreading Models](#15-multithreading-models)
16. [Inter-Process Communication](#16-inter-process-communication)
17. [CPU Scheduling](#17-cpu-scheduling)
18. [Scheduling Algorithms](#18-scheduling-algorithms)
19. [Multiprocessor and Real-Time
    Scheduling](#19-multiprocessor-and-real-time-scheduling)
20. [Process Synchronization](#20-process-synchronization)
21. [Critical Section Problem](#21-critical-section-problem)
22. [Mutex, Semaphore, Spinlock and
    Monitor](#22-mutex-semaphore-spinlock-and-monitor)
23. [Classic Synchronization
    Problems](#23-classic-synchronization-problems)
24. [Deadlock](#24-deadlock)
25. [Deadlock Prevention](#25-deadlock-prevention)
26. [Deadlock Avoidance and Banker's
    Algorithm](#26-deadlock-avoidance-and-bankers-algorithm)
27. [Deadlock Detection and
    Recovery](#27-deadlock-detection-and-recovery)
28. [Deadlock vs Starvation vs
    Livelock](#28-deadlock-vs-starvation-vs-livelock)
29. [Memory Management Basics](#29-memory-management-basics)
30. [Contiguous Memory Allocation](#30-contiguous-memory-allocation)
31. [Paging](#31-paging)
32. [TLB and Effective Access Time](#32-tlb-and-effective-access-time)
33. [Multi-Level and Advanced Page
    Tables](#33-multi-level-and-advanced-page-tables)
34. [Segmentation](#34-segmentation)
35. [Virtual Memory](#35-virtual-memory)
36. [Page Fault](#36-page-fault)
37. [Page Replacement Algorithms](#37-page-replacement-algorithms)
38. [Thrashing and Working Set](#38-thrashing-and-working-set)
39. [Memory Allocation, Heap, Stack and
    Fragmentation](#39-memory-allocation-heap-stack-and-fragmentation)
40. [File System Basics](#40-file-system-basics)
41. [Directories and Links](#41-directories-and-links)
42. [File Allocation Methods](#42-file-allocation-methods)
43. [Inodes and UNIX File Systems](#43-inodes-and-unix-file-systems)
44. [File Permissions](#44-file-permissions)
45. [Journaling, VFS and Mounting](#45-journaling-vfs-and-mounting)
46. [Disk Structure and Disk Access
    Time](#46-disk-structure-and-disk-access-time)
47. [Disk Scheduling](#47-disk-scheduling)
48. [RAID](#48-raid)
49. [I/O Management](#49-io-management)
50. [Interrupts, Exceptions and
    Traps](#50-interrupts-exceptions-and-traps)
51. [Context Switching](#51-context-switching)
52. [DMA, Buffering, Caching and
    Spooling](#52-dma-buffering-caching-and-spooling)
53. [Virtualization and Containers](#53-virtualization-and-containers)
54. [Security and Protection](#54-security-and-protection)
55. [Linux Commands for OA](#55-linux-commands-for-oa)
56. [Important Formulas](#56-important-formulas)
57. [OA Numerical Problem-Solving
    Patterns](#57-oa-numerical-problem-solving-patterns)
58. [Most Asked Interview Questions](#58-most-asked-interview-questions)
59. [Last-Minute Revision Sheet](#59-last-minute-revision-sheet)

------------------------------------------------------------------------

# 1. What is an Operating System?

An **Operating System (OS)** is system software that sits between
**applications/users and hardware**.

Think of the OS as a **manager**:

``` text
User / Applications
        ↓
   Operating System
        ↓
CPU | RAM | Disk | Keyboard | Network | Printer
```

Without an OS, every application would have to know how to directly
control hardware.

## Main jobs of an OS

  -----------------------------------------------------------------------
  Job                     Simple meaning          Example
  ----------------------- ----------------------- -----------------------
  Process management      Manage running programs Run Chrome + VS Code
                                                  together

  CPU scheduling          Decide who gets CPU     Give each process a
                                                  time slice

  Memory management       Manage RAM              Give memory to
                                                  processes

  File management         Manage                  `open()`, `read()`,
                          files/directories       `write()`

  Device management       Control hardware        Disk driver, keyboard
                          devices                 driver

  Security                Protect resources       File permissions

  IPC                     Let processes           Pipe, shared memory
                          communicate             

  Error handling          Detect/recover from     Page fault
                          errors                  

  Networking              Provide communication   Sockets

  User interface          Let users interact      Shell/GUI
  -----------------------------------------------------------------------

### Easy analogy

Imagine a college:

-   **CPU** = teachers
-   **RAM** = classrooms
-   **Disk** = storage room
-   **Processes** = students/classes
-   **OS** = administration
-   **Scheduler** = timetable
-   **File system** = filing department
-   **Permissions** = who is allowed to enter which room

------------------------------------------------------------------------

# 2. OS Services and System Components

An OS provides services to both **users** and **programs**.

## Important OS services

### 1. Program execution

The OS loads a program into memory, starts it, and handles its
termination.

Example:

``` bash
./program
```

The OS creates a process and gives it resources.

### 2. I/O operations

Programs should not directly control hardware.

Example:

``` c
read(fd, buffer, 100);
```

The OS handles the actual device operation.

### 3. File-system manipulation

Programs need to:

-   create files
-   delete files
-   open files
-   read/write files
-   create directories
-   change permissions

### 4. Communication

Processes may need to exchange information.

Examples:

-   pipes
-   shared memory
-   sockets
-   message queues

### 5. Error detection

The OS detects problems such as:

-   invalid memory access
-   disk errors
-   illegal instructions
-   page faults

### 6. Resource allocation

The OS decides who gets:

-   CPU
-   memory
-   disk
-   I/O devices

### 7. Accounting

The OS can track:

-   CPU time
-   memory usage
-   I/O usage
-   process information

### 8. Protection and security

The OS prevents one process/user from improperly accessing another's
resources.

------------------------------------------------------------------------

# 3. User Mode vs Kernel Mode

Modern CPUs usually provide at least two privilege levels:

-   **User mode**
-   **Kernel mode**

## User mode

Applications normally run here.

They have restricted access.

For example, an application cannot simply execute arbitrary hardware
instructions.

## Kernel mode

The OS kernel runs here.

It has privileged access to:

-   CPU control
-   memory management
-   hardware
-   device drivers
-   process management

### Why do we need two modes?

Suppose a buggy application could directly modify another process's
memory.

That would be disastrous.

So:

``` text
Application
   ↓
User mode
   ↓ system call / interrupt
Kernel mode
   ↓
Hardware
```

### Mode bit

A simplified model:

-   `1` → user mode
-   `0` → kernel mode

> The exact hardware representation varies by architecture; the
> important OA concept is that the CPU distinguishes privileged kernel
> execution from restricted user execution.

------------------------------------------------------------------------

# 4. Types of Operating Systems

## 4.1 Batch OS

Jobs are collected and executed with little/no user interaction.

**Example:** old mainframe batch processing.

Good for:

-   payroll
-   billing
-   large offline jobs

------------------------------------------------------------------------

## 4.2 Multiprogramming OS

Several programs are kept in memory.

When one process waits for I/O, the CPU can run another.

``` text
P1 → CPU → waits for disk
             ↓
            CPU
             ↓
P2 runs
```

**Main goal:** increase CPU utilization.

------------------------------------------------------------------------

## 4.3 Multitasking / Time-Sharing OS

CPU rapidly switches between processes.

Example:

``` text
P1 → P2 → P3 → P1 → P2 → ...
```

Users feel that programs are running simultaneously.

**Main goal:** responsiveness.

------------------------------------------------------------------------

## 4.4 Multiprocessing OS

The computer has multiple CPUs/cores.

``` text
Core 1 → P1
Core 2 → P2
Core 3 → P3
```

This provides true parallel execution.

------------------------------------------------------------------------

## 4.5 Real-Time OS

A real-time OS must respond within timing constraints.

### Hard real-time

Missing the deadline can be catastrophic.

Examples:

-   aircraft control
-   some medical control systems
-   airbag control

### Soft real-time

Missing a deadline is undesirable but not catastrophic.

Examples:

-   multimedia
-   video streaming

------------------------------------------------------------------------

## 4.6 Distributed OS

Multiple computers cooperate and may appear as one system.

------------------------------------------------------------------------

## 4.7 Network OS

Designed to provide services over a network.

Examples:

-   file sharing
-   printer sharing
-   user management

------------------------------------------------------------------------

## 4.8 Embedded OS

Designed for dedicated devices.

Examples:

-   routers
-   smart appliances
-   industrial controllers

------------------------------------------------------------------------

## 4.9 Mobile OS

Designed for mobile devices with:

-   touch input
-   battery constraints
-   sensors
-   wireless communication

Examples:

-   Android
-   iOS

------------------------------------------------------------------------

# 5. OS Structures and Kernel Types

The **kernel** is the core part of the OS.

It manages:

-   CPU
-   memory
-   processes
-   devices
-   system calls
-   protection

## 5.1 Monolithic Kernel

Most OS services run inside kernel space.

``` text
Kernel
├── Process management
├── Memory management
├── File system
├── Device drivers
└── Networking
```

### Advantages

-   Fast
-   Direct communication between components

### Disadvantages

-   Large kernel
-   Bug in a kernel component can affect the whole system

**Example:** Linux uses a monolithic kernel design with modular
components.

------------------------------------------------------------------------

## 5.2 Microkernel

Only essential functionality stays in kernel space.

``` text
Kernel
├── IPC
├── Scheduling
└── Basic memory management

User space
├── File server
├── Drivers
└── Other services
```

### Advantage

Better isolation and modularity.

### Disadvantage

More communication/message-passing overhead.

Examples:

-   MINIX
-   QNX

------------------------------------------------------------------------

## 5.3 Hybrid Kernel

Combines ideas from monolithic and microkernel designs.

Examples:

-   Windows NT family
-   Apple's XNU

------------------------------------------------------------------------

## 5.4 Layered OS

OS is divided into layers.

Higher layers use lower layers.

``` text
Layer 5 → User programs
Layer 4 → File system
Layer 3 → I/O
Layer 2 → Memory
Layer 1 → CPU
Layer 0 → Hardware
```

Advantage: easier organization/debugging.

------------------------------------------------------------------------

## 5.5 Modular Kernel

Kernel can load/unload modules when required.

Linux supports loadable kernel modules.

Example:

``` text
Kernel
   +
Wi-Fi driver module
   +
USB driver module
```

------------------------------------------------------------------------

## 5.6 Exokernel

Very small kernel that securely exposes hardware resources to
applications.

Main idea: give applications more control.

Mostly a research concept.

------------------------------------------------------------------------

# 6. Boot Process

When a computer starts:

``` text
Power ON
   ↓
Firmware BIOS/UEFI
   ↓
POST
   ↓
Find boot device
   ↓
Bootloader
   ↓
Kernel
   ↓
Kernel initializes hardware
   ↓
PID 1 / init / systemd
   ↓
Services
   ↓
Login / GUI
```

## Step-by-step

### 1. Power ON

CPU starts executing firmware code.

### 2. BIOS/UEFI

Firmware initializes hardware and identifies boot devices.

### 3. POST

Power-On Self-Test checks basic hardware.

### 4. Bootloader

Examples:

-   GRUB
-   Windows Boot Manager

The bootloader loads the kernel.

### 5. Kernel

Kernel initializes:

-   memory management
-   CPU scheduling
-   drivers
-   file systems
-   networking

### 6. Init process

On modern Linux systems, `systemd` commonly runs as PID 1.

It starts required services.

------------------------------------------------------------------------

# 7. System Calls

A **system call** is the controlled interface through which a user
program asks the kernel to perform a privileged operation.

Example:

``` c
read(fd, buffer, 100);
```

A program cannot directly access the disk. It requests the kernel.

## System call flow

``` text
User program
     ↓
Library/API
     ↓
System call
     ↓
Trap / syscall instruction
     ↓
CPU switches to kernel mode
     ↓
Kernel performs operation
     ↓
Return to user mode
```

## Main categories

### Process control

``` text
fork()
exec()
wait()
exit()
kill()
```

### File management

``` text
open()
read()
write()
close()
lseek()
unlink()
```

### Information

``` text
getpid()
time()
sleep()
```

### Communication

``` text
pipe()
mmap()
socket()
send()
recv()
```

### Protection

``` text
chmod()
chown()
umask()
```

------------------------------------------------------------------------

## API vs System Call

These are **not exactly the same thing**.

An API is a programming interface.

A system call is the actual kernel entry mechanism.

Example:

``` text
printf()
   ↓
C library
   ↓
write() system call
   ↓
kernel
```

One library/API function may use zero, one, or multiple system calls.

------------------------------------------------------------------------

# 8. Process Concept

A **program** is passive.

A **process** is a program currently executing.

``` text
Program on disk
     ↓
OS loads it
     ↓
Process in memory
```

### Example

`chrome.exe` on disk = program.

Running Chrome = process(es).

------------------------------------------------------------------------

## Process memory layout

A simplified process:

``` text
High Address
+------------------+
| Stack            |
| ↓ grows down     |
+------------------+
|                  |
| Free space       |
|                  |
+------------------+
| Heap             |
| ↑ grows up       |
+------------------+
| Data / BSS       |
+------------------+
| Text / Code      |
+------------------+
Low Address
```

### Text

Contains executable instructions.

### Data

Contains initialized global/static variables.

### BSS

Contains uninitialized or zero-initialized global/static variables.

### Heap

Dynamic memory.

Example:

``` cpp
int* p = new int(10);
```

### Stack

Stores function-call information and local variables.

Example:

``` cpp
void f() {
    int x = 10;
}
```

`x` is typically stored on the stack.

------------------------------------------------------------------------

# 9. Process Control Block

The OS maintains a **PCB (Process Control Block)** for each process.

Think of the PCB as the process's **ID card + current state**.

It can contain:

-   PID
-   Parent PID
-   Process state
-   Program counter
-   CPU registers
-   Scheduling information
-   Memory-management information
-   Accounting information
-   I/O/open-file information

### Why is PCB important?

During a context switch:

``` text
P1 running
   ↓
save P1 CPU state → PCB1
   ↓
load P2 CPU state ← PCB2
   ↓
P2 running
```

------------------------------------------------------------------------

# 10. Process States and Schedulers

## Basic 5-state model

``` text
             admitted
 NEW ----------------→ READY
                         |
                         | dispatch
                         ↓
                      RUNNING
                     /   |   \
                    /    |    \
                 I/O    exit   preempt
                  ↓      ↓       ↓
               WAITING TERMINATED READY
                  |
                  | I/O complete
                  ↓
                READY
```

## States

  State             Meaning
  ----------------- --------------------------
  New               Process is being created
  Ready             Waiting for CPU
  Running           Currently executing
  Waiting/Blocked   Waiting for I/O/event
  Terminated        Finished

### Important difference

**Ready ≠ Running**

-   Ready: wants CPU
-   Running: currently has CPU

------------------------------------------------------------------------

## 10.1 Long-Term Scheduler

Also called **job scheduler**.

Moves jobs into the ready pool.

Main job:

> Control the degree of multiprogramming.

Runs relatively rarely.

------------------------------------------------------------------------

## 10.2 Short-Term Scheduler

Also called **CPU scheduler**.

Chooses which ready process gets the CPU.

Runs very frequently.

------------------------------------------------------------------------

## 10.3 Medium-Term Scheduler

Handles swapping.

``` text
RAM
 ↓
Suspend / swap out
 ↓
Disk
 ↓
Swap in
 ↓
RAM
```

------------------------------------------------------------------------

## Dispatcher

The dispatcher gives CPU control to the selected process.

It performs tasks such as:

-   context switch
-   switching to user mode
-   jumping to the correct program location

**Dispatch latency** = time required to perform this handoff.

------------------------------------------------------------------------

# 11. Process Creation: fork, exec, wait

## 11.1 fork()

`fork()` creates a new process called the child.

After `fork()`, parent and child continue from the next instruction.

Return values:

``` text
fork() == 0       → child
fork() > 0        → parent receives child's PID
fork() == -1      → failure
```

### Example

``` c
printf("A\n");
fork();
printf("B\n");
```

Output conceptually:

``` text
A
B
B
```

`A` executes before the fork, so once.

`B` executes after the fork, so both processes execute it.

> Exact output order can vary when multiple processes write to the
> terminal.

------------------------------------------------------------------------

## Fork counting

If there are `n` unconditional forks:

``` text
Total processes = 2^n
```

Example:

``` c
fork();
fork();
fork();
```

Total:

``` text
2^3 = 8
```

Children created:

``` text
8 - 1 = 7
```

------------------------------------------------------------------------

## 11.2 exec()

`exec()` **replaces the current process image** with another program.

The PID normally remains the same.

Example:

``` c
fork();

if (child) {
    execlp("ls", "ls", "-l", NULL);
}
```

The child becomes the `ls` program.

### Very important

``` text
fork() → creates a process
exec() → replaces the current program
```

They are different operations.

------------------------------------------------------------------------

## 11.3 wait()

The parent can wait for a child:

``` c
wait(NULL);
```

This lets the parent collect the child's termination status and prevents
an unreaped terminated child from remaining as a zombie.

------------------------------------------------------------------------

## Common pattern

``` text
Parent
  |
 fork()
 /   \
P     C
     |
   exec()
     |
   program
     |
   exit()
     |
P waits
```

------------------------------------------------------------------------

# 12. Zombie, Orphan and Daemon Processes

## Zombie

Child has finished, but its parent has not yet collected its termination
status.

``` text
Child → terminated
Parent → has not wait()ed
```

The child is no longer executing.

A small process-table entry remains.

### Fix

Parent calls:

``` c
wait()
```

------------------------------------------------------------------------

## Orphan

Parent terminates while child is still running.

The child is adopted by a system process such as PID 1.

The orphan **continues running**.

------------------------------------------------------------------------

## Zombie vs Orphan

               Zombie                                 Orphan
  ------------ -------------------------------------- -----------------------------
  Child        Already terminated                     Still running
  Parent       Still exists but hasn't reaped child   Has terminated
  Main issue   Unreaped process entry                 Parent relationship changed

------------------------------------------------------------------------

## Daemon

A daemon is a background service process.

Examples on Linux:

-   SSH service
-   logging services
-   scheduled-job services

A daemon is not simply "a process with no parent"; it is a background
service designed to operate without normal interactive terminal use.

------------------------------------------------------------------------

# 13. Threads

A **thread** is the smallest unit of CPU execution inside a process.

A process can contain multiple threads:

``` text
Process
├── Thread 1
├── Thread 2
└── Thread 3
```

Threads of one process share:

-   code
-   data
-   heap
-   address space
-   many OS resources such as open files

Each thread has its own:

-   program counter
-   registers
-   stack
-   thread ID

### Example

A web browser might have different threads for:

``` text
UI
Network
Rendering
Background work
```

------------------------------------------------------------------------

# 14. Process vs Thread

  -----------------------------------------------------------------------
  Feature                 Process                 Thread
  ----------------------- ----------------------- -----------------------
  Address space           Separate                Shared within process

  Creation                More expensive          Cheaper

  Communication           IPC usually required    Shared memory directly
                                                  available

  Isolation               Strong                  Weak

  Stack                   Own                     Own

  Registers               Own                     Own

  Heap                    Own address space       Shared

  Failure                 Usually isolated        One bad thread can
                                                  affect whole process

  Context switch          Usually more expensive  Usually cheaper
  -----------------------------------------------------------------------

### Easy memory trick

> **Process = house. Thread = person inside the house.**

People in the same house share:

-   kitchen → heap/data
-   electricity → resources

But each person has:

-   own workspace → stack
-   own current activity → registers/PC

------------------------------------------------------------------------

# 15. Multithreading Models

## Many-to-One

``` text
T1 \
T2  → K1
T3 /
```

Many user threads mapped to one kernel thread.

Problem:

-   no true parallelism
-   one blocking system call can block all threads

------------------------------------------------------------------------

## One-to-One

``` text
T1 → K1
T2 → K2
T3 → K3
```

Provides true parallelism.

Used by common modern OS threading implementations.

Cost: more kernel resources.

------------------------------------------------------------------------

## Many-to-Many

``` text
T1 \
T2  \
T3   → K1, K2
T4  /
```

Many user threads mapped over multiple kernel threads.

------------------------------------------------------------------------

# 16. Inter-Process Communication

Processes normally have separate address spaces.

So they need **IPC** to communicate.

Two major models:

``` text
Shared Memory
Message Passing
```

------------------------------------------------------------------------

## 16.1 Shared Memory

OS creates/maps a memory region shared by processes.

``` text
Process A ──┐
            ├── Shared Memory
Process B ──┘
```

### Advantage

Very fast after setup.

### Problem

Processes must synchronize access.

Example:

``` text
A writes counter
B reads counter
```

Without synchronization, race conditions can occur.

------------------------------------------------------------------------

## 16.2 Message Passing

Processes exchange messages through OS-supported mechanisms.

``` text
P1 --send(message)--> OS --> receive()-- P2
```

Easier isolation.

Usually more overhead than direct shared memory.

------------------------------------------------------------------------

## IPC mechanisms

  Mechanism            Key idea
  -------------------- --------------------------------------------
  Pipe                 Byte stream between related processes
  FIFO                 Named pipe; unrelated processes can use it
  Message queue        Kernel-managed messages
  Shared memory        Shared region
  Socket               Local/network communication
  Signal               Asynchronous notification
  Memory-mapped file   File mapped into memory

------------------------------------------------------------------------

## Anonymous Pipe

``` bash
ls | grep ".txt"
```

The output of `ls` is connected to the input of `grep`.

------------------------------------------------------------------------

## Named Pipe

A FIFO has a filesystem name and can be used by unrelated processes.

------------------------------------------------------------------------

## Signals

Common signals:

  Signal    Meaning
  --------- --------------------------------------
  SIGINT    Interrupt, usually Ctrl+C
  SIGTERM   Request graceful termination
  SIGKILL   Force kill; cannot be caught/ignored
  SIGSTOP   Stop process
  SIGCONT   Continue process
  SIGSEGV   Invalid memory access

------------------------------------------------------------------------

# 17. CPU Scheduling

The CPU scheduler decides:

> Which ready process should run next?

------------------------------------------------------------------------

## Important terms

### Arrival Time (AT)

Time at which process enters the ready queue.

### Burst Time (BT)

CPU time required by the process.

### Completion Time (CT)

Time when process finishes.

### Turnaround Time

``` text
TAT = CT - AT
```

### Waiting Time

``` text
WT = TAT - BT
```

### Response Time

``` text
RT = First CPU start time - AT
```

### Throughput

``` text
Processes completed / unit time
```

### CPU Utilization

``` text
Busy CPU time / total time × 100
```

------------------------------------------------------------------------

## Preemptive vs Non-Preemptive

### Non-preemptive

Once a process gets CPU, it normally keeps it until:

-   it finishes, or
-   it blocks/waits

Examples:

-   FCFS
-   non-preemptive SJF
-   non-preemptive Priority

### Preemptive

OS can take CPU away.

Examples:

-   Round Robin
-   SRTF
-   preemptive Priority

------------------------------------------------------------------------

# 18. Scheduling Algorithms

## 18.1 FCFS

**First Come First Served**

Non-preemptive.

Process that arrives first runs first.

Example:

``` text
P1 P2 P3
```

If:

``` text
BT(P1)=10
BT(P2)=2
BT(P3)=1
```

Then P2 and P3 wait behind P1.

### Problem: Convoy Effect

A long process can make many short processes wait.

### Advantage

Very simple.

### Disadvantage

Poor average waiting time in many workloads.

------------------------------------------------------------------------

# 18.2 SJF

**Shortest Job First**

Choose the ready process with the smallest burst time.

### Key fact

SJF gives the minimum average waiting time when the relevant burst times
are known, under the standard assumptions.

### Problem

Future CPU burst is not known exactly.

### Another problem

Long processes can starve.

------------------------------------------------------------------------

# 18.3 SRTF

**Shortest Remaining Time First**

Preemptive version of SJF.

If a new process arrives with a shorter remaining time, preempt the
current process.

Example:

``` text
P1 remaining = 7
New P2 burst = 3
```

CPU switches to P2.

------------------------------------------------------------------------

# 18.4 Round Robin

Each process gets a fixed time quantum.

Example:

``` text
Quantum = 2

P1 → 2 units
P2 → 2 units
P3 → 2 units
P1 → ...
```

### Quantum too large

RR approaches FCFS.

### Quantum too small

Too many context switches.

### Main advantage

Good response time for interactive systems.

------------------------------------------------------------------------

# 18.5 Priority Scheduling

Highest-priority process runs.

Some systems use:

``` text
smaller number = higher priority
```

but always check the question's convention.

### Problem

Starvation.

### Solution

**Aging**

Increase the priority of processes that wait for a long time.

------------------------------------------------------------------------

# 18.6 HRRN

**Highest Response Ratio Next**

Non-preemptive.

``` text
Response Ratio = (Waiting Time + Burst Time) / Burst Time
```

or:

``` text
RR = 1 + W/B
```

Higher ratio gets CPU.

### Why it reduces starvation

As waiting time increases, response ratio increases.

------------------------------------------------------------------------

# 18.7 Multilevel Queue

Ready queue is divided into fixed queues.

Example:

``` text
System
Interactive
Batch
```

A process generally stays in its assigned queue.

Each queue can use a different algorithm.

------------------------------------------------------------------------

# 18.8 Multilevel Feedback Queue

Processes can move between queues.

Example:

``` text
High priority, small quantum
          ↓
uses entire quantum
          ↓
Lower priority
```

I/O-bound processes that frequently block can remain at higher priority.

MLFQ attempts to combine:

-   good response time
-   throughput
-   fairness

------------------------------------------------------------------------

# 18.9 Lottery Scheduling

Processes receive tickets.

A random ticket is selected.

More tickets → higher probability of CPU time.

------------------------------------------------------------------------

# 18.10 Linux CFS

The traditional Linux Completely Fair Scheduler conceptually tracks
**virtual runtime (`vruntime`)**.

The runnable task with the smallest appropriate virtual runtime is
favored.

For OA purposes:

> CFS tries to distribute CPU time fairly among runnable tasks.

------------------------------------------------------------------------

## Scheduling comparison

  Algorithm     Preemptive?              Starvation? Main point
  ----------- ------------- ------------------------ ------------------------
  FCFS                   No                       No Simple
  SJF                    No                 Possible Minimum avg WT
  SRTF                  Yes                 Possible Preemptive SJF
  RR                    Yes                       No Time sharing
  Priority           Either                 Possible Aging fixes starvation
  HRRN                   No                Much less Uses waiting time
  MLQ               Usually                 Possible Fixed queues
  MLFQ                  Yes   Possible without aging Processes move queues

------------------------------------------------------------------------

# 19. Multiprocessor and Real-Time Scheduling

## Multiprocessor scheduling

### Asymmetric multiprocessing

One CPU may perform most OS management work.

### Symmetric multiprocessing (SMP)

Each CPU/core can schedule work.

Modern multicore systems commonly use SMP-like designs.

------------------------------------------------------------------------

## Processor Affinity

Try to keep a process/thread on the same CPU.

Why?

Because CPU caches may already contain useful data.

This reduces cache-related overhead.

------------------------------------------------------------------------

## Load Balancing

Move work between CPUs to avoid one CPU being overloaded while another
is idle.

------------------------------------------------------------------------

## Real-Time Scheduling

### EDF --- Earliest Deadline First

Task with earliest deadline gets priority.

### Rate Monotonic

Shorter period → higher priority.

Used for periodic real-time tasks.

------------------------------------------------------------------------

# 20. Process Synchronization

When multiple threads/processes access shared data, the result can
depend on timing.

That is a **race condition**.

------------------------------------------------------------------------

## Example: `counter++`

Suppose:

``` text
counter = 5
```

Two threads execute:

``` cpp
counter++;
```

Conceptually:

``` text
LOAD
ADD 1
STORE
```

Possible execution:

``` text
T1: LOAD 5
T2: LOAD 5
T1: ADD → 6
T2: ADD → 6
T1: STORE 6
T2: STORE 6
```

Expected:

``` text
7
```

Actual:

``` text
6
```

This is a lost update.

------------------------------------------------------------------------

# 21. Critical Section Problem

A **critical section** is the part of code that accesses shared
data/resource.

``` text
Entry Section
      ↓
Critical Section
      ↓
Exit Section
      ↓
Remainder Section
```

A correct solution needs:

## 1. Mutual Exclusion

Only one process/thread can be inside the critical section at a time.

## 2. Progress

If no one is inside the critical section, processes that want to enter
should not be postponed indefinitely by processes that do not need it.

## 3. Bounded Waiting

A waiting process should not wait forever while others repeatedly enter.

### Memory trick

``` text
M P B
Mutual exclusion
Progress
Bounded waiting
```

------------------------------------------------------------------------

# 22. Mutex, Semaphore, Spinlock and Monitor

## Mutex

A mutex is an ownership-based lock.

``` text
lock()
critical section
unlock()
```

Usually only the thread that owns the mutex should unlock it.

Use it when protecting shared data.

------------------------------------------------------------------------

## Semaphore

A semaphore is an integer synchronization mechanism accessed through
atomic operations.

Two conceptual operations:

``` text
wait()   → acquire/decrement
signal() → release/increment
```

### Counting semaphore

Can represent multiple identical resources.

Example:

``` text
5 printers
Semaphore = 5
```

### Binary semaphore

Value is typically 0 or 1.

Can be used for mutual exclusion or signaling, although a mutex is
usually preferable when ownership semantics are required.

------------------------------------------------------------------------

## Mutex vs Semaphore

  Mutex                               Semaphore
  ----------------------------------- ----------------------------
  Ownership-based                     No ownership requirement
  Usually protects critical section   Can signal/count resources
  Usually binary                      Binary or counting
  Owner unlocks                       Another thread can signal

------------------------------------------------------------------------

## Spinlock

A thread repeatedly checks a lock instead of sleeping.

``` text
while(lock_is_busy)
    keep checking;
```

### Good when

Expected wait is extremely short and sleeping/waking would cost more.

### Bad when

Lock is held for a long time because CPU time is wasted spinning.

------------------------------------------------------------------------

## Monitor

A monitor combines:

-   shared data
-   operations on that data
-   automatic mutual exclusion
-   condition variables

Only one thread executes inside the monitor at a time.

Java's `synchronized` mechanism is closely related to monitor-style
synchronization.

------------------------------------------------------------------------

# 23. Classic Synchronization Problems

## 23.1 Producer-Consumer

A producer adds items to a bounded buffer.

A consumer removes them.

For buffer size `N`:

``` text
empty = N
full = 0
mutex = 1
```

Typical order:

``` text
Producer:
wait(empty)
wait(mutex)
add item
signal(mutex)
signal(full)

Consumer:
wait(full)
wait(mutex)
remove item
signal(mutex)
signal(empty)
```

### Why does `empty` matter?

Producer must not insert into a full buffer.

### Why does `full` matter?

Consumer must not remove from an empty buffer.

### Why mutex?

Only one thread modifies the buffer at a time.

------------------------------------------------------------------------

## 23.2 Readers-Writers

Multiple readers can read simultaneously.

A writer needs exclusive access.

``` text
Reader + Reader → allowed
Reader + Writer → not allowed
Writer + Writer → not allowed
```

### Readers-preference

Readers may repeatedly enter, causing writer starvation.

Other versions provide writer preference or fairness.

------------------------------------------------------------------------

## 23.3 Dining Philosophers

Five philosophers sit around a table.

Each needs two forks.

Naive solution:

``` text
pick left fork
pick right fork
eat
release both
```

If every philosopher picks the left fork first:

``` text
P1 holds left
P2 holds left
P3 holds left
P4 holds left
P5 holds left
```

Everyone waits for the right fork.

**Deadlock.**

### Solutions

-   Allow at most 4 philosophers to try simultaneously.
-   Require both forks to be acquired atomically.
-   Use different fork order for different philosophers.
-   Use a waiter/monitor.

------------------------------------------------------------------------

## 23.4 Sleeping Barber

One barber, limited waiting chairs.

Customers:

-   wake barber if sleeping
-   wait if a chair is free
-   leave if all chairs are occupied

Tests synchronization, blocking and resource counting.

------------------------------------------------------------------------

## 23.5 Barrier

A barrier forces threads to wait until all participating threads reach a
certain point.

Example:

``` text
T1 ───────→ barrier ┐
T2 ─────→ barrier   ├→ continue
T3 ─────────→ barrier┘
```

Useful in parallel algorithms.

------------------------------------------------------------------------

# 24. Deadlock

A deadlock occurs when processes are permanently waiting for resources
held by one another.

Example:

``` text
P1 holds Printer
P1 wants Scanner

P2 holds Scanner
P2 wants Printer
```

Neither can continue.

------------------------------------------------------------------------

## Four Coffman Conditions

**All four must be present for deadlock to occur.**

### 1. Mutual Exclusion

A resource cannot be shared simultaneously.

### 2. Hold and Wait

A process holds one resource while waiting for another.

### 3. No Preemption

Resource cannot be forcibly taken away.

### 4. Circular Wait

There is a cycle:

``` text
P1 waits for P2
P2 waits for P3
P3 waits for P1
```

### Memory trick

``` text
M H N C
Mutual exclusion
Hold and wait
No preemption
Circular wait
```

------------------------------------------------------------------------

# 25. Deadlock Prevention

Prevention means:

> Design the system so that at least one Coffman condition can never
> hold.

## Break Mutual Exclusion

Make a resource shareable where possible.

Not possible for inherently exclusive resources such as a physical
printer during a particular operation.

------------------------------------------------------------------------

## Break Hold and Wait

Require processes to request all needed resources at once.

Problem:

-   poor resource utilization
-   possible starvation

------------------------------------------------------------------------

## Break No Preemption

If a process cannot obtain a requested resource, it may have to release
resources it already holds.

Works only for resources that can safely be taken/recreated.

------------------------------------------------------------------------

## Break Circular Wait

Give every resource a global order.

Example:

``` text
R1 < R2 < R3
```

Every process must acquire resources in increasing order.

### Example

Correct:

``` text
lock(A)
lock(B)
```

and every thread follows the same order.

This prevents:

``` text
Thread 1: A → B
Thread 2: B → A
```

------------------------------------------------------------------------

# 26. Deadlock Avoidance and Banker's Algorithm

## Safe State

A state is **safe** if there exists at least one order in which every
process can obtain its remaining resources and finish.

That order is called a **safe sequence**.

## Unsafe State

No safe sequence exists.

Important:

> **Unsafe does not automatically mean deadlocked.**

It means the system has entered a state from which deadlock may become
possible.

------------------------------------------------------------------------

## Banker's Algorithm

Used for deadlock avoidance when maximum resource demands are known.

Data:

``` text
Available
Max
Allocation
Need
```

Formula:

``` text
Need = Max - Allocation
```

### Safety algorithm

1.  Set:

``` text
Work = Available
```

2.  Set all:

``` text
Finish[i] = false
```

3.  Find a process where:

``` text
Need[i] <= Work
```

4.  Pretend it finishes:

``` text
Work = Work + Allocation[i]
Finish[i] = true
```

5.  Repeat.

6.  If every process finishes:

``` text
Safe
```

Otherwise:

``` text
Unsafe
```

------------------------------------------------------------------------

## Example

Available:

``` text
(3, 3, 2)
```

Suppose:

  Process   Allocation   Max     Need
  --------- ------------ ------- -------
  P0        0 1 0        7 5 3   7 4 3
  P1        2 0 0        3 2 2   1 2 2
  P2        3 0 2        9 0 2   6 0 0
  P3        2 1 1        2 2 2   0 1 1
  P4        0 0 2        4 3 3   4 3 1

Start:

``` text
Work = (3,3,2)
```

P1 can finish because:

``` text
(1,2,2) <= (3,3,2)
```

After P1:

``` text
Work = (3,3,2) + (2,0,0)
     = (5,3,2)
```

P3:

``` text
(0,1,1) <= (5,3,2)
```

Work:

``` text
(7,4,3)
```

P4:

``` text
(4,3,1) <= (7,4,3)
```

Work:

``` text
(7,4,5)
```

P0:

``` text
(7,4,3) <= (7,4,5)
```

Work:

``` text
(7,5,5)
```

P2:

``` text
(6,0,0) <= (7,5,5)
```

Therefore safe sequence:

``` text
<P1, P3, P4, P0, P2>
```

------------------------------------------------------------------------

## Resource Request Algorithm

When process `Pi` requests resources:

### Step 1

Check:

``` text
Request <= Need
```

If not, invalid request.

### Step 2

Check:

``` text
Request <= Available
```

If not, process must wait.

### Step 3

Pretend to allocate.

### Step 4

Run the safety algorithm.

-   Safe → grant
-   Unsafe → roll back and make process wait

------------------------------------------------------------------------

# 27. Deadlock Detection and Recovery

## Detection: Single instance

Build a **wait-for graph**.

``` text
P1 → P2
P2 → P3
P3 → P1
```

Cycle → deadlock.

------------------------------------------------------------------------

## Detection: Multiple instances

Use a detection algorithm based on:

-   Available
-   Allocation
-   Current Request

------------------------------------------------------------------------

## Recovery

### 1. Process termination

Terminate:

-   all deadlocked processes, or
-   one at a time until deadlock disappears

### 2. Resource preemption

Take a resource from a selected victim.

Potential problems:

-   rollback cost
-   starvation

------------------------------------------------------------------------

## Deadlock-free resource formula

For a single resource type where:

-   `P` = number of processes
-   each process may need at most `R`
-   `N` = total resources

A sufficient condition for avoiding deadlock is:

``` text
N >= P(R - 1) + 1
```

Example:

``` text
P = 3
R = 4

N >= 3(4-1)+1
N >= 10
```

------------------------------------------------------------------------

# 28. Deadlock vs Starvation vs Livelock

  ----------------------------------------------------------------------------------
                    Deadlock               Starvation        Livelock
  ----------------- ---------------------- ----------------- -----------------------
  Main idea         Waiting cycle          One process waits Processes are active
                                           indefinitely      but make no progress

  Progress          None in cycle          Other processes   No useful progress
                                           progress          

  CPU usage         Often low              Victim may get    Often high
                                           little CPU        

  Typical fix       Prevention/avoidance   Aging/fairness    Backoff/randomization
  ----------------------------------------------------------------------------------

### Example

**Deadlock:**

``` text
A waits for B
B waits for A
```

**Starvation:**

``` text
High-priority jobs keep arriving.
Low-priority job never gets CPU.
```

**Livelock:**

Two threads repeatedly detect conflict and back off at exactly the same
time, then retry and conflict again.

------------------------------------------------------------------------

# 29. Memory Management Basics

The OS must:

-   allocate memory
-   free memory
-   protect processes
-   translate addresses
-   support virtual memory
-   share memory safely

------------------------------------------------------------------------

## Logical vs Physical Address

### Logical / Virtual address

Generated by the CPU for a process.

### Physical address

Actual location in RAM.

The **MMU (Memory Management Unit)** performs address translation.

``` text
CPU
 ↓
Virtual address
 ↓
MMU
 ↓
Physical address
 ↓
RAM
```

------------------------------------------------------------------------

## Address Binding

### Compile time

Address known at compile time.

### Load time

Address determined when program is loaded.

### Execution time

Address translation happens while program runs.

Modern virtual-memory systems use runtime address translation.

------------------------------------------------------------------------

## Base and Limit Registers

Suppose:

``` text
Base = 14000
Limit = 3000
Logical address = 346
```

Physical address:

``` text
14000 + 346 = 14346
```

If:

``` text
Logical address = 3500
```

then it exceeds the limit → protection fault/trap.

------------------------------------------------------------------------

# 30. Contiguous Memory Allocation

Each process gets one contiguous memory region.

## Fixed Partitioning

Memory divided into fixed partitions.

### Problem

**Internal fragmentation**

Example:

``` text
Partition = 100 KB
Process = 70 KB
Waste = 30 KB
```

The unused 30 KB is inside an allocated partition.

------------------------------------------------------------------------

## Variable Partitioning

Partitions are created based on process size.

### Problem

**External fragmentation**

Free memory exists, but it is split into many small holes.

Example:

``` text
Free: 10 KB + 15 KB + 20 KB
```

Total free = 45 KB.

But a 40 KB contiguous request may fail if no single hole is large
enough.

------------------------------------------------------------------------

# 31. Paging

Paging divides:

``` text
Logical memory → Pages
Physical memory → Frames
```

Pages and frames have the same fixed size.

Example:

``` text
Page size = 4 KB
```

A process of 10 KB needs:

``` text
3 pages
```

because:

``` text
10 KB / 4 KB = 2.5 → 3
```

------------------------------------------------------------------------

## Page Table

Maps:

``` text
Page number → Frame number
```

Example:

``` text
Page 0 → Frame 5
Page 1 → Frame 2
Page 2 → Frame 9
```

------------------------------------------------------------------------

## Address translation

Logical address:

``` text
[ Page Number | Offset ]
```

Page table converts:

``` text
Page Number → Frame Number
```

Physical address:

``` text
[ Frame Number | Offset ]
```

The offset does not change.

------------------------------------------------------------------------

## Why paging removes external fragmentation

A process does not need consecutive frames.

Example:

``` text
Page 0 → Frame 2
Page 1 → Frame 8
Page 2 → Frame 4
```

The frames can be scattered.

### But paging can have internal fragmentation

The final page may not be completely filled.

------------------------------------------------------------------------

## Paging calculations

If:

``` text
Page size = 2^12 bytes
```

then:

``` text
Offset bits = 12
```

If logical address is 32 bits:

``` text
Page number bits = 32 - 12 = 20
```

Number of pages:

``` text
2^20
```

------------------------------------------------------------------------

## Page Table Size

If:

``` text
Logical address = 32 bits
Page size = 4 KB = 2^12
PTE = 4 bytes
```

Number of pages:

``` text
2^(32-12) = 2^20
```

Page table size:

``` text
2^20 × 4 bytes
= 4 MB
```

------------------------------------------------------------------------

# 32. TLB and Effective Access Time

## TLB

**Translation Lookaside Buffer** is a small, fast cache containing
recent page-table mappings.

Without TLB:

``` text
1. Access page table
2. Access actual data
```

So a memory reference can require roughly two memory accesses.

With TLB hit:

``` text
1. Find translation in TLB
2. Access data
```

------------------------------------------------------------------------

## EAT formula

Let:

-   `h` = TLB hit ratio
-   `t` = TLB lookup time
-   `m` = memory access time

Then:

``` text
EAT = h(t + m) + (1-h)(t + 2m)
```

Example:

``` text
h = 0.9
t = 10 ns
m = 100 ns
```

``` text
EAT
= 0.9(10+100) + 0.1(10+200)
= 99 + 21
= 120 ns
```

------------------------------------------------------------------------

# 33. Multi-Level and Advanced Page Tables

A huge page table can waste memory.

Multi-level paging breaks the page table into smaller tables.

Example:

``` text
Virtual address
[ p1 | p2 | offset ]
   ↓
Level 1 table
   ↓
Level 2 table
   ↓
Frame
```

Only required lower-level tables need to exist.

------------------------------------------------------------------------

## Inverted Page Table

Instead of one entry per virtual page, use roughly one entry per
physical frame.

Advantage:

-   much smaller for large virtual address spaces

Disadvantage:

-   lookup can be more complicated

------------------------------------------------------------------------

## Hashed Page Table

Virtual page number is hashed to find a page-table entry/bucket.

Useful for large address spaces.

------------------------------------------------------------------------

## Copy-on-Write

After `fork()`, parent and child can initially share physical pages.

``` text
Parent ─┐
        ├── same physical page
Child ──┘
```

If one writes:

``` text
Parent writes
    ↓
Page copied
    ↓
Parent gets private copy
```

This saves memory and makes `fork()` more efficient.

------------------------------------------------------------------------

# 34. Segmentation

Segmentation divides memory according to logical program units.

Example:

``` text
Segment 0 → Code
Segment 1 → Data
Segment 2 → Stack
Segment 3 → Heap
```

Logical address:

``` text
<segment number, offset>
```

Segment table stores:

``` text
Base
Limit
```

Physical address:

``` text
Base + Offset
```

provided:

``` text
Offset < Limit
```

------------------------------------------------------------------------

## Paging vs Segmentation

  Paging                            Segmentation
  --------------------------------- ------------------------------------
  Fixed-size pages                  Variable-size segments
  Physical memory-oriented          Programmer/logical view
  Internal fragmentation            External fragmentation
  Page table                        Segment table
  Usually invisible to programmer   Historically visible logical units

------------------------------------------------------------------------

# 35. Virtual Memory

Virtual memory allows a process to use an address space larger than the
amount of physical RAM currently available.

It uses secondary storage as backing storage.

``` text
Virtual address space
        ↓
RAM + disk-backed pages
```

### Important

Virtual memory is not simply "extra RAM".

Disk is much slower than RAM.

Its purpose is mainly to provide:

-   large address spaces
-   process isolation
-   efficient memory use
-   demand loading

------------------------------------------------------------------------

## Demand Paging

A page is loaded into RAM only when needed.

This is called **lazy loading**.

------------------------------------------------------------------------

# 36. Page Fault

A page fault happens when a process accesses a virtual page that is not
currently present in RAM.

### Steps

``` text
CPU references page
      ↓
Page not present
      ↓
Page-fault trap
      ↓
OS checks whether access is valid
      ↓
Find free frame / choose victim
      ↓
Read page from disk
      ↓
Update page table
      ↓
Restart instruction
```

### Important

A page fault is not necessarily an error in the program.

It is often a normal part of virtual memory.

An invalid memory reference can cause a protection error/segmentation
fault instead.

------------------------------------------------------------------------

## Page Fault EAT

If:

-   `p` = page-fault probability
-   `m` = normal memory access time
-   `F` = page-fault service time

Simplified:

``` text
EAT = (1-p)m + pF
```

Because page-fault service can take milliseconds while RAM access takes
nanoseconds, even a tiny fault probability can have a large performance
impact.

------------------------------------------------------------------------

# 37. Page Replacement Algorithms

When there is no free frame and a new page must be loaded, the OS must
choose a victim.

------------------------------------------------------------------------

## 37.1 FIFO

Replace the page that entered memory first.

Example:

``` text
Oldest page → victim
```

### Advantage

Simple.

### Disadvantage

Can suffer from **Belady's anomaly**.

------------------------------------------------------------------------

## 37.2 Optimal

Replace the page whose next use is farthest in the future.

### Advantage

Minimum possible page faults.

### Problem

Future references are unknown.

So it is mainly a benchmark.

------------------------------------------------------------------------

## 37.3 LRU

Replace the page that has not been used for the longest time.

Based on past behavior.

### Advantage

Usually performs well.

### Problem

Exact implementation can be expensive.

------------------------------------------------------------------------

## 37.4 Second Chance / Clock

FIFO plus a reference bit.

If reference bit is:

``` text
1 → clear it and give another chance
0 → replace
```

Useful approximation to LRU.

------------------------------------------------------------------------

## Belady's Anomaly

With FIFO, increasing the number of frames can sometimes increase page
faults.

That sounds strange, but it can happen.

Stack algorithms such as LRU and Optimal do not exhibit Belady's
anomaly.

------------------------------------------------------------------------

# 38. Thrashing and Working Set

**Thrashing** occurs when the system spends most of its time handling
page faults instead of executing useful work.

``` text
CPU work ↓
Page faults ↑
Disk I/O ↑
Performance ↓
```

### Causes

-   too many processes
-   too few frames
-   poor locality

------------------------------------------------------------------------

## Working Set

The working set is the collection of pages a process is actively using
during a recent window of execution.

If the OS keeps the working set in RAM, page faults are reduced.

------------------------------------------------------------------------

## Page Fault Frequency

If fault rate becomes too high:

``` text
Give process more frames
```

If fault rate becomes very low:

``` text
Some frames may be reclaimed
```

If total memory demand is too high, reduce the degree of
multiprogramming.

------------------------------------------------------------------------

# 39. Memory Allocation, Heap, Stack and Fragmentation

## Stack

Used for:

-   function calls
-   local variables
-   return addresses

Fast and automatically managed.

Problem:

``` text
Very deep recursion → stack overflow
```

------------------------------------------------------------------------

## Heap

Used for dynamic memory.

Example:

``` cpp
int* p = new int[100];
delete[] p;
```

In languages with manual memory management, forgetting to release memory
can cause leaks.

------------------------------------------------------------------------

## Memory Leak

Memory remains allocated but is no longer useful/reachable.

------------------------------------------------------------------------

## Dangling Pointer

Pointer refers to memory that has already been freed.

``` text
free(p)
↓
p still points to old address
```

Using it is unsafe.

------------------------------------------------------------------------

## Internal Fragmentation

Waste **inside** allocated blocks.

Example:

``` text
Block = 8 KB
Needed = 6 KB
Waste = 2 KB
```

------------------------------------------------------------------------

## External Fragmentation

Free memory exists but is scattered.

------------------------------------------------------------------------

## Compaction

Move allocated blocks together to create a large contiguous free area.

Works for systems where relocation is supported, but costs time.

------------------------------------------------------------------------

## First Fit

Take the first hole that is large enough.

Fast and commonly effective.

------------------------------------------------------------------------

## Best Fit

Take the smallest hole that is large enough.

May create many tiny holes.

------------------------------------------------------------------------

## Worst Fit

Take the largest hole.

Usually not as effective in general-purpose allocation.

------------------------------------------------------------------------

## Next Fit

Like first fit, but search begins from where the previous search ended.

------------------------------------------------------------------------

## Buddy System

Memory blocks are powers of two.

Example:

``` text
1024 KB
 ├── 512 KB
 │    ├── 256 KB
 │    └── 256 KB
 └── 512 KB
```

When memory is freed, buddy blocks can be merged.

------------------------------------------------------------------------

## Slab Allocation

Kernel allocators can maintain caches of frequently used kernel objects.

Benefits:

-   fast allocation
-   reduced fragmentation
-   object reuse

------------------------------------------------------------------------

# 40. File System Basics

A file is a named collection of data stored on secondary storage.

## File attributes

May include:

-   name
-   identifier
-   type
-   size
-   location
-   owner
-   permissions
-   timestamps

------------------------------------------------------------------------

## File operations

Common operations:

``` text
create
open
read
write
seek
close
delete
truncate
```

------------------------------------------------------------------------

## File descriptor

In UNIX-like systems, an open file is represented to a process by a
**file descriptor**.

Standard descriptors:

``` text
0 → stdin
1 → stdout
2 → stderr
```

Example:

``` c
write(1, "Hello", 5);
```

writes to standard output.

------------------------------------------------------------------------

# 41. Directories and Links

## Directory structures

### Single-level

All files in one directory.

Problem: name conflicts.

### Two-level

Separate directory per user.

### Tree

Hierarchical directories.

Most common.

### Acyclic graph

Allows sharing while preventing directory cycles.

### General graph

Cycles can exist.

Requires mechanisms to manage cycles/reclamation.

------------------------------------------------------------------------

## Hard Link

A hard link is another directory entry referring to the same inode/file
object.

``` text
name1 ──┐
        ├── inode → data
name2 ──┘
```

Deleting one name does not necessarily delete the file data.

The data is reclaimed when the inode's link/reference count reaches zero
and no process still has it open.

Usually:

-   cannot cross file-system boundaries
-   cannot normally hard-link directories

------------------------------------------------------------------------

## Symbolic Link

A symbolic link stores a path to another file.

``` text
shortcut → target path
```

If target disappears:

``` text
shortcut → broken link
```

------------------------------------------------------------------------

# 42. File Allocation Methods

## 42.1 Contiguous Allocation

File occupies consecutive disk blocks.

``` text
100 101 102 103 104
```

### Advantages

-   very fast sequential access
-   fast direct access

### Disadvantages

-   external fragmentation
-   difficult file growth

------------------------------------------------------------------------

## 42.2 Linked Allocation

Each block points to the next.

``` text
Block 7 → Block 19 → Block 4 → Block 25
```

### Advantage

No external fragmentation.

### Disadvantage

Poor random access.

------------------------------------------------------------------------

## 42.3 FAT

File Allocation Table stores the linked-list information in a separate
table.

This makes traversal easier than reading a pointer from each disk block.

------------------------------------------------------------------------

## 42.4 Indexed Allocation

An index block stores pointers to file blocks.

``` text
Index block
├── block 7
├── block 19
├── block 4
└── block 25
```

Supports direct access better than pure linked allocation.

------------------------------------------------------------------------

# 43. Inodes and UNIX File Systems

An **inode** stores metadata about a file and pointers to its data
blocks.

Typically includes:

-   file type
-   permissions
-   owner
-   size
-   timestamps
-   link count
-   data block pointers

### Important

The filename is generally stored in the **directory entry**, not inside
the inode itself.

------------------------------------------------------------------------

## Direct and indirect pointers

A simplified inode may have:

``` text
Direct pointers
Single indirect
Double indirect
Triple indirect
```

If:

``` text
Block size = 4 KB
Pointer = 4 bytes
```

Then one indirect block holds:

``` text
4096 / 4 = 1024 pointers
```

A single-indirect block can therefore address:

``` text
1024 × 4 KB = 4 MB
```

A double-indirect block can address:

``` text
1024 × 1024 × 4 KB ≈ 4 GB
```

------------------------------------------------------------------------

# 44. File Permissions

UNIX permission example:

``` text
-rwxr-xr--
```

Break it into:

``` text
owner  group  others
rwx    r-x    r--
```

Values:

``` text
r = 4
w = 2
x = 1
```

Example:

``` bash
chmod 754 file
```

Means:

``` text
7 = rwx
5 = r-x
4 = r--
```

------------------------------------------------------------------------

## Directory execute permission

For a directory, `x` generally means you can **traverse/search** it.

This is a common OA/interview trap.

------------------------------------------------------------------------

## Special permissions

### setuid

Executable can run with the owner's effective privileges.

### setgid

Can provide group-related privilege inheritance/behavior.

### sticky bit

Commonly used on directories such as `/tmp` so users cannot freely
delete/rename files owned by others.

------------------------------------------------------------------------

# 45. Journaling, VFS and Mounting

## Journaling

A journal records filesystem changes so the filesystem can recover more
reliably after a crash.

Basic idea:

``` text
Record intended metadata changes
        ↓
Perform changes
        ↓
Crash?
        ↓
Replay/recover journal
```

Journaling improves consistency/recovery; it does not mean "data can
never be lost."

------------------------------------------------------------------------

## VFS

**Virtual File System** provides a common interface to different
filesystem implementations.

``` text
Applications
     ↓
VFS
 ┌───┼────┐
ext4 NTFS NFS ...
```

Applications can use common system calls without knowing the internal
filesystem implementation.

------------------------------------------------------------------------

## Mounting

Mounting attaches a filesystem to an existing directory tree.

Example:

``` text
mount /dev/sdb1 /mnt/data
```

After mounting, the filesystem becomes accessible through `/mnt/data`.

------------------------------------------------------------------------

# 46. Disk Structure and Disk Access Time

Traditional HDD structure:

``` text
Platter
  ↓
Tracks
  ↓
Sectors

Same-numbered tracks across platters → Cylinder
```

------------------------------------------------------------------------

## Disk access time

Approximately:

``` text
Access Time
= Seek Time
+ Rotational Latency
+ Transfer Time
```

### Seek time

Time to move the disk head to the desired track.

### Rotational latency

Time waiting for the desired sector to rotate under the head.

Average rotational latency:

``` text
1/2 × rotation time
```

For 7200 RPM:

``` text
1 rotation = 60/7200 sec
           ≈ 8.33 ms

Average latency ≈ 4.17 ms
```

------------------------------------------------------------------------

# 47. Disk Scheduling

Suppose:

``` text
Queue = 98, 183, 37, 122, 14, 124, 65, 67
Head = 53
```

------------------------------------------------------------------------

## 47.1 FCFS

Serve in given order.

Simple but can cause large head movement.

------------------------------------------------------------------------

## 47.2 SSTF

**Shortest Seek Time First**

Choose the closest request.

From 53:

``` text
65 is closest
```

Then continue choosing the closest request.

### Problem

Far-away requests can starve.

------------------------------------------------------------------------

## 47.3 SCAN

Also called the **elevator algorithm**.

Head moves in one direction, servicing requests, then reverses.

``` text
→ → → → →
← ← ← ← ←
```

It can travel to the end of the disk before reversing, depending on the
exact convention in the question.

------------------------------------------------------------------------

## 47.4 LOOK

Like SCAN, but the head reverses at the last pending request rather than
going all the way to the physical disk end.

------------------------------------------------------------------------

## 47.5 C-SCAN

Moves in one direction only.

When it reaches one end, it jumps back to the other end and continues in
the same direction.

Provides more uniform waiting time.

------------------------------------------------------------------------

## 47.6 C-LOOK

Like C-SCAN, but jumps between the last request in one direction and the
first request in the other direction rather than traveling to physical
ends.

------------------------------------------------------------------------

## SCAN vs LOOK

``` text
SCAN → goes to disk end
LOOK → stops at last request
```

------------------------------------------------------------------------

# 48. RAID

RAID combines multiple disks for performance, redundancy, or both.

## RAID 0 --- Striping

``` text
Disk 1: A C E
Disk 2: B D F
```

Fast.

No redundancy.

If one disk fails, data can be lost.

Minimum: 2 disks.

------------------------------------------------------------------------

## RAID 1 --- Mirroring

``` text
Disk 1: A B C
Disk 2: A B C
```

Excellent redundancy.

Usable capacity is roughly 50%.

Minimum: 2 disks.

------------------------------------------------------------------------

## RAID 5 --- Distributed Parity

Uses striping + distributed parity.

Can tolerate one disk failure.

Minimum: 3 disks.

Usable capacity is roughly:

``` text
(N - 1) disks
```

------------------------------------------------------------------------

## RAID 6

Double parity.

Can tolerate two disk failures.

Minimum: 4 disks.

Usable capacity is roughly:

``` text
(N - 2) disks
```

------------------------------------------------------------------------

## RAID 10

Mirroring + striping.

Requires at least 4 disks.

Provides good performance and redundancy but costs more storage.

------------------------------------------------------------------------

# 49. I/O Management

I/O devices are much slower than the CPU.

The OS provides mechanisms to manage this mismatch.

## Programmed I/O / Polling

CPU repeatedly checks the device.

``` text
CPU:
Is it ready?
Is it ready?
Is it ready?
```

Simple but wastes CPU time.

------------------------------------------------------------------------

## Interrupt-driven I/O

Device interrupts CPU when it needs attention or completes an operation.

Better CPU utilization.

------------------------------------------------------------------------

## DMA

**Direct Memory Access**

A DMA controller transfers a block of data between device and memory
with limited CPU involvement.

``` text
Device → DMA controller → RAM
                  ↓
             interrupt CPU
```

The CPU can do other work during much of the transfer.

------------------------------------------------------------------------

## Device Controller vs Driver

### Controller

Hardware that operates the device.

### Driver

OS software that communicates with the controller.

------------------------------------------------------------------------

# 50. Interrupts, Exceptions and Traps

## Interrupt

Usually an asynchronous event from outside the current instruction
stream.

Example:

``` text
Keyboard input
Disk I/O completed
Timer interrupt
```

------------------------------------------------------------------------

## Exception

Synchronous event caused by the current instruction.

Examples:

``` text
Divide by zero
Invalid instruction
Page fault
```

------------------------------------------------------------------------

## Trap

A deliberate/synchronous exception used for things such as:

-   system calls
-   debugging

Terminology varies somewhat by architecture, but for OA purposes:

> Interrupt = usually external/asynchronous.\
> Exception/trap = synchronous with instruction execution.

------------------------------------------------------------------------

## Interrupt handling

Simplified:

``` text
Device raises interrupt
        ↓
CPU saves relevant state
        ↓
Find ISR
        ↓
ISR executes
        ↓
Restore state
        ↓
Continue
```

------------------------------------------------------------------------

## Interrupt Vector Table

Contains mappings from interrupt/exception numbers to handler addresses.

------------------------------------------------------------------------

## Maskable vs Non-Maskable

### Maskable

Can be temporarily disabled/masked.

### Non-maskable

Designed for events that should not be ignored.

------------------------------------------------------------------------

# 51. Context Switching

A **context switch** changes the CPU from one process/thread to another.

Example:

``` text
P1 running
   ↓
timer interrupt
   ↓
save P1 registers/PC/SP
   ↓
scheduler selects P2
   ↓
load P2 state
   ↓
P2 running
```

Saved state can include:

-   program counter
-   registers
-   stack pointer
-   scheduling state
-   address-space information

------------------------------------------------------------------------

## Why is context switching expensive?

Because it does not directly perform useful application work.

Costs can include:

-   saving/loading registers
-   scheduler work
-   address-space changes
-   TLB effects
-   cache disruption

### Mode switch vs Context switch

**Mode switch:**

``` text
User → Kernel → User
```

Same process can continue.

**Context switch:**

``` text
Process/Thread A → Process/Thread B
```

The CPU execution context changes.

A system call can cause a mode switch without causing a context switch.

------------------------------------------------------------------------

# 52. DMA, Buffering, Caching and Spooling

## Buffering

Temporary storage used to handle speed differences.

Example:

``` text
Producer → Buffer → Consumer
```

------------------------------------------------------------------------

## Caching

Store frequently used data in faster storage.

Example:

``` text
CPU cache
```

The goal is to avoid repeatedly accessing slower storage.

------------------------------------------------------------------------

## Spooling

Jobs are queued for a device that handles one job at a time.

Classic example:

``` text
Programs → Print queue → Printer
```

------------------------------------------------------------------------

## Blocking I/O

Process waits until operation can proceed/complete.

------------------------------------------------------------------------

## Non-blocking I/O

Call returns without waiting for the operation to complete.

------------------------------------------------------------------------

## Asynchronous I/O

Program continues and later receives completion notification/result.

------------------------------------------------------------------------

# 53. Virtualization and Containers

## Virtual Machine

A hypervisor allows multiple guest operating systems to run on one
physical machine.

``` text
Hardware
   ↓
Hypervisor
 ├── VM 1 → Linux
 ├── VM 2 → Windows
 └── VM 3 → Linux
```

------------------------------------------------------------------------

## Type 1 Hypervisor

Runs directly on hardware.

``` text
Hardware
 ↓
Hypervisor
 ↓
VMs
```

Examples:

-   VMware ESXi
-   Xen

------------------------------------------------------------------------

## Type 2 Hypervisor

Runs on a host operating system.

``` text
Hardware
 ↓
Host OS
 ↓
Hypervisor
 ↓
VM
```

Example:

-   VirtualBox

------------------------------------------------------------------------

## VM vs Container

### VM

Virtualizes a complete machine/OS environment.

Each VM usually has its own guest kernel.

### Container

Shares the host kernel but isolates processes/resources.

``` text
Host kernel
├── Container A
├── Container B
└── Container C
```

Linux containers commonly rely on mechanisms such as:

-   namespaces
-   cgroups

### Easy difference

> **VM → separate guest OS/kernel.**\
> **Container → shared host kernel.**

------------------------------------------------------------------------

# 54. Security and Protection

## Authentication

Answers:

> Who are you?

Examples:

-   password
-   fingerprint
-   MFA

------------------------------------------------------------------------

## Authorization

Answers:

> What are you allowed to do?

Example:

``` text
User A → read file
User B → read + write file
```

------------------------------------------------------------------------

## Principle of Least Privilege

Give a user/process only the permissions it actually needs.

------------------------------------------------------------------------

## Access Control List

ACL describes permissions associated with an object.

Example:

``` text
file.txt
Alice → read/write
Bob   → read
```

------------------------------------------------------------------------

## Capability

Capability-based systems associate subjects with unforgeable
permissions/tokens for resources.

Easy distinction:

``` text
ACL → object asks "who can access me?"
Capability → subject has "what can I access?"
```

------------------------------------------------------------------------

## Common security threats

### Virus

Malicious code that attaches/spreads through files or programs.

### Worm

Self-propagating malware, often across networks.

### Trojan

Malicious software disguised as legitimate software.

### Ransomware

Encrypts/locks data and demands payment.

### Privilege escalation

Attacker gains higher privileges than intended.

### DoS

Makes a service unavailable by exhausting resources or otherwise
overwhelming it.

------------------------------------------------------------------------

## Buffer Overflow

Writing beyond a buffer's bounds.

Possible consequences:

-   memory corruption
-   crashes
-   control-flow hijacking

Mitigations include:

-   stack canaries
-   ASLR
-   DEP/NX
-   bounds checking
-   memory-safe languages

------------------------------------------------------------------------

## Encryption vs Hashing

### Encryption

Reversible using a key.

``` text
Plaintext → encryption → ciphertext
ciphertext → decryption → plaintext
```

### Hashing

One-way transformation intended for integrity/password storage.

Passwords should normally be stored using a password hashing/KDF scheme
with a unique salt, not as plaintext.

------------------------------------------------------------------------

# 55. Linux Commands for OA

  Task                     Command
  ------------------------ --------------------
  List processes           `ps aux`
  Interactive processes    `top`, `htop`
  Process tree             `pstree`
  Kill process             `kill PID`
  Force kill               `kill -9 PID`
  Background process       `command &`
  Jobs                     `jobs`
  Foreground               `fg`
  Background stopped job   `bg`
  Memory                   `free -h`
  VM statistics            `vmstat`
  Disk filesystem usage    `df -h`
  Directory size           `du -sh dir`
  Open files               `lsof`
  File permissions         `ls -l`
  Change permissions       `chmod`
  Change owner             `chown`
  Hard link                `ln file link`
  Symbolic link            `ln -s file link`
  System calls             `strace ./program`
  Mount                    `mount`
  Unmount                  `umount`
  Inode number             `ls -i`
  File metadata            `stat file`
  Scheduling/RT priority   `chrt`
  Change priority          `nice`, `renice`

------------------------------------------------------------------------

# 56. Important Formulas

  Concept                                 Formula
  --------------------------------------- ---------------------------------------
  Turnaround Time                         `CT - AT`
  Waiting Time                            `TAT - BT`
  Response Time                           `First CPU Start - AT`
  CPU Utilization                         `Busy Time / Total Time × 100`
  Throughput                              `Completed Processes / Time`
  HRRN                                    `(W + B) / B`
  Banker's Need                           `Max - Allocation`
  Deadlock-free single-resource bound     `N >= P(R-1)+1`
  Number of pages                         `Process Size / Page Size` rounded up
  Number of frames                        `Physical Memory / Page Size`
  Page table size                         `Number of Pages × PTE Size`
  Offset bits                             `log2(Page Size)`
  TLB EAT                                 `h(t+m) + (1-h)(t+2m)`
  Page-fault EAT                          `(1-p)m + pF`
  Disk access                             `Seek + Rotation + Transfer`
  Average rotational latency              `1/2 × rotation time`
  Rotation time                           `60 / RPM` seconds
  Processes after n unconditional forks   `2^n`

------------------------------------------------------------------------

# 57. OA Numerical Problem-Solving Patterns

## 57.1 CPU Scheduling

Always make this table:

  Process     AT   BT   CT   TAT   WT   RT
  --------- ---- ---- ---- ----- ---- ----

Then:

``` text
TAT = CT - AT
WT  = TAT - BT
RT  = First Start - AT
```

### Recommended approach

1.  Draw Gantt chart.
2.  Find CT.
3.  Calculate TAT.
4.  Calculate WT.
5.  Calculate RT.
6.  Take averages only after individual values are correct.

------------------------------------------------------------------------

## 57.2 Round Robin

Keep a ready queue.

At every event:

-   process arrival
-   quantum expiration
-   process completion

Be careful about the question's convention when an arrival and quantum
expiration occur at the same time.

------------------------------------------------------------------------

## 57.3 Page Replacement

Create a frame table:

``` text
Reference:  7  0  1  2  0  3
Frame 1:
Frame 2:
Frame 3:

Fault?:
```

Mark:

``` text
H = hit
F = fault
```

Count only faults.

------------------------------------------------------------------------

## 57.4 Banker's Algorithm

Always calculate:

``` text
Need = Max - Allocation
```

Then:

``` text
Work = Available
```

Find a process with:

``` text
Need <= Work
```

After it finishes:

``` text
Work += Allocation
```

Repeat.

------------------------------------------------------------------------

## 57.5 Paging Address Questions

Suppose:

``` text
Page size = 4 KB
```

Convert:

``` text
4 KB = 2^12
```

Therefore:

``` text
Offset = 12 bits
```

Then:

``` text
Page number = address bits - offset bits
```

------------------------------------------------------------------------

## 57.6 Disk Scheduling

Write the exact head path.

For example:

``` text
53 → 65 → 67 → ...
```

Then add absolute movements:

``` text
|65-53| + |67-65| + ...
```

Be careful about whether SCAN/C-SCAN goes to the physical disk end.
Follow the convention stated by the question.

------------------------------------------------------------------------

## 57.7 Fork Questions

Do not blindly use `2^n`.

First check:

-   `if`
-   `&&`
-   `||`
-   loops
-   conditions
-   whether a process reaches each `fork()`

Remember:

``` text
fork() return in child = 0
fork() return in parent = child's PID
```

This is especially important with:

``` c
if (fork() && fork())
```

because `&&` uses short-circuit evaluation.

------------------------------------------------------------------------

# 58. Most Asked Interview Questions

## Q1. Program vs Process?

**Program:** passive executable file.

**Process:** active execution of a program with resources and a PCB.

------------------------------------------------------------------------

## Q2. Process vs Thread?

A process has its own address space; threads within a process share the
address space but have separate stacks/registers/PCs.

------------------------------------------------------------------------

## Q3. Why are threads faster than processes?

Threads share the same address space and many resources, so creation and
switching can be cheaper and communication is easier.

------------------------------------------------------------------------

## Q4. What is a race condition?

A result depends on the timing/order of concurrent accesses to shared
data.

------------------------------------------------------------------------

## Q5. How do you prevent a race condition?

Use appropriate synchronization:

-   mutex
-   semaphore
-   monitor
-   atomic operations
-   other concurrency primitives

------------------------------------------------------------------------

## Q6. Mutex vs Semaphore?

Mutex is mainly an ownership-based lock.

Semaphore is a signaling/counting mechanism.

------------------------------------------------------------------------

## Q7. What is deadlock?

Processes wait forever because each is waiting for resources held by
others in a cycle.

------------------------------------------------------------------------

## Q8. Four deadlock conditions?

``` text
Mutual exclusion
Hold and wait
No preemption
Circular wait
```

------------------------------------------------------------------------

## Q9. Deadlock prevention vs avoidance?

**Prevention:** structurally prevent at least one necessary condition.

**Avoidance:** allow requests but grant them only if the resulting state
remains safe.

------------------------------------------------------------------------

## Q10. Safe vs Unsafe vs Deadlocked?

``` text
Deadlocked ⊂ Unsafe
```

A safe state has a safe sequence.

An unsafe state has no guaranteed safe sequence.

An unsafe state is not necessarily already deadlocked.

------------------------------------------------------------------------

## Q11. What is starvation?

A process waits indefinitely because other processes keep getting the
resource/CPU.

Typical solution:

``` text
Aging
```

------------------------------------------------------------------------

## Q12. What is thrashing?

System spends excessive time handling page faults instead of executing
useful work.

Solutions:

-   reduce multiprogramming
-   increase available memory
-   working-set model
-   page-fault frequency control

------------------------------------------------------------------------

## Q13. Paging vs Segmentation?

Paging uses fixed-size pages/frames.

Segmentation uses variable-sized logical segments.

Paging mainly causes internal fragmentation; segmentation can cause
external fragmentation.

------------------------------------------------------------------------

## Q14. What is a TLB?

A fast cache of recent virtual-page-to-physical-frame translations.

------------------------------------------------------------------------

## Q15. What is a page fault?

A process references a page that is not currently in RAM, causing the OS
to load it or reject the access if it is invalid.

------------------------------------------------------------------------

## Q16. Why is Optimal page replacement not practical?

It requires knowing future page references.

It is mainly used as a theoretical benchmark.

------------------------------------------------------------------------

## Q17. What is Belady's anomaly?

With FIFO page replacement, increasing the number of frames can
sometimes increase the number of page faults.

------------------------------------------------------------------------

## Q18. What is a context switch?

Saving the execution state of one process/thread and loading another.

------------------------------------------------------------------------

## Q19. Mode switch vs context switch?

Mode switch:

``` text
User ↔ Kernel
```

Context switch:

``` text
Thread/Process A ↔ Thread/Process B
```

A mode switch does not necessarily mean a context switch.

------------------------------------------------------------------------

## Q20. What happens when you type `ls`?

A simplified flow:

``` text
Shell
 ↓
fork()
 ↓
Child
 ↓
exec("ls")
 ↓
ls performs system calls
 ↓
Output
 ↓
Child exits
 ↓
Parent shell waits
 ↓
Prompt returns
```

------------------------------------------------------------------------

## Q21. fork vs exec?

``` text
fork() → creates another process
exec() → replaces current process image
```

Common Unix pattern:

``` text
fork() + exec()
```

------------------------------------------------------------------------

## Q22. Zombie vs Orphan?

**Zombie:** child has terminated but parent has not reaped it.

**Orphan:** parent terminates while child is still running.

------------------------------------------------------------------------

## Q23. What is virtual memory?

An abstraction that gives processes a large virtual address space while
the OS maps required pages to physical memory and backing storage.

------------------------------------------------------------------------

## Q24. Why is a page fault expensive?

A disk/SSD-backed page may need to be fetched, which is many orders of
magnitude slower than normal RAM access.

------------------------------------------------------------------------

## Q25. What is Copy-on-Write?

After `fork()`, parent and child initially share pages. A page is copied
only when one process writes to it.

------------------------------------------------------------------------

## Q26. Hard link vs symbolic link?

Hard link refers to the same inode/file object.

Symbolic link refers to a pathname.

A symbolic link can become dangling when its target disappears.

------------------------------------------------------------------------

## Q27. What is DMA?

DMA lets a controller transfer data between a device and memory with
limited CPU involvement.

------------------------------------------------------------------------

## Q28. Interrupt vs Polling?

Polling repeatedly asks whether the device needs service.

Interrupts let the device notify the CPU.

------------------------------------------------------------------------

## Q29. What is a system call?

A controlled interface through which a user program requests a kernel
service.

------------------------------------------------------------------------

## Q30. Why can't user programs directly access hardware?

Because unrestricted hardware access would allow one application to
interfere with the OS and other processes. Privilege levels provide
protection.

------------------------------------------------------------------------

## Q31. What is convoy effect?

In FCFS, a long process can make many short processes wait behind it.

------------------------------------------------------------------------

## Q32. Why does SJF minimize average waiting time?

Choosing the shortest available job first minimizes the waiting
contribution caused by jobs placed before other jobs, under the standard
scheduling assumptions.

------------------------------------------------------------------------

## Q33. Why can SJF cause starvation?

Long jobs can repeatedly be postponed by newly arriving short jobs.

------------------------------------------------------------------------

## Q34. How does aging solve starvation?

Increase a waiting process's priority over time so it eventually gets
CPU access.

------------------------------------------------------------------------

## Q35. What is priority inversion?

A high-priority thread waits for a lock held by a low-priority thread,
while medium-priority work prevents the low-priority thread from
running.

A common solution is **priority inheritance**.

------------------------------------------------------------------------

# 59. Last-Minute Revision Sheet

## Must Memorize

### Process states

``` text
New → Ready → Running → Terminated
             ↓
           Waiting
             ↓
           Ready
```

### Schedulers

``` text
Long-term  → controls admission/degree of multiprogramming
Short-term → selects CPU process
Medium-term → swapping
```

### Process vs Thread

``` text
Process → separate address space
Thread  → shared address space
```

### Fork

``` text
n unconditional forks → 2^n processes
```

### System call

``` text
User → system call/trap → kernel → return
```

### Scheduling formulas

``` text
TAT = CT - AT
WT  = TAT - BT
RT  = First Start - AT
```

### Deadlock

``` text
M H N C
```

``` text
Mutual exclusion
Hold and wait
No preemption
Circular wait
```

### Synchronization

``` text
MPB
```

``` text
Mutual exclusion
Progress
Bounded waiting
```

### Paging

``` text
Logical address = Page + Offset
Page → Frame
```

### TLB

``` text
Fast cache of page-table translations
```

### Virtual memory

``` text
Page not in RAM → page fault → load page
```

### Page replacement

``` text
FIFO → oldest
LRU → least recently used
OPT → farthest future use
```

### Thrashing

``` text
Too many page faults
→ little useful CPU work
```

### File permissions

``` text
r = 4
w = 2
x = 1
```

``` text
754 = rwx r-x r--
```

### Disk

``` text
Access = Seek + Rotation + Transfer
```

### RAID

``` text
RAID 0 → striping, no redundancy
RAID 1 → mirroring
RAID 5 → distributed parity, 1-disk fault tolerance
RAID 6 → double parity, 2-disk fault tolerance
RAID 10 → mirror + stripe
```

### I/O

``` text
Polling       → CPU repeatedly checks
Interrupt I/O → device notifies CPU
DMA           → controller transfers blocks with limited CPU involvement
```

### Security

``` text
Authentication → Who are you?
Authorization  → What can you do?
```

------------------------------------------------------------------------

# High-Priority OA Checklist

If you have limited time, study these first:

1.  Process vs Thread
2.  Process states
3.  `fork()`, `exec()`, `wait()`
4.  Zombie vs Orphan
5.  CPU scheduling numericals
6.  FCFS, SJF, SRTF, RR, Priority
7.  Race condition
8.  Critical-section requirements
9.  Mutex vs Semaphore
10. Producer-Consumer
11. Readers-Writers
12. Dining Philosophers
13. Four deadlock conditions
14. Deadlock prevention vs avoidance
15. Banker's Algorithm
16. Paging and address translation
17. TLB/EAT numericals
18. Page fault
19. FIFO/LRU/Optimal page replacement
20. Belady's anomaly
21. Thrashing
22. Internal vs External fragmentation
23. Hard vs Soft links
24. Inodes
25. File permissions
26. Disk scheduling
27. RAID
28. Interrupt vs polling
29. DMA
30. Context switch vs mode switch
31. System calls
32. Virtualization vs containers
33. Authentication vs authorization

------------------------------------------------------------------------

# Quick Memory Tricks

### Process states

> **New → Ready → Running → Waiting → Ready → Running → Terminated**

### Deadlock

> **MHNC**

### Critical section requirements

> **MPB**

### CPU scheduling

> **FCFS = simple**\
> **SJF = shortest**\
> **SRTF = shortest remaining**\
> **RR = time quantum**\
> **Priority = priority**\
> **HRRN = waiting time improves priority**

### Paging

> **Page is virtual. Frame is physical.**

### Links

> **Hard link = same inode**\
> **Soft link = path**

### I/O

> **Polling asks. Interrupt tells. DMA transfers.**

### Authentication

> **AuthN = identity**\
> **AuthZ = permission**

------------------------------------------------------------------------

# Final OA Strategy

Do not try to memorize every paragraph.

For each topic, know these four things:

``` text
1. What is it?
2. Why do we need it?
3. How does it work?
4. What is the common OA trap?
```

For numerical questions:

``` text
Understand formula
      ↓
Write given values
      ↓
Draw the structure
      ↓
Calculate step by step
      ↓
Check the answer
```

For interview questions:

``` text
Definition
   ↓
Simple explanation
   ↓
Example
   ↓
Advantage / problem
   ↓
Common solution
```

The highest-value OS concepts for most software/data/analyst OAs are:

``` text
Processes + Threads
        ↓
Scheduling
        ↓
Synchronization
        ↓
Deadlocks
        ↓
Memory + Paging
        ↓
Virtual Memory
        ↓
File Systems
        ↓
I/O + Interrupts
        ↓
Security + Virtualization
```

> **Rule for revision:** If you can explain a concept in 2--3 simple
> sentences, solve one example, and distinguish it from its closest
> competing concept, you know it well enough for most OA questions.

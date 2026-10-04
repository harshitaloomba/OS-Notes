# Operating Systems: Complete Notes for OA / Interview Preparation

## Table of Contents
1. [What Does an OS Do](#1-what-does-an-os-do)
2. [Types of Operating Systems](#2-types-of-operating-systems)
3. [OS Structure, Kernel and Boot Process](#3-os-structure-kernel-and-boot-process)
4. [Process vs Thread](#4-process-vs-thread)
5. [Process States and Life Cycle](#5-process-states-and-life-cycle)
6. [Process Creation: fork, exec, wait, Zombie, Orphan](#6-process-creation-fork-exec-wait-zombie-orphan)
7. [Inter-Process Communication (IPC)](#7-inter-process-communication-ipc)
8. [CPU Scheduling Algorithms](#8-cpu-scheduling-algorithms)
9. [Process Synchronization](#9-process-synchronization)
10. [Classic Synchronization Problems](#10-classic-synchronization-problems)
11. [Deadlock](#11-deadlock)
12. [Memory Management](#12-memory-management)
13. [Virtual Memory and Page Replacement](#13-virtual-memory-and-page-replacement)
14. [File Systems](#14-file-systems)
15. [Disk Scheduling, Disk Structure and RAID](#15-disk-scheduling-disk-structure-and-raid)
16. [Deadlock vs Starvation vs Livelock](#16-deadlock-vs-starvation-vs-livelock)
17. [System Calls](#17-system-calls)
18. [Interrupts and Context Switching](#18-interrupts-and-context-switching)
19. [I/O Management](#19-io-management)
20. [Security and Protection](#20-security-and-protection)
21. [Linux Commands for OS Questions](#21-linux-commands-for-os-questions)
22. [Formula Cheat Sheet](#22-formula-cheat-sheet)
23. [Most Asked Interview / OA Questions](#23-most-asked-interview--oa-questions)

---

## 1. What Does an OS Do

An **Operating System** is system software that acts as an **interface between the user/applications and the hardware**. It manages hardware resources and provides services to programs.

### Main Functions
| Function | What it does | Example |
|---|---|---|
| Process management | Create, schedule, terminate processes; handle IPC | Running Chrome and VS Code together |
| Memory management | Allocate/deallocate RAM, virtual memory | Swapping idle pages to disk |
| File management | Organize, store, retrieve, protect files | `ext4`, NTFS |
| Device management | Drivers, buffering, spooling | Printer spooling |
| Security & protection | Authentication, access control | File permissions `rwx` |
| Resource allocation | Fair sharing of CPU, memory, I/O | Time slicing |
| User interface | CLI or GUI | Bash, Windows Explorer |
| Error detection | Detects hardware/software errors | Segmentation fault handling |

### Goals of an OS
- **Convenience**: easy to use.
- **Efficiency**: use hardware well.
- **Ability to evolve**: allows new features.

### User Mode vs Kernel Mode
| | User Mode | Kernel Mode |
|---|---|---|
| Mode bit | 1 | 0 |
| Privilege | Restricted | Full hardware access |
| Who runs here | Applications | OS kernel, drivers |
| Crash impact | Only that process | Whole system (BSOD / kernel panic) |

A **system call** or **interrupt** switches from user mode to kernel mode (a **trap**).

---

## 2. Types of Operating Systems

| Type | Description | Example |
|---|---|---|
| **Batch OS** | Similar jobs grouped and run without user interaction | Old IBM mainframes |
| **Multiprogramming** | Several jobs in memory; CPU switches when one does I/O, so CPU utilization is higher | Early UNIX |
| **Multitasking / Time-sharing** | CPU switches rapidly between tasks, giving interactivity | Linux, Windows |
| **Multiprocessing** | More than one CPU in the system | Server OS |
| **Real-Time OS (RTOS)** | Guarantees response within a deadline. **Hard RT**: missing a deadline is failure (pacemaker, airbag). **Soft RT**: occasional miss is tolerable (video streaming) | VxWorks, FreeRTOS |
| **Distributed OS** | Many machines appear as one system | LOCUS, Amoeba |
| **Network OS** | Runs on a server, serves clients | Novell NetWare |
| **Embedded OS** | Dedicated, tiny devices | Microwave, router firmware |
| **Mobile OS** | Touch, battery optimised | Android, iOS |

**Multiprogramming vs Multitasking vs Multiprocessing vs Multithreading**
- *Multiprogramming*: many programs in memory, CPU switches only on I/O wait (goal: CPU utilization).
- *Multitasking*: multiprogramming plus time slicing (goal: responsiveness).
- *Multiprocessing*: multiple CPUs (goal: throughput).
- *Multithreading*: multiple threads in one process.

---

## 3. OS Structure, Kernel and Boot Process

### Kernel Types
| Kernel | Idea | Pros | Cons | Example |
|---|---|---|---|---|
| **Monolithic** | All OS services in one big kernel space | Fast (fewer context switches) | A bug in a driver can crash the OS; hard to maintain | Linux, classic UNIX |
| **Microkernel** | Only the minimum (IPC, scheduling, basic memory) in the kernel; everything else (drivers, FS) in user space | Stable, secure, modular | Slower (message passing) | Minix, QNX, L4 |
| **Hybrid** | Mix of both | Balance | Complex | Windows NT, macOS (XNU) |
| **Exokernel** | Gives apps direct hardware access | Max performance | Complex | Research |
| **Layered** | OS in layers, each using only the lower one | Easy debugging | Hard to define layers, overhead | THE OS |

### Boot Process (PC)
1. **Power on** → CPU jumps to the BIOS/UEFI firmware address.
2. **POST** (Power-On Self Test) checks hardware.
3. Firmware finds the boot device and loads the **bootloader** (GRUB, Windows Boot Manager) from the MBR/EFI partition.
4. Bootloader loads the **kernel** into RAM.
5. Kernel initialises hardware/drivers and starts the first process (`init` / `systemd`, PID 1).
6. Init starts services and the login/GUI.

### Other Concepts
- **Bootstrap program**: the small program that loads the OS.
- **Virtual machine / Hypervisor**: Type 1 (bare metal: ESXi, Xen), Type 2 (hosted: VirtualBox).
- **Shell**: command interpreter (user-level program). **Kernel**: the core.

---

## 4. Process vs Thread

### Process
A **process** is a program in execution. Memory layout:

```
High address
+--------------+
|    Stack     |  local vars, function calls, return addresses
|      |       |
|      v       |
|              |
|      ^       |
|      |       |
|    Heap      |  malloc / new (dynamic memory)
+--------------+
|  Data (BSS)  |  global & static variables
+--------------+
|  Text (code) |  instructions (read-only)
+--------------+
Low address
```

### Process Control Block (PCB)
Kernel data structure that stores everything about a process:
- Process ID (PID), parent PID
- Process state
- Program counter (PC)
- CPU registers
- CPU scheduling info (priority, queue pointers)
- Memory management info (page tables, base/limit)
- Accounting info (CPU time used)
- I/O status (open files, devices)

### Thread
A **thread** is the smallest unit of CPU execution (a lightweight process). Threads of the same process **share** code, data, heap, open files, but each has its **own** stack, registers and PC.

### Process vs Thread
| Feature | Process | Thread |
|---|---|---|
| Definition | Program in execution | Unit of execution inside a process |
| Memory | Separate address space | Shared address space (except stack) |
| Creation/termination time | Slow | Fast |
| Context switch | Slow (switch memory maps, flush TLB) | Fast |
| Communication | IPC needed (pipes, sockets, shared memory) | Direct through shared memory |
| Isolation | Strong, one crash doesn't kill others | Weak, one thread crash can kill the process |
| Overhead | High | Low |
| Example | Chrome tabs (each is a process) | Threads inside a tab, e.g., rendering + network |

### What each thread has vs shares
| Own | Shared |
|---|---|
| Stack | Code (text) |
| Registers | Data (globals) |
| Program counter | Heap |
| Thread ID | Open files, signals |

### User-Level vs Kernel-Level Threads
| | User-Level Thread (ULT) | Kernel-Level Thread (KLT) |
|---|---|---|
| Managed by | User library | OS kernel |
| Switching | Fast, no kernel call | Slower |
| Blocking | One blocking call blocks the entire process | Others continue |
| Multi-core use | No | Yes |

### Multithreading Models
- **Many-to-One**: many user threads → 1 kernel thread (no parallelism, one block blocks all).
- **One-to-One**: each user thread → 1 kernel thread (Linux, Windows). True parallelism, but costly.
- **Many-to-Many**: M user threads → N kernel threads (N ≤ M). Flexible.

### Example (C, POSIX threads)
```c
#include <pthread.h>
#include <stdio.h>

void* worker(void* arg) {
    printf("Hello from thread %d\n", *(int*)arg);
    return NULL;
}

int main() {
    pthread_t t1, t2;
    int a = 1, b = 2;
    pthread_create(&t1, NULL, worker, &a);
    pthread_create(&t2, NULL, worker, &b);
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    return 0;
}
```

### Benefits of Multithreading
Responsiveness, resource sharing, economy, scalability on multicore.

---

## 5. Process States and Life Cycle

### 5-State Model
```
          admit                dispatch              exit
 NEW ───────────► READY ◄──────────────► RUNNING ──────────► TERMINATED
                   ▲        timeout/preempt   │
                   │                          │ I/O or event wait
                   │   I/O or event done      ▼
                   └───────────────────── WAITING (BLOCKED)
```

| State | Meaning |
|---|---|
| **New** | Process is being created (PCB being built) |
| **Ready** | In memory, waiting for the CPU |
| **Running** | Instructions executing on the CPU |
| **Waiting / Blocked** | Waiting for I/O or an event |
| **Terminated** | Finished; PCB cleanup pending |

### Transitions
- **New → Ready**: admitted by the long-term scheduler.
- **Ready → Running**: dispatched by the short-term scheduler.
- **Running → Ready**: time quantum expired or a higher-priority process arrived (preemption).
- **Running → Waiting**: requested I/O (e.g., `scanf`, disk read).
- **Waiting → Ready**: I/O completed.
- **Running → Terminated**: `exit()` or killed.

### 7-State Model (adds swapping)
Adds **Suspended Ready** and **Suspended Blocked** (process swapped out to disk by the medium-term scheduler when memory is scarce).

### Schedulers
| Scheduler | Also called | Job | Frequency |
|---|---|---|---|
| **Long-term** | Job scheduler | Picks jobs from the pool to load into memory; controls **degree of multiprogramming** | Rare |
| **Short-term** | CPU scheduler | Picks a ready process for the CPU | Very frequent (ms) |
| **Medium-term** | Swapper | Swaps processes in and out | Occasional |

**Dispatcher**: gives CPU control to the selected process (context switch, switch to user mode, jump to the right PC). **Dispatch latency** is the time this takes.

**I/O-bound vs CPU-bound**: I/O-bound processes do short CPU bursts; CPU-bound ones do long bursts. A good long-term scheduler mixes both.

---

## 6. Process Creation: fork, exec, wait, Zombie, Orphan

### fork()
Creates a **child** that is a copy of the parent. Returns:
- `0` in the child
- child's PID in the parent
- `-1` on failure

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("A\n");
    fork();
    printf("B\n");
    return 0;
}
```
Output: `A B B` (A printed once, B printed twice).

**Counting processes**: with `n` `fork()` calls (no conditions), total processes = **2^n**, so the number of children = 2^n − 1.

```c
fork(); fork(); fork();   // 2^3 = 8 processes (7 children)
```

**Typical OA question**
```c
int main() {
    if (fork() && fork())
        fork();
    printf("X ");
}
```
- Parent P: first `fork()` returns non-zero (true) → calls second `fork()`, also true in P → calls the third `fork()`.
- First child C1: first fork returns 0 → short-circuit, skips the rest.
- Second child C2 (created by P's second fork): returns 0 → condition false.
- Third child C3 (P's third fork) continues.
- Total processes = **4** → prints `X` 4 times.

### exec()
Replaces the current process image with a new program (PID stays the same).
```c
if (fork() == 0) {
    execlp("ls", "ls", "-l", NULL);   // child becomes `ls`
} else {
    wait(NULL);                        // parent waits
}
```

### wait() / waitpid()
Parent blocks until a child terminates and collects its exit status.

### Zombie Process
Child has **finished**, but the parent has **not called `wait()`**. Its PCB entry remains in the process table. Shows as `Z` in `ps`. Cleaned by `wait()` or when the parent dies (`init` adopts and reaps).

### Orphan Process
**Parent terminates before the child**. The child is adopted by `init` (PID 1) / `systemd`.

### Daemon Process
Background process with no controlling terminal (e.g., `sshd`, `cron`).

| | Zombie | Orphan |
|---|---|---|
| Who died | Child | Parent |
| Resource held | Only a process table entry | Still running normally |
| Fix | Parent calls `wait()` | Adopted by init |

---

## 7. Inter-Process Communication (IPC)

Processes are isolated, so they need IPC to cooperate.

### Two Models
1. **Shared Memory**: processes map a common memory region. Fast (no kernel after setup) but needs synchronization.
2. **Message Passing**: `send()` / `receive()` via the kernel. Slower but simpler and safe, good for distributed systems.

### IPC Mechanisms
| Mechanism | Notes |
|---|---|
| **Pipe** | Unidirectional, related processes (`ls \| grep txt`) |
| **Named pipe (FIFO)** | Pipe with a name in the file system; unrelated processes |
| **Message queue** | Kernel-managed list of messages |
| **Shared memory** | Fastest IPC |
| **Semaphore** | Synchronization primitive |
| **Socket** | Network or local communication |
| **Signal** | Asynchronous notification (`SIGINT`, `SIGKILL`) |
| **Memory-mapped file** | File mapped into address space |

### Pipe example
```c
int fd[2];
pipe(fd);                  // fd[0] = read end, fd[1] = write end
if (fork() == 0) {
    close(fd[0]);
    write(fd[1], "hi", 3);
} else {
    close(fd[1]);
    char buf[10];
    read(fd[0], buf, 10);
}
```

### Common Signals
| Signal | Meaning |
|---|---|
| SIGINT (2) | Ctrl+C |
| SIGKILL (9) | Cannot be caught or ignored |
| SIGTERM (15) | Polite termination |
| SIGSEGV (11) | Invalid memory access |
| SIGSTOP / SIGCONT | Pause / resume |

---

## 8. CPU Scheduling Algorithms

### Basic Terms
| Term | Meaning | Formula |
|---|---|---|
| **Arrival Time (AT)** | When the process enters the ready queue | |
| **Burst Time (BT)** | CPU time needed | |
| **Completion Time (CT)** | When it finishes | |
| **Turnaround Time (TAT)** | Total time in system | `CT − AT` |
| **Waiting Time (WT)** | Time spent in the ready queue | `TAT − BT` |
| **Response Time (RT)** | First CPU time − arrival | `First run − AT` |
| **Throughput** | Processes completed per unit time | |
| **CPU utilization** | % of time the CPU is busy | |

### Preemptive vs Non-Preemptive
- **Non-preemptive**: once the CPU is given, the process keeps it until it finishes or blocks (FCFS, non-preemptive SJF/Priority).
- **Preemptive**: OS can take the CPU away (SRTF, Round Robin, preemptive Priority).

### Common Example Used Below
| Process | AT | BT |
|---|---|---|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 8 |
| P4 | 3 | 6 |

---

### 8.1 FCFS (First Come First Served)
Non-preemptive, a simple FIFO queue.

Gantt: `| P1 (0-5) | P2 (5-8) | P3 (8-16) | P4 (16-22) |`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 5 | 5 | 0 |
| P2 | 8 | 7 | 4 |
| P3 | 16 | 14 | 6 |
| P4 | 22 | 19 | 13 |

Avg TAT = 45/4 = **11.25**, Avg WT = 23/4 = **5.75**

**Drawback: Convoy Effect.** Short processes wait behind one long CPU-bound process.

---

### 8.2 SJF (Shortest Job First), non-preemptive
Picks the ready process with the smallest burst time. **Optimal for minimum average waiting time** (non-preemptive, all arrive together).

Gantt: `| P1 (0-5) | P2 (5-8) | P4 (8-14) | P3 (14-22) |`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 5 | 5 | 0 |
| P2 | 8 | 7 | 4 |
| P3 | 22 | 20 | 12 |
| P4 | 14 | 11 | 5 |

Avg TAT = 43/4 = **10.75**, Avg WT = 21/4 = **5.25**

**Drawback:** starvation of long jobs; needs the burst time in advance (usually estimated with exponential averaging: `τ(n+1) = α·t(n) + (1−α)·τ(n)`).

---

### 8.3 SRTF (Shortest Remaining Time First), preemptive SJF
Whenever a new process arrives, compare its burst with the remaining time of the running one.

Gantt: `| P1 (0-1) | P2 (1-4) | P1 (4-8) | P4 (8-14) | P3 (14-22) |`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 8 | 8 | 3 |
| P2 | 4 | 3 | 0 |
| P3 | 22 | 20 | 12 |
| P4 | 14 | 11 | 5 |

Avg TAT = 42/4 = **10.5**, Avg WT = 20/4 = **5.0**

---

### 8.4 Round Robin (RR), time quantum q = 2
Preemptive, designed for time-sharing. Each process gets at most `q` units, then goes to the back of the queue.

Gantt: `| P1 0-2 | P2 2-4 | P3 4-6 | P1 6-8 | P4 8-10 | P2 10-11 | P3 11-13 | P1 13-14 | P4 14-16 | P3 16-18 | P4 18-20 | P3 20-22 |`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 14 | 14 | 9 |
| P2 | 11 | 10 | 7 |
| P3 | 22 | 20 | 12 |
| P4 | 20 | 17 | 11 |

Avg TAT = 61/4 = **15.25**, Avg WT = 39/4 = **9.75**

**Quantum choice**
- Very large q → behaves like FCFS.
- Very small q → too many context switches (overhead).
- Rule of thumb: 80% of CPU bursts should be shorter than q.

**Queue convention:** if a new arrival and a preempted process hit the queue at the same time, the new arrival goes first (state this assumption in exams).

---

### 8.5 Priority Scheduling
CPU goes to the highest-priority process (a lower number often means higher priority). Can be preemptive or non-preemptive.

Example (non-preemptive, all AT = 0):
| P | BT | Priority |
|---|---|---|
| P1 | 10 | 3 |
| P2 | 1 | 1 |
| P3 | 2 | 4 |
| P4 | 1 | 5 |
| P5 | 5 | 2 |

Order: P2 → P5 → P1 → P3 → P4. Gantt: `| P2 0-1 | P5 1-6 | P1 6-16 | P3 16-18 | P4 18-19 |`
WT: P2=0, P5=1, P1=6, P3=16, P4=18 → Avg = 41/5 = **8.2**

**Problem: Starvation**. **Solution: Aging** (gradually raise the priority of waiting processes).

---

### 8.6 HRRN (Highest Response Ratio Next), non-preemptive
`Response Ratio = (Waiting time + Burst time) / Burst time`
Choose the highest ratio. It reduces starvation compared to SJF because waiting time increases the ratio.

---

### 8.7 Multilevel Queue (MLQ)
Ready queue split into permanent queues (e.g., System > Interactive > Batch), each with its own algorithm. The process is fixed to one queue. Scheduling is between queues, by fixed priority or time slice.

### 8.8 Multilevel Feedback Queue (MLFQ)
Like MLQ but processes **move between queues** based on behavior.
- New process enters the top queue (small q).
- If it uses its full quantum, it is demoted.
- If it blocks early (I/O-bound), it stays high.
- Long waiting → promoted (aging).
It is the most general and most complex algorithm. Used in real OSes.

### 8.9 Other Schedulers
- **Lottery scheduling**: each process holds tickets; a random draw picks the winner.
- **Linux CFS (Completely Fair Scheduler)**: uses a red-black tree ordered by *virtual runtime*; the lowest vruntime runs next.
- **Real-time**: **EDF** (Earliest Deadline First), **Rate Monotonic** (shorter period = higher priority).

### Comparison
| Algorithm | Preemptive | Starvation | Notes |
|---|---|---|---|
| FCFS | No | No | Convoy effect |
| SJF | No | Yes | Min avg WT |
| SRTF | Yes | Yes | Min avg WT overall |
| RR | Yes | No | Good response time |
| Priority | Both | Yes | Use aging |
| HRRN | No | No | Fair |
| MLFQ | Yes | Possible (without aging) | Flexible |

### Multiprocessor Scheduling (brief)
- **Asymmetric**: one master CPU runs OS code.
- **SMP**: each CPU self-schedules.
- **Processor affinity**: keep a process on the same CPU for cache benefits (soft/hard).
- **Load balancing**: push/pull migration.

---

## 9. Process Synchronization

### Why?
When multiple processes/threads access shared data concurrently, results depend on the order of execution: a **race condition**.

**Example (race condition)**: `counter++` is really three steps (load, add, store).
```
Initial counter = 5
T1: LOAD  r1 = 5
T2: LOAD  r2 = 5
T1: ADD   r1 = 6
T2: ADD   r2 = 6
T1: STORE counter = 6
T2: STORE counter = 6     <-- expected 7, got 6 (lost update)
```

### Critical Section Problem
Each process has:
```
do {
    [ Entry Section ]
    [ Critical Section ]   // access shared resource
    [ Exit Section ]
    [ Remainder Section ]
} while (true);
```

### Three Requirements for a Solution
1. **Mutual Exclusion**: only one process in the CS at a time.
2. **Progress**: if the CS is free, only the processes wanting entry decide who goes next, with no indefinite postponement.
3. **Bounded Waiting**: a limit exists on how many times others can enter before a waiting process gets in (no starvation).

### Peterson's Solution (2 processes, software)
```c
bool flag[2] = {false, false};
int turn;

// Process i (j = 1 - i)
flag[i] = true;       // I want to enter
turn = j;             // but you go first
while (flag[j] && turn == j);   // busy wait
    /* critical section */
flag[i] = false;
```
Satisfies mutual exclusion, progress and bounded waiting. It may fail on modern CPUs with instruction reordering unless memory barriers are used.

### Hardware Support
**Test-and-Set**
```c
bool test_and_set(bool *lock) {
    bool old = *lock;
    *lock = true;
    return old;            // executed atomically
}

while (test_and_set(&lock));   // spin
    /* critical section */
lock = false;
```
**Compare-and-Swap (CAS)**, **atomic variables**, and disabling interrupts (works only on uniprocessors).

### Mutex Lock
Binary lock with `acquire()` / `release()`. **Only the owner can unlock it.**
```c
pthread_mutex_lock(&m);
   // critical section
pthread_mutex_unlock(&m);
```
**Spinlock**: busy-waits (good for very short waits on multicore). A sleeping lock blocks the thread.

### Semaphore
Integer variable accessed only through atomic `wait(P)` and `signal(V)`.
```c
wait(S):   while (S <= 0);  S--;      // P, down
signal(S): S++;                       // V, up
```
| | Counting Semaphore | Binary Semaphore |
|---|---|---|
| Range | 0 to N | 0 or 1 |
| Use | Manage N identical resources | Mutual exclusion / signalling |

**Mutex vs Semaphore**
| Mutex | Semaphore |
|---|---|
| Locking mechanism (ownership) | Signalling mechanism |
| Only the locker unlocks | Any process can signal |
| Binary | Counting or binary |

Implementation without busy waiting: a blocked process is put into the semaphore's queue (`block()`), and `signal` calls `wakeup()`.

### Monitor
High-level construct: shared data + procedures + **only one process active inside at a time** (automatic mutual exclusion). Uses **condition variables** with `wait()` and `signal()`. Java's `synchronized` is a monitor.

### Condition Variable Example (Java style)
```java
synchronized void put(int x) {
    while (full) wait();
    buffer.add(x);
    notifyAll();
}
```

### Other Terms
- **Busy waiting**: continuously checking a condition (wastes CPU).
- **Priority inversion**: a low-priority process holds a lock needed by a high-priority one, while a medium-priority process preempts the low one. **Fix: priority inheritance** (Mars Pathfinder bug).
- **Atomic operation**: indivisible.
- **Thread-safe**: correct under concurrent use.
- **Reentrant lock**: the same thread can acquire it again.

---

## 10. Classic Synchronization Problems

### 10.1 Producer-Consumer (Bounded Buffer)
Buffer size N. Semaphores: `mutex = 1`, `empty = N`, `full = 0`.
```c
// Producer
do {
    produce_item();
    wait(empty);       // wait for a free slot
    wait(mutex);
    add_item_to_buffer();
    signal(mutex);
    signal(full);      // one more item
} while (1);

// Consumer
do {
    wait(full);        // wait for an item
    wait(mutex);
    remove_item();
    signal(mutex);
    signal(empty);     // one more free slot
    consume_item();
} while (1);
```
**Warning:** swapping `wait(empty)` and `wait(mutex)` can cause **deadlock**.

### 10.2 Readers-Writers
Many readers can read together, but a writer needs exclusive access.
```c
semaphore rw = 1, mutex = 1;
int read_count = 0;

// Writer
wait(rw);
   write();
signal(rw);

// Reader
wait(mutex);
read_count++;
if (read_count == 1) wait(rw);    // first reader locks writers out
signal(mutex);

   read();

wait(mutex);
read_count--;
if (read_count == 0) signal(rw);  // last reader lets writers in
signal(mutex);
```
This is the "readers-preference" version, and writers may starve.

### 10.3 Dining Philosophers
5 philosophers, 5 forks (chopsticks). Each needs 2 forks to eat.
```c
semaphore fork[5] = {1,1,1,1,1};

// Naive: philosopher i
wait(fork[i]);
wait(fork[(i+1)%5]);
   eat();
signal(fork[i]);
signal(fork[(i+1)%5]);
```
If all pick up their left fork at once, **deadlock**.

**Solutions**
1. Allow at most 4 philosophers at the table.
2. Pick up both forks only if both are available (inside a critical section).
3. **Asymmetric**: odd philosophers pick left first, even pick right first (breaks circular wait).
4. Use a waiter / monitor.

### 10.4 Sleeping Barber
One barber, N waiting chairs. The barber sleeps if there are no customers; customers leave if the chairs are full. Uses semaphores `customers`, `barber`, `mutex`.

### 10.5 Other Concepts
- **Barrier**: all threads wait until everyone reaches a point.
- **Deadlock in locks**: `Thread A: lock(X); lock(Y)` vs `Thread B: lock(Y); lock(X)`.

---

## 11. Deadlock

A set of processes is **deadlocked** when each is waiting for a resource held by another in the set, and none can proceed.

**Real example:** Process A holds the printer and requests the scanner. Process B holds the scanner and requests the printer. Both wait forever.

### System Model
Request → Use → Release.

### Four Necessary Conditions (Coffman). ALL must hold:
1. **Mutual Exclusion**: resource is non-shareable.
2. **Hold and Wait**: holds at least one resource while waiting for others.
3. **No Preemption**: resources cannot be forcibly taken away.
4. **Circular Wait**: a cycle P0→P1→...→Pn→P0 of waiting.

### Resource Allocation Graph (RAG)
- Circle = process, rectangle = resource (dots = instances).
- Request edge: P → R. Assignment edge: R → P.
- **No cycle** → no deadlock.
- **Cycle with single-instance resources** → deadlock.
- **Cycle with multi-instance resources** → deadlock is *possible* but not certain.

```
P1 ──requests──► R2        R1 ──held by──► P1
P2 ──requests──► R1        R2 ──held by──► P2      => cycle => deadlock
```

### Methods of Handling Deadlock
1. **Prevention**: ensure at least one condition can never hold.
2. **Avoidance**: dynamically check that the state stays safe (Banker's).
3. **Detection and Recovery**: allow deadlock, find it, fix it.
4. **Ignore (Ostrich algorithm)**: used by Windows/Linux for user-level deadlocks.

### 11.1 Deadlock Prevention
| Break | How | Drawback |
|---|---|---|
| Mutual exclusion | Make resources shareable (e.g., read-only files), spooling | Not possible for all |
| Hold & Wait | Request all resources at start, or release before requesting new | Low utilization, starvation |
| No preemption | If a request fails, release held resources; or preempt | Works only for savable states (CPU registers, memory) |
| Circular wait | Impose a **global ordering** of resources; request only in increasing order | Inflexible |

Lock ordering example: always lock `A` before `B` in all threads, so no cycle.

### 11.2 Deadlock Avoidance, Safe State
- **Safe state**: a **safe sequence** exists, in which every process can finish in some order.
- **Unsafe state**: no safe sequence (may lead to deadlock, but doesn't always).
- Deadlock ⊂ Unsafe states.

### Banker's Algorithm (multiple instances)
Data structures (n processes, m resource types):
- `Available[m]`: free instances.
- `Max[n][m]`: max demand of each process.
- `Allocation[n][m]`: currently held.
- `Need[n][m] = Max − Allocation`.

**Safety algorithm**
1. `Work = Available`; `Finish[i] = false` for all.
2. Find `i` with `Finish[i] == false` and `Need[i] <= Work`.
3. If found: `Work += Allocation[i]`, `Finish[i] = true`, repeat step 2.
4. If all `Finish` are true → **safe**.

### Worked Example
Resources A=10, B=5, C=7. Available = (3, 3, 2).

| P | Allocation (A B C) | Max (A B C) | Need = Max − Alloc |
|---|---|---|---|
| P0 | 0 1 0 | 7 5 3 | 7 4 3 |
| P1 | 2 0 0 | 3 2 2 | 1 2 2 |
| P2 | 3 0 2 | 9 0 2 | 6 0 0 |
| P3 | 2 1 1 | 2 2 2 | 0 1 1 |
| P4 | 0 0 2 | 4 3 3 | 4 3 1 |

Steps:
1. Work = (3,3,2). P1 need (1,2,2) ≤ Work ✔ → Work = (3,3,2)+(2,0,0) = **(5,3,2)**
2. P3 need (0,1,1) ✔ → Work = (5,3,2)+(2,1,1) = **(7,4,3)**
3. P4 need (4,3,1) ✔ → Work = (7,4,3)+(0,0,2) = **(7,4,5)**
4. P0 need (7,4,3) ✔ → Work = (7,4,5)+(0,1,0) = **(7,5,5)**
5. P2 need (6,0,0) ✔ → Work = (7,5,5)+(3,0,2) = **(10,5,7)**

**Safe sequence: ⟨P1, P3, P4, P0, P2⟩**. The system is safe.

**Resource-Request Algorithm:** when Pi requests `Request[i]`: check `Request ≤ Need`, check `Request ≤ Available`, then *pretend* to allocate (`Available −= Request`, `Allocation += Request`, `Need −= Request`) and run the safety algorithm. If safe → grant; else roll back and make Pi wait.

**RAG algorithm** (single-instance) uses *claim edges*; a request is granted only if converting the claim edge into an assignment edge creates no cycle.

### 11.3 Deadlock Detection
- **Single instance**: build a **wait-for graph** (remove resource nodes). A cycle means deadlock.
- **Multiple instances**: run a Banker's-like detection algorithm using the current `Request` matrix.
- Run detection periodically or when CPU utilization drops.

### 11.4 Recovery
**Process termination**: abort all deadlocked processes, or abort one at a time until the cycle breaks (choose by priority, runtime, resources held).
**Resource preemption**: select a victim, **rollback** it to a safe state, and avoid starvation (don't always pick the same victim).

### Quick formula (single resource type)
If each of `P` processes needs at most `R` units and the total is `N`, the system is **deadlock-free** if
`N ≥ P × (R − 1) + 1`.
Example: 3 processes each need 4 resources → minimum for deadlock-free = 3×3+1 = **10**.

---

## 12. Memory Management

**Goal:** share limited RAM between many processes, with protection and efficiency.

### Address Types
| | Logical (Virtual) Address | Physical Address |
|---|---|---|
| Generated by | CPU | Memory unit |
| Seen by | Program | Memory hardware |
| Mapping | Done by the **MMU** (Memory Management Unit) at run time | |

### Address Binding Times
Compile time (absolute code) → Load time (relocatable) → **Run time** (needs hardware MMU; used by modern OS).

### Relocation Register Example
Base (relocation) = 14000, Limit = 3000. Logical address 346 → physical = 14000+346 = **14346**. Logical 3500 > limit → **trap** (protection fault).

### Loading and Linking
- **Static linking**: libraries copied into the executable (bigger file).
- **Dynamic linking**: libraries (DLL / .so) linked at runtime (shared, smaller).
- **Dynamic loading**: routine loaded only when called.
- **Swapping**: move a process to the backing store and back.
- **Overlays**: load only the needed part of a program (old technique).

---

### 12.1 Contiguous Memory Allocation
Each process occupies one contiguous block.

**Fixed (static) partitioning**: memory split into fixed-size partitions. Suffers from **internal fragmentation**.
**Variable (dynamic) partitioning**: partitions created to fit the process size. Suffers from **external fragmentation**.

### Allocation Strategies (for variable partitions)
Free holes: 100K, 500K, 200K, 300K, 600K. Request: 212K, 417K, 112K, 426K.

| Strategy | Rule | Result |
|---|---|---|
| **First Fit** | First hole big enough | 212→500 (rem 288); 417→600 (rem 183); 112→288 hole (rem 176); 426→**fails** |
| **Best Fit** | Smallest hole that fits | 212→300; 417→500; 112→200; 426→600 ✔ all allocated |
| **Worst Fit** | Largest hole | 212→600 (rem 388); 417→500 (rem 83); 112→388; 426→**fails** |
| **Next Fit** | Like first fit but starts from the last allocation point | |

- First Fit is fast; Best Fit leaves tiny unusable holes and is slow; Worst Fit is generally poor.

### Fragmentation
| | Internal | External |
|---|---|---|
| What | Wasted space **inside** an allocated block | Free space is scattered in small holes |
| Occurs in | Fixed partition, paging (last page) | Variable partition, segmentation |
| Fix | Smaller/more flexible blocks | **Compaction** (shift processes together), paging |

*50-percent rule:* with first-fit, for every N allocated blocks, about N/2 blocks are lost to fragmentation.

---

### 12.2 Paging
Non-contiguous allocation. Physical memory is divided into fixed-size **frames**; logical memory into same-size **pages**. A **page table** maps page → frame. This **eliminates external fragmentation** (internal fragmentation is only in the last page).

**Logical address = Page number (p) | Page offset (d)**

Example: page size = 4 KB = 2^12. 32-bit logical address → offset 12 bits, page number 20 bits.
Logical address `0x00003ABC` → page `3`, offset `0xABC`. If page 3 → frame 7, physical = `7 × 4096 + 0xABC` = `0x7ABC`.

```
Logical addr: [ p | d ]  --page table[p]--> frame f  --> physical: [ f | d ]
```

**Page table entry (PTE)** contains: frame number, valid/invalid bit, protection bits (r/w/x), dirty (modified) bit, reference bit.

**Calculations**
- Number of pages = Process size / Page size
- Page table size = (Number of pages) × (PTE size)
- Number of frames = Physical memory / Page size
- Logical address bits = log2(logical address space); Physical = log2(physical memory)

Example: Logical address space 2^32 B, page size 4 KB, PTE 4 B → pages = 2^32/2^12 = 2^20; page table = 2^20 × 4 B = **4 MB** per process.

### TLB (Translation Lookaside Buffer)
Small, fast, associative cache of recent page→frame translations. Without it, every access costs 2 memory accesses (page table + data).

**Effective Access Time (EAT)**
`EAT = h × (T_tlb + T_mem) + (1 − h) × (T_tlb + 2·T_mem)`
Example: hit ratio 90%, TLB = 10 ns, memory = 100 ns:
EAT = 0.9×(10+100) + 0.1×(10+100+100) = 99 + 21 = **120 ns**

- On a context switch, the TLB is flushed or tagged with **ASIDs** (address-space IDs).

### Multi-level Paging
Page table itself is paged, so a huge page table doesn't need contiguous memory.
- 2-level: `[p1 | p2 | d]`. Saves memory since unused regions need no table.
- x86-64 uses 4-level paging.

### Hashed and Inverted Page Table
- **Hashed page table**: hash of page number → chain of entries (good for >32-bit address spaces).
- **Inverted page table**: one entry **per physical frame** `(pid, page)`. Less memory, slower search (use hashing).

### Shared Pages
Reentrant code (e.g., libc) can be mapped to the same frames in many processes. **Copy-on-Write (COW)**: after `fork()`, parent and child share pages until one writes, and then the page is copied.

---

### 12.3 Segmentation
Memory divided into **logical** variable-size segments (code, stack, heap, data). Logical address = `<segment number, offset>`.

- Segment table has `base` and `limit` for each segment.
- If `offset >= limit` → trap.
- Example: segment 2 has base 4300, limit 400. Address `<2, 53>` → 4353. Address `<2, 500>` → trap.
- Pros: matches the programmer's view, supports sharing/protection per segment. Cons: **external fragmentation**.

| Paging | Segmentation |
|---|---|
| Fixed size | Variable size |
| Invisible to user | Visible to user |
| Internal fragmentation | External fragmentation |
| Page table | Segment table |

**Segmented paging** combines both (used in Intel x86): segment → page table → frame.

---

## 13. Virtual Memory and Page Replacement

### Virtual Memory
Lets a process run even if only **part** of it is in RAM, using disk as an extension. Programs can be bigger than physical memory. Implemented via **demand paging**.

### Demand Paging
A page is loaded **only when referenced**. The valid/invalid bit shows if the page is in memory.

**Page Fault steps**
1. CPU references a page whose valid bit = 0 → **trap** to OS.
2. OS checks that the reference is legal (else terminate, segmentation fault).
3. Finds a free frame (or selects a victim using page replacement).
4. Reads the page from disk into the frame.
5. Updates the page table (valid = 1).
6. Restarts the interrupted instruction.

**Effective Access Time with page faults**
`EAT = (1 − p) × T_mem + p × T_fault_service`
Example: T_mem = 200 ns, fault service = 8 ms = 8,000,000 ns, p = 1/1000.
EAT = 0.999×200 + 0.001×8,000,000 = 199.8 + 8000 ≈ **8200 ns**, a 40x slowdown, so the page-fault rate must be very low.

**Pure demand paging**: start with zero pages in memory.
**Prepaging**: load pages in advance.

### Page Replacement Algorithms
Reference string: `7 0 1 2 0 3 0 4 2 3 0 3 2 1 2 0 1 7 0 1`, **3 frames**.

**FIFO**: replace the oldest page. Result: **15 faults**.
**Optimal (OPT/Belady's MIN)**: replace the page not needed for the longest time in the future. Result: **9 faults**. Best possible, but not implementable (needs future knowledge); used as a benchmark.
**LRU**: replace the least recently used page. Result: **12 faults**. Needs a counter or stack/hardware support.

**Short worked trace (OPT)** with the first part `7 0 1 2 0 3`:
- 7, 0, 1 → 3 faults (frames: 7 0 1)
- 2 → fault; evict the one used farthest in the future → 7 (frames: 2 0 1)
- 0 → hit
- 3 → fault; evict 1 (frames: 2 0 3)

**Belady's Anomaly**: with FIFO, giving *more* frames can produce *more* faults.
Reference string `1 2 3 4 1 2 5 1 2 3 4 5`: 3 frames → **9 faults**; 4 frames → **10 faults**. LRU and OPT (stack algorithms) never show it.

**LRU approximations**
- **Second-chance (Clock)**: FIFO + reference bit. If R=1, clear it and give a second chance; if R=0, replace.
- **Enhanced second chance**: uses (reference, modify) pairs: (0,0) best victim → (0,1) → (1,0) → (1,1).
- **LFU / MFU**: least/most frequently used.
- **Additional reference bits / aging**.

**Global vs local replacement**: global picks a victim from any process; local only from the faulting process's own frames.

### Frame Allocation
- **Equal**: m/n frames each.
- **Proportional**: by process size.
- **Priority-based**.
- Minimum frames required depend on the instruction set architecture.

### Thrashing
The system spends more time **paging than executing**; CPU utilization falls as the OS adds more processes in response (wrong reaction!).
- Cause: too many processes, not enough frames.
- **Working Set Model**: keep the working set (pages used in the last Δ references) of each process in memory; if the total demand exceeds available frames, suspend a process.
- **Page Fault Frequency (PFF)**: adjust frames according to the fault rate (too high → give more; too low → take away).

### Other Terms
- **Locality of reference**: temporal and spatial; basis for caching and paging.
- **Copy-on-write**, **memory-mapped files** (`mmap`).
- **Swap space**: disk area for evicted pages.
- **Stack vs Heap**: stack is auto-managed, fast, limited (stack overflow); heap is dynamic, manual, can leak or fragment.
- **Memory leak**: allocated memory never freed. **Dangling pointer**: pointer to freed memory.
- **Buddy system**: allocate in power-of-2 blocks, split and merge buddies (fast merging, internal fragmentation).
- **Slab allocator**: caches for kernel objects (avoids fragmentation).
- **Cache hierarchy**: Registers → L1 → L2 → L3 → RAM → SSD/HDD (faster = smaller = costlier).

---

## 14. File Systems

### File
Named collection of related data on secondary storage.

**Attributes:** name, identifier (inode), type, location, size, protection (permissions), timestamps, owner.
**Operations:** create, open, read, write, seek, delete, truncate, close.
**Open-file tables:** per-process table → system-wide open-file table → inode.

### File Types
Regular, directory, special (device), symbolic links, sockets, pipes. Extensions: `.exe`, `.txt`.

### Access Methods
| Method | Description |
|---|---|
| Sequential | Read in order (tape) |
| Direct (random) | Jump to any block (database) |
| Indexed | Index points to blocks |

### Directory Structures
| Structure | Notes |
|---|---|
| Single-level | All files in one directory; name clashes |
| Two-level | One directory per user |
| Tree | Hierarchical, absolute/relative paths (most common) |
| Acyclic graph | Shared files via links; no cycles |
| General graph | Cycles possible, needs garbage collection |

### Links
- **Hard link**: another directory entry for the same inode. The file survives until all links are removed. Cannot span file systems or link directories.
- **Soft (symbolic) link**: a special file containing a path. Breaks if the target is deleted.

### File Allocation Methods
| Method | Idea | Pros | Cons |
|---|---|---|---|
| **Contiguous** | File in consecutive blocks | Fast sequential and direct access; simple | External fragmentation; file growth hard |
| **Linked** | Each block points to the next | No external fragmentation; grows easily | Slow random access; pointer overhead; reliability |
| **FAT** (File Allocation Table) | Linked list kept in a table in memory | Faster random access than pure linked | Table size |
| **Indexed** | An index block holds all block pointers | Direct access, no external fragmentation | Index block overhead; linked/multilevel index for large files |

### UNIX inode
Stores metadata plus pointers: **12 direct**, 1 single-indirect, 1 double-indirect, 1 triple-indirect.

**Max file size example**: block = 4 KB, pointer = 4 B → 1024 pointers per block.
- Direct: 12 × 4 KB = 48 KB
- Single: 1024 × 4 KB = 4 MB
- Double: 1024² × 4 KB = 4 GB
- Triple: 1024³ × 4 KB = 4 TB
Total ≈ 4 TB + 4 GB + 4 MB + 48 KB.

(The inode stores everything except the **file name**, which lives in the directory entry.)

### Free-Space Management
- **Bit vector (bitmap)**: 1 bit per block (0 = free). Simple, fast to find contiguous blocks, but needs memory.
- **Linked list** of free blocks.
- **Grouping**, **Counting** (address + count of contiguous blocks).

### Directory Implementation
Linear list (simple, slow search) or hash table (fast, collisions).

### File System Concepts
- **Mounting**: attaching a file system to the directory tree.
- **Virtual File System (VFS)**: a common interface over different file systems (ext4, NTFS, NFS).
- **Journaling** (ext3/4, NTFS): log changes first to recover from crashes.
- **Buffer cache / page cache**.
- **Disk partition / formatting**: logical vs physical.
- Common FS: FAT32 (4 GB file limit), NTFS (Windows), ext4 (Linux), APFS (macOS).

### File Permissions (UNIX)
`-rwxr-xr--` = type, owner `rwx`, group `r-x`, others `r--`. Numeric: r=4, w=2, x=1.
`chmod 754 file` → owner 7 (rwx), group 5 (r-x), others 4 (r--).
Special bits: **setuid**, **setgid**, **sticky bit** (`/tmp`).

---

## 15. Disk Scheduling, Disk Structure and RAID

### Disk Structure
Platters → tracks → sectors; cylinder = same track on all platters.

**Disk access time = Seek time + Rotational latency + Transfer time**
- Seek: move head to the track (dominant).
- Rotational latency: average ≈ half a rotation (for 7200 RPM, one rotation = 8.33 ms, so avg ≈ 4.17 ms).
- Transfer: data read time.

### Disk Scheduling Algorithms
Request queue: `98, 183, 37, 122, 14, 124, 65, 67`. Head at **53**. Disk range 0–199.

| Algorithm | Idea | Head movement |
|---|---|---|
| **FCFS** | In arrival order | 45+85+146+85+108+110+59+2 = **640** |
| **SSTF** | Closest request next | 53→65→67→37→14→98→122→124→183 = **236** (can starve far requests) |
| **SCAN (Elevator)** | Go to one end, serving requests on the way, then reverse. Heading toward 0 first: 53→37→14→0→65→67→98→122→124→183 | **236** |
| **C-SCAN** | Go to one end, jump back to the start without serving, continue in the same direction (uniform wait). Toward 199 first: 53→...→183→199→(0)→14→37 | **382** (counting the 199 return jump; 183 without) |
| **LOOK** | SCAN but reverses at the last request instead of the disk end. 53→37→14→65→...→183 | **208** |
| **C-LOOK** | C-SCAN but jumps from the last request | |

Notes: SSTF/SCAN/LOOK suit heavy loads; SSD needs no seek scheduling (FCFS/NOOP).

### Disk Management
Low-level formatting, partitioning, boot block, bad-block handling. **Swap space** management.

### SSD vs HDD
SSD: no moving parts, fast random access, limited write cycles (wear levelling). HDD: cheap per GB, mechanical seek.

### RAID (Redundant Array of Independent Disks)
| Level | Technique | Min disks | Fault tolerance | Notes |
|---|---|---|---|---|
| **RAID 0** | Striping | 2 | None | Fastest, no redundancy |
| **RAID 1** | Mirroring | 2 | 1 disk | 50% usable capacity |
| **RAID 5** | Striping + distributed parity | 3 | 1 disk | Good balance, capacity (N−1) |
| **RAID 6** | Double parity | 4 | 2 disks | Capacity (N−2) |
| **RAID 10** | Mirror + stripe | 4 | 1 per mirror pair | Fast and safe, costly |

---

## 16. Deadlock vs Starvation vs Livelock

| Feature | Deadlock | Starvation | Livelock |
|---|---|---|---|
| Meaning | Processes wait forever for each other, **no progress** | A process waits **indefinitely** because others keep getting the resource | Processes keep **changing state** reacting to each other but make no progress |
| Process state | Blocked | Ready/blocked, but never scheduled | Running (active), but useless work |
| CPU usage | None | Low for the victim | High (wasted) |
| Affects | All processes in the cycle | Individual (low-priority) process | The processes involved |
| Cause | 4 Coffman conditions | Unfair scheduling/priority | Reactive retry loops |
| Fix | Prevention, avoidance, recovery | **Aging**, fair scheduling | Random backoff, ordering |
| Example | A holds R1 wants R2; B holds R2 wants R1 | Low-priority process never gets CPU under SJF/priority | Two people in a corridor both stepping aside repeatedly; two threads retrying `tryLock` in sync |

- Deadlock is permanent and **a special case of starvation** (everyone starves), but starvation does not imply deadlock.

---

## 17. System Calls

A **system call** is the programmatic way for a user program to request a service from the kernel. Triggered via a **trap/software interrupt** (`int 0x80`, `syscall`), switching to kernel mode.

### Flow (`read()` example)
```
User program calls read()
   → libc wrapper puts syscall number in a register
   → trap instruction (mode switch to kernel)
   → kernel's system call table lookup → sys_read()
   → work done, result placed in register
   → return to user mode
```
Parameters are passed via registers, a memory block/table (pointer in register), or the stack.

### API vs System Call
An **API** (POSIX, Win32) is a library interface; one API function may use zero, one or many system calls (`printf` → `write`).

### Categories
| Category | Unix | Windows |
|---|---|---|
| Process control | `fork, exec, exit, wait, kill` | `CreateProcess, ExitProcess, WaitForSingleObject` |
| File management | `open, read, write, close, lseek, unlink, stat` | `CreateFile, ReadFile, WriteFile, CloseHandle` |
| Device management | `ioctl, read, write` | `SetConsoleMode, ReadConsole` |
| Information maintenance | `getpid, alarm, sleep, time` | `GetCurrentProcessId, SetTimer` |
| Communication | `pipe, shmget, mmap, socket, send, recv` | `CreatePipe, CreateFileMapping` |
| Protection | `chmod, chown, umask` | `SetFileSecurity` |

### Example
```c
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = open("a.txt", O_RDONLY);        // system call
    char buf[100];
    int n = read(fd, buf, 100);              // system call
    write(1, buf, n);                        // fd 1 = stdout
    close(fd);
}
```
Standard file descriptors: **0 = stdin, 1 = stdout, 2 = stderr**.

### Why can't users call kernel code directly?
For protection: it prevents user programs from accessing hardware or other processes' memory.

---

## 18. Interrupts and Context Switching

### Interrupt
A signal to the CPU that an event needs immediate attention. The CPU stops the current work, saves state, runs an **Interrupt Service Routine (ISR)**, then resumes.

| Type | Source | Example |
|---|---|---|
| **Hardware interrupt** (asynchronous) | External devices | Keyboard press, disk I/O complete, timer |
| **Software interrupt / Trap** (synchronous) | Program instruction | System call, `int 0x80` |
| **Exception** | CPU error during execution | Divide by zero, page fault, invalid opcode |

- **Maskable** interrupts can be disabled (INTR); **non-maskable** (NMI) cannot (hardware failure).
- **Fault** (restartable: page fault), **trap** (after the instruction: breakpoint/syscall), **abort** (unrecoverable).

### Interrupt Handling Steps
1. Device raises an interrupt line.
2. CPU finishes the current instruction.
3. Saves PC and flags (state) on the stack.
4. Looks up the **Interrupt Vector Table (IVT)** → address of the ISR.
5. Executes the ISR in kernel mode.
6. Restores state (`iret`) and resumes.

- **Interrupt vs Polling**: polling = CPU repeatedly checks the device (wasteful); interrupt = the device notifies the CPU.
- **Timer interrupt** gives the OS control back for preemptive scheduling.
- **Interrupt latency**: time from interrupt to the start of the ISR.
- **DMA (Direct Memory Access)**: a controller transfers blocks between device and memory without the CPU; one interrupt at the end.

### Context Switch
Switching the CPU from one process/thread to another.

**Steps**
1. Save the current process's context (registers, PC, stack pointer) into its **PCB**.
2. Update its state (Running → Ready/Waiting).
3. Select the next process (scheduler).
4. Load the saved context of the new process from its PCB.
5. Resume execution at its saved PC.

```
P1 running → [interrupt/syscall] → save P1 state to PCB1 → load P2 state from PCB2 → P2 running
```

- Context switch is **pure overhead** (no useful work); typical cost is microseconds.
- Costs: saving registers, TLB flush, cache pollution (cold caches).
- Thread switches within a process are cheaper (no address space switch).
- Triggers: time slice expiry, I/O request, higher-priority arrival, interrupt, termination.
- **Mode switch** (user→kernel) is cheaper than a full **context switch** (it changes no process).

---

## 19. I/O Management

- **I/O techniques**: Programmed I/O (polling), Interrupt-driven I/O, DMA.
- **Device controller / driver**: the controller is hardware; the driver is the OS code talking to it.
- **Buffering**: temporary storage to handle speed mismatch (single, double, circular buffers).
- **Caching**: a fast copy of data. **Spooling**: queue jobs for a device (print spooler).
- **Blocking vs Non-blocking vs Asynchronous I/O**: blocking suspends the process; non-blocking returns immediately; asynchronous notifies on completion.
- **Memory-mapped I/O vs Port-mapped I/O**: device registers appear in the memory address space or use special I/O instructions.
- **Device types**: block devices (disk), character devices (keyboard), network devices.

---

## 20. Security and Protection

- **Authentication** (who are you: password, biometrics, MFA) vs **Authorization** (what can you do).
- **Access Control**: ACL (per object list), Capability list (per subject).
- **Protection domains**, **principle of least privilege**.
- **Hardware protection**: dual mode, memory protection (base/limit registers), CPU timer (prevent infinite loops).
- **Threats**: virus, worm, trojan, ransomware, buffer overflow, privilege escalation, DoS.
- **Buffer overflow**: writing past an array's bounds can overwrite the return address. Mitigations: stack canaries, **ASLR**, **DEP/NX bit**.
- **Cryptography basics**: symmetric vs asymmetric, hashing for passwords (with salt).

---

## 21. Linux Commands for OS Questions

| Task | Command |
|---|---|
| List processes | `ps aux`, `top`, `htop` |
| Kill process | `kill -9 PID`, `pkill name` |
| Background / foreground | `cmd &`, `jobs`, `fg`, `bg` |
| Process tree | `pstree` |
| Memory usage | `free -h`, `vmstat` |
| Disk usage | `df -h`, `du -sh dir` |
| Open files | `lsof` |
| Change priority | `nice -n 10 cmd`, `renice` |
| File permissions | `chmod`, `chown`, `ls -l` |
| Links | `ln file hard`, `ln -s file soft` |
| Trace system calls | `strace ./a.out` |
| Mount | `mount`, `umount` |
| Inode info | `ls -i`, `stat file` |
| Scheduling policy | `chrt` |

---

## 22. Formula Cheat Sheet

| Concept | Formula |
|---|---|
| Turnaround time | `CT − AT` |
| Waiting time | `TAT − BT` |
| Response time | `First CPU − AT` |
| CPU utilization | `(Busy time / Total time) × 100` |
| Throughput | `Processes completed / Time` |
| Response ratio (HRRN) | `(W + S) / S` |
| Banker's Need | `Max − Allocation` |
| Deadlock-free resources | `N ≥ P(R−1) + 1` |
| Number of pages | `Process size / Page size` |
| Number of frames | `RAM size / Page size` |
| Page table size | `No. of pages × PTE size` |
| TLB EAT | `h(t+m) + (1−h)(t+2m)` |
| Page fault EAT | `(1−p)·m + p·fault_time` |
| Page hit ratio | `Hits / References` |
| Page fault ratio | `Faults / References` |
| Disk access time | `Seek + Rotational latency + Transfer` |
| Rotational latency (avg) | `½ × (60/RPM)` seconds |
| Processes after n forks | `2^n` |
| Logical address bits | `log2(virtual address space)` |
| Physical address bits | `log2(RAM)` |
| Offset bits | `log2(page size)` |

---

## 23. Most Asked Interview / OA Questions

**Q1. Difference between process and program?**
A program is a passive file on disk (executable); a process is the active instance in memory with PCB, stack, heap, etc.

**Q2. What is a race condition?**
The outcome depends on the sequence/timing of access to shared data by concurrent threads. Fix with locks/semaphores/atomics.

**Q3. Mutex vs Semaphore vs Monitor?**
Mutex: ownership-based lock for mutual exclusion. Semaphore: counter for signalling/resource counting. Monitor: language-level construct that bundles data, mutual exclusion and condition variables.

**Q4. What are the Coffman conditions?**
Mutual exclusion, hold and wait, no preemption, circular wait.

**Q5. Why is SJF optimal but impractical?**
It gives minimum average waiting time but needs the future burst time (only estimable), and may starve long jobs.

**Q6. What is thrashing and how do you stop it?**
Excessive paging with little useful work. Reduce multiprogramming, use the working set model or PFF, add RAM.

**Q7. Paging vs Segmentation?**
Paging: fixed-size, physical division, internal fragmentation. Segmentation: variable-size, logical division, external fragmentation.

**Q8. What is a TLB?**
A cache for page-table entries, which reduces memory access time from 2 accesses to ~1 on a hit.

**Q9. What is virtual memory?**
An abstraction giving each process a large contiguous address space, backed by RAM + disk, using demand paging.

**Q10. Why is a context switch expensive?**
State save/restore, cache and TLB invalidation, scheduler overhead; no useful work done.

**Q11. What happens when you type `ls` in a shell?**
Shell calls `fork()` → child calls `exec("ls")` → `ls` uses `open/read/write` syscalls → output printed → child `exit()` → shell `wait()` returns and the prompt reappears.

**Q12. What happens on a page fault?** See Section 13: trap → check validity → find a frame → disk read → update table → restart instruction.

**Q13. Can a thread have its own address space?**
No. Threads share their process's address space, but each has a private stack and registers.

**Q14. Does a kernel need to be in main memory always?**
Core parts (resident) yes; some modules/drivers can be loaded dynamically.

**Q15. Hard link vs soft link?**
Hard: same inode, survives deletion of the original name. Soft: path-based pointer, dangles if the target is removed.

**Q16. What is `fork()` vs `vfork()` vs `clone()`?**
`fork`: copy (with COW). `vfork`: child shares the parent's address space until `exec` (parent suspended). `clone`: fine-grained control, used to create threads in Linux.

**Q17. Internal vs External fragmentation?** See Section 12.

**Q18. What is a deadlock-free approach for locks in code?**
Consistent global lock ordering, `tryLock` with timeout, lock-free structures, minimize lock scope.

**Q19. What is the difference between preemptive and non-preemptive kernels?**
A preemptive kernel can be interrupted while running kernel code (better responsiveness for real-time), at the price of more synchronization complexity.

**Q20. What is a pipe? Can unrelated processes use it?**
A unidirectional byte stream. An anonymous pipe works between related processes only; a named pipe (FIFO) works for unrelated ones.

### Quick Revision Tips for OA
- Always **draw the Gantt chart** for scheduling and write CT → TAT → WT in a table.
- For page replacement, **draw frames per reference** and mark hit (H) / fault (F).
- For Banker's, **write the Need matrix first**, then iterate the safety check.
- For fork questions, count with `2^n` and watch for `&&`, `||` and `if` conditions.
- For paging math, convert everything to **powers of 2** and split address bits.
- Memorise the four deadlock conditions, the three CS requirements, and the process states.

---
*End of notes. Good luck with your OA!*

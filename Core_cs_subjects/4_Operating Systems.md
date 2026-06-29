# 🖥️ Operating System — Complete Placement Notes
> *Theory + ASCII Diagrams + C++ & Java Code + Interview MCQs*

```
┌─────────────────────────────────────────────────────────┐
│   USER  →  APPLICATION  →  OPERATING SYSTEM  →  HARDWARE │
└─────────────────────────────────────────────────────────┘
```

---

## 📑 Table of Contents
1. [Introduction to OS](#1-introduction-to-os)
2. [Process Management](#2-process-management)
3. [CPU Scheduling](#3-cpu-scheduling)
4. [Process Synchronization](#4-process-synchronization)
5. [Deadlock](#5-deadlock)
6. [Inter-Process Communication](#6-inter-process-communication)
7. [Memory Management](#7-memory-management)
8. [File Systems](#8-file-systems)
9. [Disk Management](#9-disk-management)
10. [🎯 Placement Cheat-Sheet](#10-placement-cheat-sheet)

---

## 1. Introduction to OS

### 📌 What is an OS?
A system software acting as a bridge between **user applications** and **hardware**.

```
┌────────┐     ┌─────────────┐     ┌────────────────┐     ┌──────────┐
│  User  │ ──▶ │ Application │ ──▶ │ Operating System│ ──▶ │ Hardware │
└────────┘     └─────────────┘     └────────────────┘     └──────────┘
```

Examples: Windows, Linux, macOS, Android.

### ⚡ Why is an OS Needed?
| Problem without OS | Why it matters |
|---|---|
| Complex/bulky apps | Every app would need its own hardware-talk code |
| Uncontrolled resource usage | One app could hog CPU/memory/I-O |
| No memory protection | Apps could overwrite each other's memory |

### 🧠 Main Functions of OS
```
                        ┌────────────────────┐
        ┌──────────────▶│        OS          │◀──────────────┐
        │               └────────┬───────────┘                │
 Memory Mgmt                Process Mgmt                  Security
        │               ┌────────┴───────────┐                │
        └──────────────▶│   Device Mgmt /     │◀──────────────┘
                         │   File Mgmt         │
                         └────────────────────┘
```
1. Process Management 2. Resource Allocation 3. Memory Management
4. File System Management 5. Device Management 6. Security 7. Process Scheduling

### 🏗️ Types of OS
| Type | Key Idea | Example |
|---|---|---|
| Batch | Jobs grouped, no manual intervention | Old payroll systems |
| Multiprogramming | Multiple programs in RAM, CPU switches | Early UNIX |
| Multitasking | Single user, many programs at once | Windows/Linux desktop |
| Real-Time (RTOS) | Immediate, predictable response | Satellites, airbags |
| Distributed | Many computers act as one system | Cloud clusters |

### 🔑 Kernel & Modes
```
 USER MODE  (restricted)         KERNEL MODE (privileged, full HW access)
 ┌──────────────────┐    trap/   ┌──────────────────────────┐
 │  User Programs    │  syscall  │   Kernel (the "heart")    │
 │  (limited access)  │ ───────▶ │   Full HW access          │
 └──────────────────┘            └──────────────────────────┘
```

### 🧩 OS Structures
| Structure | Idea | Example |
|---|---|---|
| Monolithic | Whole OS = 1 big program in kernel | Linux, UNIX |
| Microkernel | Only core services in kernel | Mach, MINIX |
| Layered | OS built in stacked layers | THE |
| Modular | Independent pluggable modules | Linux (modules) |

### 📞 System Calls
```
USER MODE                                KERNEL MODE
┌────────────────┐  ②Trap   ┌──────────┐ ③Execute  ┌─────────────┐
│ ① User Process  │ ───────▶ │  System  │ ────────▶ │  Kernel runs │
│   executing      │          │   Call   │           │  the request │
│ ④ Resumes ◀────────────────│ Return   │◀──────────│              │
└────────────────┘            └──────────┘            └─────────────┘
```

| Category | Examples |
|---|---|
| Process Control | fork(), exec(), exit() |
| File Management | open(), read(), write() |
| Device Management | ioctl(), read(), write() |
| Info Maintenance | getpid(), uname() |
| Communication | pipe(), socket() |

**C++ vs Java — making a system call (process creation)**

```cpp
// C++ (POSIX, Linux only) — fork() is a direct system call
#include <iostream>
#include <unistd.h>     // fork()
#include <sys/wait.h>

int main() {
    pid_t pid = fork();          // syscall: creates a child process
    if (pid == 0) {
        std::cout << "Child process, PID=" << getpid() << "\n";
    } else if (pid > 0) {
        wait(NULL);               // parent waits for child
        std::cout << "Parent process, child PID=" << pid << "\n";
    } else {
        std::cerr << "fork failed\n";
    }
    return 0;
}
```

```java
// Java — JVM hides raw system calls; ProcessBuilder spawns an OS process
import java.io.IOException;

public class ProcessDemo {
    public static void main(String[] args) throws IOException, InterruptedException {
        ProcessBuilder pb = new ProcessBuilder("echo", "Hello from child process");
        pb.inheritIO();
        Process child = pb.start();   // internally OS creates a new process
        int exitCode = child.waitFor();
        System.out.println("Child exited with code: " + exitCode);
    }
}
```
> 💡 **Placement Tip:** Interviewers love asking *"What happens when you call `fork()`?"* — Answer: a new process (child) is created as a near-copy of the parent; `fork()` returns 0 in the child and the child's PID in the parent.

### ✅ Quick MCQ Recap
| Q | A |
|---|---|
| OS's primary function | Interface between user & hardware |
| Mode with full hardware access | Kernel Mode |
| Best for industrial robots | Real-Time OS |
| Microkernel vs Monolithic | Microkernel = minimal kernel-mode services |

---

## 2. Process Management

### 📌 Program vs Process
```
PROGRAM (on disk)             PROCESS (in RAM, executing)
┌────────────────┐    run    ┌─────────────────────────────┐
│  a.out / .exe   │ ───────▶ │ Text | Data | Heap | Stack    │
│  (passive code)  │          │ + Registers + PC (active)     │
└────────────────┘            └─────────────────────────────┘
```

| Aspect | Program | Process |
|---|---|---|
| Nature | Passive | Active |
| Stored | Disk | RAM |
| Multiplicity | One program → many processes | Each has own memory/state |

### 🗂️ Process Control Block (PCB)
```
┌───────────────────────────┐
│ Pointer | Process State     │
│ Process ID (PID)            │
│ Program Counter             │
│ CPU Registers                │
│ Memory Limits                │
│ List of Open Files           │
└───────────────────────────┘
```
PCB = "hospital file" for a process — tracks everything the OS needs to pause/resume it.

### 🔄 Process States
```
        admitted
  ┌───────────────────▶ ┌───────┐  Scheduler Dispatch   ┌─────────┐  Exit  ┌────────────┐
  │       new            │ ready │ ────────────────────▶ │ running │ ─────▶ │ terminated │
  └───────────────────   └───────┘ ◀──────────────────── └─────────┘        └────────────┘
                              ▲     I/O or event completion    │
                              │                                 │ I/O or event wait
                              └──────────────┌─────────┐◀───────┘
                                              │ waiting │
                                              └─────────┘
```

### 🔁 Context Switching
```
Process A running  ──▶  Save A's state into A's PCB
                    ──▶  Load B's state from B's PCB
                    ──▶  Process B running
```
- **Overhead** — no useful work happens during the switch.
- Needed for: multitasking, fairness, responsiveness, handling I/O waits.

### 🧵 Threads — "Lightweight Processes"
```
            PROCESS (shared Code, Data, Files)
        ┌─────────────────────────────────────────┐
        │  Thread 1        Thread 2      Thread 3   │
        │  (own Stack,     (own Stack,    (own Stack,│
        │   PC, Registers)  PC, Registers) PC, Reg.) │
        └─────────────────────────────────────────┘
```

| Aspect | Process | Thread |
|---|---|---|
| Memory | Own address space | Shares with sibling threads |
| Communication | IPC (slow) | Shared memory (fast) |
| Context switch | Slower | Faster |
| Crash impact | Isolated | Can crash whole process |
| Control block | PCB | TCB |

| ULT (User-Level) | KLT (Kernel-Level) |
|---|---|
| Managed by library | Managed by OS |
| Fast context switch | Slow (syscall needed) |
| One block ⇒ all block | Only blocking thread pauses |

**Thread creation — C++ vs Java**

```cpp
// C++ (C++11 std::thread)
#include <iostream>
#include <thread>

void printTask(int id) {
    std::cout << "Thread " << id << " is running\n";
}

int main() {
    std::thread t1(printTask, 1);   // create thread
    std::thread t2(printTask, 2);
    t1.join();                      // wait for thread to finish
    t2.join();
    std::cout << "Main thread done\n";
    return 0;
}
```

```java
// Java — built-in Thread class
public class ThreadDemo {
    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> System.out.println(
            Thread.currentThread().getName() + " is running");

        Thread t1 = new Thread(task, "Thread-1");
        Thread t2 = new Thread(task, "Thread-2");
        t1.start();   // start() launches a NEW thread (don't call run() directly!)
        t2.start();
        t1.join();    // wait for thread to finish
        t2.join();
        System.out.println("Main thread done");
    }
}
```
> 💡 **Interview Trap:** `start()` creates a real OS thread; calling `run()` directly just executes on the **current** thread — no concurrency!

### ⚙️ CPU-bound vs I/O-bound
| Type | Behaviour |
|---|---|
| CPU-bound | Long CPU bursts, few I/O ops, keeps CPU busy |
| I/O-bound | Short CPU bursts, frequent I/O, CPU often idle |

---

## 3. CPU Scheduling

### 📌 Goals
✅ Maximize CPU utilization & throughput
✅ Minimize turnaround, waiting & response time
✅ Fairness — avoid starvation

### 🟢 Preemptive vs 🔴 Non-Preemptive
```
NON-PREEMPTIVE:  P1 ▭▭▭▭▭▭▭▭ (runs to completion / I/O wait)
PREEMPTIVE:      P1 ▭▭▭| P2 cuts in |▭▭ P1 resumes
```

### 📊 Key Terms
```
Arrival(AT) ──▶ [ waiting in queue ] ──▶ Start exec ──▶ [ BURST TIME ] ──▶ Completion(CT)
                |←──────────── Turnaround Time (TAT = CT − AT) ───────────→|
                |←─ Waiting Time (WT = TAT − BT) ─→|
Response Time (RT) = time of FIRST execution − AT
```

### 🧮 Scheduling Queues
```
 Job Queue (disk) ──Long-term──▶ Ready Queue (RAM) ──Short-term──▶ CPU ──▶ Exit
                                       ▲   │
                          Medium-term  │   │ I/O wait
                                       │   ▼
                                 Swapped Queue   Waiting Queue
```

| Scheduler | Job | Frequency |
|---|---|---|
| Long-term | Job → Ready queue | Rare |
| Short-term | Ready → CPU | Every few ms |
| Medium-term | Swap in/out | As needed |

### 🏆 Scheduling Algorithms Compared
| Algorithm | Type | Pros | Cons |
|---|---|---|---|
| FCFS | Non-preemptive | Simple, fair | Convoy effect |
| SJF | Non-preemptive | Min avg waiting time | Starvation, needs burst time |
| SRTF | Preemptive | Min avg turnaround | Frequent switching, starvation |
| Round Robin | Preemptive | Fair time-sharing | High overhead if quantum small |
| Priority | Either | Important tasks first | Starvation (fix: aging) |
| MLFQ | Preemptive | Flexible, reduces starvation | Complex to tune |

### 🔢 Worked Example (FCFS)
```
P1(AT=0,BT=8)  P2(AT=1,BT=4)  P3(AT=2,BT=2)  P4(AT=3,BT=1)

Gantt: |---P1---|--P2--|-P3-|P4|
       0        8      12   14 15
```
| Process | CT | TAT | WT |
|---|---|---|---|
| P1 | 8 | 8 | 0 |
| P2 | 12 | 11 | 7 |
| P3 | 14 | 12 | 10 |
| P4 | 15 | 12 | 11 |

### 💻 Round Robin — C++ vs Java

```cpp
// C++ — Round Robin Scheduling Simulation
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

struct Process { int id, burst, remaining; };

int main() {
    int quantum = 2;
    vector<Process> procs = {{1,5,5}, {2,3,3}, {3,8,8}};
    queue<int> q;
    for (int i = 0; i < procs.size(); i++) q.push(i);

    cout << "Execution order:\n";
    while (!q.empty()) {
        int idx = q.front(); q.pop();
        Process &p = procs[idx];
        int exec = min(quantum, p.remaining);
        cout << "P" << p.id << " runs for " << exec << " units\n";
        p.remaining -= exec;
        if (p.remaining > 0) q.push(idx);   // not finished -> back of queue
    }
    return 0;
}
```

```java
// Java — Round Robin Scheduling Simulation
import java.util.*;

class Process {
    int id, burst, remaining;
    Process(int id, int burst) { this.id = id; this.burst = burst; this.remaining = burst; }
}

public class RoundRobinDemo {
    public static void main(String[] args) {
        int quantum = 2;
        List<Process> procs = new ArrayList<>(List.of(
            new Process(1, 5), new Process(2, 3), new Process(3, 8)));
        Queue<Process> queue = new LinkedList<>(procs);

        System.out.println("Execution order:");
        while (!queue.isEmpty()) {
            Process p = queue.poll();
            int exec = Math.min(quantum, p.remaining);
            System.out.println("P" + p.id + " runs for " + exec + " units");
            p.remaining -= exec;
            if (p.remaining > 0) queue.add(p);   // re-add if not finished
        }
    }
}
```

> 💡 **Placement Tip:** Be ready to manually solve a Gantt chart for FCFS/SJF/SRTF/RR/Priority on paper — very common in interviews & exams.

---

## 4. Process Synchronization

### 📌 Why Synchronize?
When processes/threads share data, a **race condition** can occur — final result depends on unpredictable execution order.

```
Thread A: read x=5 → x=x+1 → write x=6
Thread B:        read x=5 → x=x+1 → write x=6   ❌ Lost update! Expected x=7
```

### 🚧 Critical Section Problem — 3 Conditions
| Condition | Meaning |
|---|---|
| Mutual Exclusion | Only 1 process in critical section at a time |
| Progress | Decision can't be postponed forever |
| Bounded Waiting | Limited turns before a process gets its chance |

### 🔐 Common Solutions
```
       SOFTWARE              HARDWARE             HIGH-LEVEL
  Peterson's / Bakery   Test-and-Set, CAS,    Mutex, Semaphore,
                         Disable Interrupts      Monitor
```

### 🔑 Semaphore Operations
```
wait(S):  while (S <= 0); S = S - 1;   // P operation — acquire
signal(S): S = S + 1;                  // V operation — release
```

**Mutex/Semaphore — C++ vs Java**

```cpp
// C++ — std::mutex protecting a shared counter
#include <iostream>
#include <thread>
#include <mutex>

int counter = 0;
std::mutex mtx;

void increment(int times) {
    for (int i = 0; i < times; i++) {
        mtx.lock();        // wait() — enter critical section
        counter++;
        mtx.unlock();       // signal() — leave critical section
    }
}

int main() {
    std::thread t1(increment, 100000);
    std::thread t2(increment, 100000);
    t1.join();
    t2.join();
    std::cout << "Final counter: " << counter << "\n";  // always 200000
    return 0;
}
```

```java
// Java — synchronized keyword (built-in mutex)
public class CounterDemo {
    private static int counter = 0;

    public static synchronized void increment() {  // critical section
        counter++;
    }

    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> { for (int i = 0; i < 100000; i++) increment(); };
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start(); t2.start();
        t1.join(); t2.join();
        System.out.println("Final counter: " + counter);  // always 200000
    }
}
```

### 🍽️ Classic Synchronization Problems
| Problem | Core Idea | Semaphores Used |
|---|---|---|
| Producer-Consumer | Bounded buffer, wait if full/empty | `mutex`, `empty`, `full` |
| Dining Philosophers | Avoid deadlock for shared forks | `mutex`, `semaphore[i]` |
| Reader-Writer | Many readers OR one writer | `mutex`, `writeLock`, `readCount` |

```
       PRODUCER                BUFFER (size N)              CONSUMER
   ┌───────────────┐      ┌───┬───┬───┬───┬───┐      ┌───────────────┐
   │  produce item  │ ───▶ │ x │ x │   │   │   │ ───▶ │  consume item  │
   └───────────────┘      └───┴───┴───┴───┴───┘      └───────────────┘
       waits if FULL                                 waits if EMPTY
```

**Producer-Consumer — C++ vs Java**

```cpp
// C++ — simplified Producer-Consumer with mutex + condition_variable
#include <iostream>
#include <thread>
#include <queue>
#include <mutex>
#include <condition_variable>

std::queue<int> buffer;
const int MAX = 5;
std::mutex mtx;
std::condition_variable cv;

void producer() {
    for (int i = 1; i <= 10; i++) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return buffer.size() < MAX; });  // wait if full
        buffer.push(i);
        std::cout << "Produced: " << i << "\n";
        cv.notify_all();
    }
}

void consumer() {
    for (int i = 1; i <= 10; i++) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return !buffer.empty(); });       // wait if empty
        int val = buffer.front(); buffer.pop();
        std::cout << "Consumed: " << val << "\n";
        cv.notify_all();
    }
}

int main() {
    std::thread p(producer), c(consumer);
    p.join(); c.join();
    return 0;
}
```

```java
// Java — Producer-Consumer with wait()/notifyAll()
import java.util.LinkedList;

class Buffer {
    private final LinkedList<Integer> queue = new LinkedList<>();
    private final int MAX = 5;

    public synchronized void produce(int value) throws InterruptedException {
        while (queue.size() == MAX) wait();      // wait if full
        queue.add(value);
        System.out.println("Produced: " + value);
        notifyAll();
    }

    public synchronized int consume() throws InterruptedException {
        while (queue.isEmpty()) wait();          // wait if empty
        int val = queue.removeFirst();
        System.out.println("Consumed: " + val);
        notifyAll();
        return val;
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        Buffer buffer = new Buffer();
        Thread producer = new Thread(() -> {
            try { for (int i = 1; i <= 10; i++) buffer.produce(i); }
            catch (InterruptedException e) {}
        });
        Thread consumer = new Thread(() -> {
            try { for (int i = 1; i <= 10; i++) buffer.consume(); }
            catch (InterruptedException e) {}
        });
        producer.start(); consumer.start();
    }
}
```

> 💡 **Placement Tip:** "Difference between mutex and semaphore" is asked everywhere:
> - **Mutex** → lock, binary, ownership (only the locker can unlock)
> - **Semaphore** → counter, can allow N processes, no ownership

---

## 5. Deadlock

### 📌 Definition
A set of processes are **permanently blocked**, each waiting for a resource held by another.

```
   P1 ──holds──▶ R_A          P2 ──holds──▶ R_B
   P1 ──waits──▶ R_B          P2 ──waits──▶ R_A
            ╲                      ╱
             ╲────── DEADLOCK ────╱
```

### ⚠️ 4 Necessary Conditions (must ALL hold)
| Condition | Meaning |
|---|---|
| Mutual Exclusion | Resource held in non-shareable mode |
| Hold and Wait | Holding ≥1 resource, waiting for more |
| No Preemption | Can't forcibly take resources |
| Circular Wait | Processes wait in a cycle |

### 🗺️ Resource Allocation Graph (RAG)
```
Single instance + cycle = DEADLOCK guaranteed

   P1 ──▶ R1                R1 ──▶ P1
    ▲       │                 (assignment edges → )
    │       ▼               (request edges  P → R)
   R2 ◀──── P2
```

### 🛠️ Handling Deadlocks
```
┌──────────────────────────────────────────────────────────┐
│ 1. PREVENTION   — break one of the 4 conditions           │
│ 2. AVOIDANCE    — Banker's Algorithm, check safe state     │
│ 3. DETECTION+RECOVERY — let it happen, detect, kill/rollback│
│ 4. IGNORE (Ostrich Algorithm) — assume it won't happen      │
└──────────────────────────────────────────────────────────┘
```

### 🏦 Banker's Algorithm (core idea)
```
Need[i] = Max[i] - Allocation[i]

Safety Check:
  Work = Available
  for each process Pi (not finished):
      if Need[i] <= Work:
          Work += Allocation[i]
          mark Pi finished
  if all finished → SAFE STATE
  else            → UNSAFE STATE (possible deadlock)
```

**Deadlock simulation — C++ vs Java**

```cpp
// C++ — simple Banker's Algorithm safety check
#include <iostream>
#include <vector>
using namespace std;

bool isSafe(vector<int>& available, vector<vector<int>>& maxNeed,
            vector<vector<int>>& allocation, int n, int m) {
    vector<vector<int>> need(n, vector<int>(m));
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            need[i][j] = maxNeed[i][j] - allocation[i][j];

    vector<bool> finished(n, false);
    vector<int> work = available;
    int count = 0;

    while (count < n) {
        bool found = false;
        for (int i = 0; i < n; i++) {
            if (finished[i]) continue;
            bool canRun = true;
            for (int j = 0; j < m; j++)
                if (need[i][j] > work[j]) { canRun = false; break; }
            if (canRun) {
                for (int j = 0; j < m; j++) work[j] += allocation[i][j];
                finished[i] = true;
                found = true;
                count++;
            }
        }
        if (!found) return false;   // no process could proceed -> unsafe
    }
    return true;
}

int main() {
    vector<int> available = {3, 3, 2};
    vector<vector<int>> maxNeed = {{7,5,3}, {3,2,2}, {9,0,2}};
    vector<vector<int>> allocation = {{0,1,0}, {2,0,0}, {3,0,2}};

    cout << (isSafe(available, maxNeed, allocation, 3, 3)
              ? "System is in SAFE state\n" : "System is in UNSAFE state\n");
    return 0;
}
```

```java
// Java — simple Banker's Algorithm safety check
public class BankersAlgorithm {
    static boolean isSafe(int[] available, int[][] maxNeed, int[][] allocation, int n, int m) {
        int[][] need = new int[n][m];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                need[i][j] = maxNeed[i][j] - allocation[i][j];

        boolean[] finished = new boolean[n];
        int[] work = available.clone();
        int count = 0;

        while (count < n) {
            boolean found = false;
            for (int i = 0; i < n; i++) {
                if (finished[i]) continue;
                boolean canRun = true;
                for (int j = 0; j < m; j++)
                    if (need[i][j] > work[j]) { canRun = false; break; }
                if (canRun) {
                    for (int j = 0; j < m; j++) work[j] += allocation[i][j];
                    finished[i] = true;
                    found = true;
                    count++;
                }
            }
            if (!found) return false;   // unsafe
        }
        return true;
    }

    public static void main(String[] args) {
        int[] available = {3, 3, 2};
        int[][] maxNeed = {{7,5,3}, {3,2,2}, {9,0,2}};
        int[][] allocation = {{0,1,0}, {2,0,0}, {3,0,2}};

        System.out.println(isSafe(available, maxNeed, allocation, 3, 3)
            ? "System is in SAFE state" : "System is in UNSAFE state");
    }
}
```

> 💡 **Placement Tip:** Dining Philosophers + Banker's Algorithm are extremely common DSA/OS interview questions — practice writing the safety-check logic by hand.

---

## 6. Inter-Process Communication (IPC)

### 📌 When Needed
Whenever 2+ processes need to exchange data (same machine or network).

### 🔀 Approaches
```
MESSAGE PASSING                      SHARED MEMORY
┌─────────┐   send   ┌─────────┐    ┌─────────┐  read/write  ┌──────────┐
│ Process1 │ ───────▶ │ Process2 │    │ Process1 │ ◀────────▶ │ SharedMem │
└─────────┘  receive  └─────────┘    └─────────┘              └──────────┘
  (no shared memory)                   (needs sync, but very fast)
```

| Mechanism | Direction | Use case |
|---|---|---|
| Pipe | One-way, related processes | `ls \| grep txt` |
| FIFO (named pipe) | One-way, unrelated processes | Named mailbox |
| Socket | Two-way, any machine | Web servers, chat apps |
| Signal | Async notification | Graceful shutdown |
| Message Queue | FIFO buffered | Task queues, print spoolers |
| Shared Memory | Direct shared region | Real-time video/audio |

**IPC — C++ vs Java**

```cpp
// C++ — Pipe between parent & child process (POSIX)
#include <iostream>
#include <unistd.h>
#include <cstring>

int main() {
    int fd[2];
    pipe(fd);                // fd[0]=read end, fd[1]=write end
    pid_t pid = fork();

    if (pid == 0) {                       // Child: writes to pipe
        close(fd[0]);
        const char* msg = "Hello from child!";
        write(fd[1], msg, strlen(msg) + 1);
        close(fd[1]);
    } else {                              // Parent: reads from pipe
        close(fd[1]);
        char buffer[100];
        read(fd[0], buffer, sizeof(buffer));
        std::cout << "Parent received: " << buffer << "\n";
        close(fd[0]);
    }
    return 0;
}
```

```java
// Java — simple in-memory message queue (simulates IPC between threads)
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class MessageQueueDemo {
    public static void main(String[] args) throws InterruptedException {
        BlockingQueue<String> queue = new LinkedBlockingQueue<>();

        Thread sender = new Thread(() -> {
            try { queue.put("Hello from sender!"); }
            catch (InterruptedException e) {}
        });

        Thread receiver = new Thread(() -> {
            try { System.out.println("Receiver got: " + queue.take()); }
            catch (InterruptedException e) {}
        });

        sender.start();
        receiver.start();
        sender.join();
        receiver.join();
    }
}
```

> 💡 **Placement Tip:** Know the difference — "Pipe = same machine, related processes" vs "Socket = any machine, network-capable."

---

## 7. Memory Management

### 📚 Memory Hierarchy
```
   FAST/EXPENSIVE   ┌────────────────────┐
        ▲           │   CPU Registers     │  Level 0
        │           ├────────────────────┤
        │           │  Cache Memory        │  Level 1
        │           ├────────────────────┤
        │           │  Main Memory (RAM)    │  Level 2
        │           ├────────────────────┤
        │           │  Magnetic Disk         │  Level 3
        │           ├────────────────────┤
        ▼           │  Optical Disk / Tape    │  Level 4/5
   SLOW/CHEAP       └────────────────────┘
```

### 🧭 Logical vs Physical Address
```
CPU ──(logical addr)──▶ [ MMU ] ──(physical addr)──▶ RAM
```
| Feature | Logical | Physical |
|---|---|---|
| Generated by | CPU | MMU |
| Visible to | Program | Hardware only |

### 🗃️ Fragmentation
| Type | Where wasted | Fix |
|---|---|---|
| Internal | Inside allocated block | Paging |
| External | Between allocated blocks | Compaction / Paging |

```
INTERNAL:  [process uses 6KB|██ 2KB wasted██] (8KB block)
EXTERNAL:  [P1][free 2KB][P2][free 3KB][P3][free 1KB]  → can't fit 5KB process!
```

### 🧱 Memory Allocation Techniques
```
                     Memory Management
                   ┌──────────┴──────────┐
              Contiguous              Non-Contiguous
              ┌─────┴─────┐      ┌────┬────┬────┬────┐
           Fixed       Variable Paging Segmentation Inverted Segmented-
         Partition     Partition                     Paging   Paging
```

| Technique | Idea | Pros | Cons |
|---|---|---|---|
| Fixed Partitioning | Equal fixed-size blocks | Simple | Internal fragmentation |
| Variable Partitioning | Size = process size | No internal frag | External fragmentation |
| Paging | Fixed-size pages ↔ frames | No external frag | Page table overhead |
| Segmentation | Logical segments (code/data/stack) | Modular | External fragmentation |
| Segmented Paging | Segments split into pages | Best of both | Complex translation |

### 📍 Paging Address Translation
```
Logical Address: [ Page Number p | Offset d ]
                        │
                        ▼ (lookup in Page Table)
Physical Address: [ Frame Number f | Offset d ]
```

### 🔄 Allocation Strategies
| Strategy | Picks |
|---|---|
| First Fit | First block big enough |
| Next Fit | Continue from last position |
| Best Fit | Smallest sufficient block |
| Worst Fit | Largest block |

### 🌀 Virtual Memory & Demand Paging
```
Process Address Space ◀──swap──▶ Hard Disk (extra "virtual" RAM)
```
- Demand Paging: load a page only on a **Page Fault**.

### 🔁 Page Replacement Algorithms
```
FIFO:    [1][2][3] → need 4 → remove OLDEST (1)
LRU:     1,2,1,3   → remove LEAST RECENTLY USED (2)
Optimal: remove page NOT NEEDED for longest future time (theoretical best)
```

| Algorithm | Removes | Notes |
|---|---|---|
| FIFO | Oldest loaded | Suffers Belady's Anomaly |
| LRU | Least recently used | Good real-world approximation |
| Optimal | Won't be used longest | Best but needs future knowledge |
| Random | Random page | Unpredictable |

**LRU Cache — C++ vs Java** (classic placement coding question!)

```cpp
// C++ — LRU Cache using unordered_map + doubly linked list (std::list)
#include <iostream>
#include <list>
#include <unordered_map>
using namespace std;

class LRUCache {
    int capacity;
    list<pair<int,int>> cacheList;                       // (key, value), front = most recent
    unordered_map<int, list<pair<int,int>>::iterator> map;

public:
    LRUCache(int cap) : capacity(cap) {}

    int get(int key) {
        if (map.find(key) == map.end()) return -1;       // page fault
        auto it = map[key];
        cacheList.splice(cacheList.begin(), cacheList, it); // move to front (most recent)
        return it->second;
    }

    void put(int key, int value) {
        if (map.find(key) != map.end()) {
            cacheList.erase(map[key]);
        } else if (cacheList.size() == capacity) {
            auto last = cacheList.back();                 // remove LRU (back)
            map.erase(last.first);
            cacheList.pop_back();
        }
        cacheList.push_front({key, value});
        map[key] = cacheList.begin();
    }
};

int main() {
    LRUCache cache(2);
    cache.put(1, 10);
    cache.put(2, 20);
    cout << cache.get(1) << "\n";   // 10
    cache.put(3, 30);               // evicts key 2 (least recently used)
    cout << cache.get(2) << "\n";   // -1 (page fault)
    return 0;
}
```

```java
// Java — LRU Cache using LinkedHashMap (built-in access-order tracking)
import java.util.LinkedHashMap;
import java.util.Map;

class LRUCache extends LinkedHashMap<Integer, Integer> {
    private int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true);   // true = access-order (LRU behavior)
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);   // -1 simulates a page fault
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;   // auto-evict least recently used
    }
}

public class LRUCacheDemo {
    public static void main(String[] args) {
        LRUCache cache = new LRUCache(2);
        cache.put(1, 10);
        cache.put(2, 20);
        System.out.println(cache.get(1));   // 10
        cache.put(3, 30);                   // evicts key 2
        System.out.println(cache.get(2));   // -1 (page fault)
    }
}
```

### 🌪️ Thrashing
```
CPU Utilization
     ▲
     │        ╱‾‾‾╲
     │      ╱       ╲
     │    ╱           ╲___ (Thrashing: utilization crashes)
     └──────────────────────▶ Degree of Multiprogramming
```
Cause: too many processes, not enough RAM → excessive page faults → almost no real work done.

### 🧩 Extras
| Term | Meaning |
|---|---|
| TLB | Fast cache for recent page-table lookups |
| Overlay | Load only needed program module (pre-virtual-memory) |
| Compaction | Shift memory blocks together to remove external fragmentation |

> 💡 **Placement Tip:** LRU Cache (LeetCode #146) is asked at almost every product-based company — know both the manual doubly-linked-list version (C++) and the LinkedHashMap shortcut (Java).

---

## 8. File Systems

### 📌 What is a File System?
Organizes how data is **stored & retrieved** on disk (NTFS, ext4, FAT32...).

### 🏷️ File Attributes
`Name | Type | Size | Location | Timestamps | Permissions | Owner`

### 📂 Directory Structures
```
SINGLE-LEVEL          TWO-LEVEL                 TREE                   DAG
┌───────────┐    ┌─────┐ ┌─────┐         ┌────┐                  ┌────┐
│ F1 F2 F3 F4│    │User1│ │User2│         │ D1 │                  │ D1 │
└───────────┘    └─────┘ └─────┘         ├──┬──┤                  ├──┬──┤
 (name clashes)  (no sharing)         D11  D12 D13         shared links allowed
```

### 🔍 File Access Methods
| Method | How | Use case |
|---|---|---|
| Sequential | One record after another | Audio/log files |
| Direct | Jump to any block | Databases |
| Indexed | Index block → data blocks | Random access efficiency |

### 💾 File Allocation Methods
```
CONTIGUOUS:  [■■■■■]                (fast, but external fragmentation)
LINKED:      [■]→[■]→[■]→null        (no frag, but slow direct access)
INDEXED:     Index[5,9,13,18] → data blocks  (fast random access)
```

| Feature | Contiguous | Linked | Indexed |
|---|---|---|---|
| Access | Fast (both) | Slow (direct) | Fast (direct) |
| Fragmentation | External | None | None |
| Growth | Difficult | Easy | Easy |

### 🆓 Free Space Management
| Method | Idea |
|---|---|
| Bitmap | 1 bit per block (1=used, 0=free) |
| Free List | Linked list of free blocks |
| Grouping | Store addresses in groups |
| Counting | Start address + count |

**Simple File Read/Write — C++ vs Java**

```cpp
// C++ — basic file write & read
#include <iostream>
#include <fstream>
#include <string>

int main() {
    std::ofstream outFile("data.txt");          // open for writing
    outFile << "Hello, Operating System!\n";
    outFile.close();

    std::ifstream inFile("data.txt");           // open for reading
    std::string line;
    while (std::getline(inFile, line)) {
        std::cout << "Read: " << line << "\n";
    }
    inFile.close();
    return 0;
}
```

```java
// Java — basic file write & read
import java.io.*;

public class FileDemo {
    public static void main(String[] args) throws IOException {
        try (FileWriter writer = new FileWriter("data.txt")) {
            writer.write("Hello, Operating System!\n");
        }

        try (BufferedReader reader = new BufferedReader(new FileReader("data.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println("Read: " + line);
            }
        }
    }
}
```

> 💡 **Placement Tip:** Know `inode` (UNIX) stores file metadata + block pointers, and that bitmaps are the most common free-space technique asked in vivas.

---

## 9. Disk Management

### 💽 Disk Structure
```
        spindle
           │
   ┌───────┼───────┐   ← Platter (top surface)
   │ track │ track │
   │   ┌───┴───┐    │
   │   │sector │    │
   └───┴───────┴───┘
           │
   actuator arm → read/write head
```

| Term | Meaning |
|---|---|
| Platter | Magnetic disk storing data |
| Track | Concentric circle on platter |
| Sector | Smallest storage unit (512B/4KB) |
| Cylinder | Same-radius tracks across all platters |

### ⏱️ Access Time Formula
```
Access Time = Seek Time + Rotational Latency + Transfer Time
```

### 🗺️ Disk Scheduling Algorithms
```
Head at 50, Requests: 82, 170, 43, 140, 24, 16, 190

FCFS:    50→82→170→43→140→24→16→190        (in arrival order)
SSTF:    50→43→24→16→82→140→170→190        (nearest request first)
SCAN:    50→82→140→170→190→(reverse)→43→24→16  (sweep like elevator)
C-SCAN:  50→82→140→170→190→jump to 0→16→24→43  (one direction + wraparound)
LOOK:    50→82→140→170→190→43→24→16        (like SCAN, no full sweep to disk edge)
C-LOOK:  50→82→140→170→190→jump to 16→24→43    (like C-SCAN, no full sweep)
```

| Algorithm | Idea | Risk |
|---|---|---|
| FCFS | Arrival order | High total seek time |
| SSTF | Nearest request | Starvation of far requests |
| SCAN | Sweep + reverse | Edge requests wait longer |
| C-SCAN | One-direction + jump | Longer travel distance |
| LOOK/C-LOOK | Like SCAN/C-SCAN but no full sweep | Minor directional bias |

**SSTF Simulation — C++ vs Java**

```cpp
// C++ — Shortest Seek Time First (SSTF) simulation
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> requests = {82, 170, 43, 140, 24, 16, 190};
    int head = 50;
    int totalSeek = 0;
    vector<bool> visited(requests.size(), false);

    cout << "Seek order: " << head;
    for (size_t count = 0; count < requests.size(); count++) {
        int closestIdx = -1, minDist = INT_MAX;
        for (size_t i = 0; i < requests.size(); i++) {
            if (!visited[i]) {
                int dist = abs(requests[i] - head);
                if (dist < minDist) { minDist = dist; closestIdx = i; }
            }
        }
        visited[closestIdx] = true;
        totalSeek += minDist;
        head = requests[closestIdx];
        cout << " -> " << head;
    }
    cout << "\nTotal seek distance: " << totalSeek << "\n";
    return 0;
}
```

```java
// Java — Shortest Seek Time First (SSTF) simulation
import java.util.*;

public class SSTFDemo {
    public static void main(String[] args) {
        int[] requests = {82, 170, 43, 140, 24, 16, 190};
        int head = 50;
        int totalSeek = 0;
        boolean[] visited = new boolean[requests.length];

        StringBuilder order = new StringBuilder(String.valueOf(head));
        for (int count = 0; count < requests.length; count++) {
            int closestIdx = -1, minDist = Integer.MAX_VALUE;
            for (int i = 0; i < requests.length; i++) {
                if (!visited[i]) {
                    int dist = Math.abs(requests[i] - head);
                    if (dist < minDist) { minDist = dist; closestIdx = i; }
                }
            }
            visited[closestIdx] = true;
            totalSeek += minDist;
            head = requests[closestIdx];
            order.append(" -> ").append(head);
        }
        System.out.println("Seek order: " + order);
        System.out.println("Total seek distance: " + totalSeek);
    }
}
```

> 💡 **Placement Tip:** Practice drawing the seek-sequence diagram for FCFS/SSTF/SCAN/C-SCAN by hand with a given head position — frequently asked as a written/whiteboard question.

---

## 10. 🎯 Placement Cheat-Sheet

### 🔥 Most-Asked Interview Questions
| # | Question | One-line Answer |
|---|---|---|
| 1 | Process vs Thread? | Process = isolated memory; Thread = shares memory, lighter |
| 2 | What is a Deadlock? | Circular wait for resources — 4 conditions must all hold |
| 3 | Mutex vs Semaphore? | Mutex = lock w/ ownership; Semaphore = counter, no ownership |
| 4 | Paging vs Segmentation? | Paging = fixed-size; Segmentation = logical, variable-size |
| 5 | What causes Thrashing? | Too many processes, too little RAM → excessive page faults |
| 6 | Belady's Anomaly? | More frames can mean MORE page faults (happens in FIFO) |
| 7 | Virtual Memory purpose? | Run programs bigger than physical RAM using disk as extension |
| 8 | Starvation vs Deadlock? | Starvation = indefinite postponement; Deadlock = total block |
| 9 | What is a Race Condition? | Unsynchronized concurrent access → unpredictable results |
| 10 | fork() vs exec()? | fork() clones process; exec() replaces process image |

### 🧩 Quick Formula Box
```
Turnaround Time (TAT) = Completion Time − Arrival Time
Waiting Time (WT)      = TAT − Burst Time
Response Time (RT)     = Time of first execution − Arrival Time
Access Time            = Seek Time + Rotational Latency + Transfer Time
Need (Banker's)         = Max − Allocation
```

### 🧠 Mind-Map Summary
```
                         OPERATING SYSTEM
        ┌─────────────┬────────────┬────────────┬─────────────┐
   Process Mgmt   CPU Scheduling  Memory Mgmt   File System   Disk Mgmt
   (PCB, Threads) (FCFS/SJF/RR)  (Paging/Virt) (Allocation)  (Scheduling)
        │
   Synchronization → Deadlock → IPC
   (Mutex/Sem)      (Banker's)  (Pipes/Sockets)
```

### ✅ Final Checklist Before Interview
- [ ] Can explain process states with the diagram from memory
- [ ] Can compute FCFS/SJF/RR Gantt charts by hand
- [ ] Can explain Producer-Consumer with code
- [ ] Know all 4 deadlock conditions + Banker's Algorithm steps
- [ ] Can explain paging address translation
- [ ] Know LRU Cache implementation (asked as a coding round Q too!)
- [ ] Can compare disk scheduling algorithms with an example

---
*📘 End of Notes — Good luck with your placements! 🚀*
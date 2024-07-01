# Week 1
## What is an operating System?
An operating system is a **special program** that acts as an intermediary between users of the computer and the computer hardware.
### Basic roles
1. Resource allocator and coordinator
	- Controls I/O requests
	- Manage conflicting requests
	- manage interrupts
2. Controls program execution
	1. Storage hierarchy manager
	2. Process manager
3. Limits program execution to ensure security
	- Prevent illegal access to the hardware or improper usage of the hardware

## User mode vs Kernel Mode
Kernel is the heart of the operating system. It operates on the physical space, and has full knowledge of all physical addresses instead of virtual addresses, and has the complete privilege.

User not dangy. Kernel dangy.
### Switching
The only way to go from user mode to kernel mode is via very specific controlled entry points.
1. Hardware interrupt: input from external devices activates the IR line, thus pausing the current execution of user programs
2. Software interrupt: a software generated interrupt that is invoked from the instruction itself

## Kernel Handling Interrupts
### Vectored Interrupt System
The interrupt signal that **INCLUDES** the identity of the device sending the interrupt signal, hence allowing the kernel to know exactly which interrupt service routine to execute.

### Polled interrupt system
Enter the general interrupt polling routine protocol, CPU scan devices to determine who interrupted. No IR line.

Use vectored interrupt if you have many interrupts frequently. Polled if you don't

## Computer system organisation
### Memory Device Hierarchy
The memory device hierarchy in a typical computer system is made of (in order of highest to lowest) registers, caches, main memory, and non-volatile secondary storage (HDD & SDD). With the highest having the most speed and also the most cost.
### Cache performance
Cache effective access time as:
$$
\alpha \tau + ( 1 - \alpha ) \times \epsilon
$$
Where:
	$\alpha$ is cache hit ratio
	$\epsilon$ is miss access time
	$\tau$ is cache hit access time
## Process Management
### Process vs Program
A process is a program in execution.
Kernel is not a process. Neither are I/O handlers because they do not have context like normal processes. But rather just pieces of instructions that handle certain events.


### Timesharing
**Goal**: Provide interactive user experience and fast response times.

Multiprogramming allows for timesharing. Which extends the concept of multiprogramming by allowing multiple users to interact with the system simultaneously.

CPU will run each job for a fixed quanta `t` and switch to another (ready) job once that time slice ends. Users experience a responsive system as the OS ensures that each user program gets regular CPU time.
### Context Switching
Context switch is the routine of **saving** the state of a process, so that it can be **restored** and **resumed** at a later point in time.
### Scheduling
Process manager is part of kernel coed, and part of the scheduler subroutine. 
1. Creation and termination of both user and system processes
2. Pausing and resuming processes in the event of interrupts or system calls
3. Synchronising processes and providing communications between virtual spaces
### Multiprogramming
Goal: Increase CPU utilization and system throughput.

Multiprogramming involves running multiple programs on a single processor system, which is needed for efficiency. **No idling**

CPU will continue to run job as long as job does not `wait` for anything else. If job `wait` for some I/O operation, CPU will switch to READY jobs.

# Week 2
## OS Services
### Applications
### How to access

## System calls
System calls are programming interfaces provided by the OS Kernel for users to access kernel services. Unlike I/O interrupts, system calls are software generated interrupts (trap instruction).

**controlled** entry points to the kernel

A system call does **NOT** generally require a **full** context switch; instead, it is processed in the context of whichever process invoked it.
### Direct vs API
API provides convenient **abstraction**.
### Ways to pass system calls

|                | Pros                                          | Cons                                          |
| -------------- | --------------------------------------------- | --------------------------------------------- |
| Registers      | Simple and fast access                        | There might be more parameters than registers |
| Stack          | Moderately fast access                        | Slower than registers                         |
| Block or table | Good for taking data in a contiguous location | slow                                          |

### Different types of system calls
1. Process Control: end abort load create treminatenateoa etc
2. File manipulation: create delete rename etc
3. Device manipulation: request and release read write repositosnthas oeasotneha etc
4. Information maintenance: get or set time date sysdata process file deviceh atrib
5. Communication: create and delete pipes, packets, transfer status information
6. Protection: network encryption and protocol
#### Blocking vs non blocking
A blocking system call is one that must wait until the action is completed: eg. `read()`
## Programs
### User vs System

| System Programs                                                            | User Programs                               |
| -------------------------------------------------------------------------- | ------------------------------------------- |
| Used for operating computer hardware and very common system-usage purposes | Used to perform specific user-related tasks |
| Typically comes with the OS                                                | Installed according to user requirements    |
| Commonly runs in the background, require minimal to no user interactions   | Requires interactions with users            |

### system programs and their purposes
System programs, also known as system utilities, provide a convenient environment for program development and execution. And they run on user mode.

## Basic principles of OS design
User goals: OS should be convenient to use, easy to learn, reliable, safe, fast
System goals: The system should be easy to design, implement, and maintain

Policy and mechanism separation
1. policy: determines what will be done
2. mechanism: determines how it something is done
Separation is important for flexibility

| Mechanism                  | Policy                   |
| -------------------------- | ------------------------ |
| Context Switching          | scheduling policy        |
| memory management          | memory allocation policy |
| File system operations     | Access control policy    |
| synchronisation primitives | page replacement policy  |
Benefits:
1. Flexibility
2. Modularity
3. Reusability
4. Ease of Updates

# Week 3
## Processes
### Codes, programs, processes
#### difference
### Process control block
Each process metadata is stored by the kernel in a particular data structure called the PCB. A process table is made out of an array of PCBs, where the process table is a data structure maintained by the kernel to facilitate context switching and scheduling.

It contains this info (updated each time a process is interrupted):
1. Process state: any of the state of the process - new ,ready, running, waiting, terminated
2. Program counter: the address of the next instruction
3. CPU registers: the contetns of the registers in the CPU when an interrupt occurs, including all the funny linking stacking pointers.
4. Scheduling information: access prioritiy
5. Memory-management information
6. Accounting info
7. I/O status
### Lifecycle
### Scheduling States
These states are:
1. New
2. Running
3. Waiting
4. Ready 
5. Terminated
## Mode Switch vs context switch
mode switch is changing the privilege of a process
### Transitions

## Concurrency, Parallelism
### Difference

## Process queues and I/O queues
1. Job queue
2. Ready queue
3. device queue
Each queue contains the pointer to the corresponding PCBs that are waiting for the resource.

## Creating new processes
### fork()
create new process
return child pid if parent, if child pid is 0
## Interprocess Communication
### How does it work?
Through 2 different mechanisms:
1. Shared memory
2. Message Passing (e.g sockets)

|                           | Message passing                                                                                         | Shared memory                                                                         |
| ------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Number of syscalls        | require syscall for each message passed through `send()` `receive()` but much quicker if small messages | costly systemcall in the beginning `shmget`, but not afterwards                       |
| number of processes using | one to one                                                                                              | many                                                                                  |
| Usage comparison          | Smaller amounts of data, large data suffers from syscall overhead                                       | Costly if only small amounts of data exchanged. large and frequent data exchange good |
| Synchronisation           | No synchonisation required. Kernel will do it                                                           | Requires synchronisation to prevent race-condition                                    |

#### Socket
Socket is one endpoint of a two-way communication link between two programs running on the network. its identifier is a concatenation between an IP address and a port 

`read`, `write` is blocking by default. if not blocking, call returns immediately with a error code

##### Message Queue vs Socket

|                             | Socket                                                                                                                                                                         | Message Queue                                                                                                                                                                                                                                                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Order Handling              | FIFO is feature<br>Real-time: order of data packets is maintained strictly in the sequence they were sent<br>                                                                  | FIFO main feature<br>Guarantees message order and delivery, including handling retries and ensuring no message lost in event of network failure                                                                                                                                                                               |
| Operational Context         | Direct Communication: both ends must be connected for data to be transmitted<br>No persistence<br>Immediate processing: crucial for applications requiring low latency<br><br> | Async processing: producer can continue to send messages<br>Persistence: Messages can be stored persistently until processed<br>Buffering and throttling: message queues can buffer a large number of messages and throttle processing based on consumer availability and load. Typically larger than what sockets can handle |
| Connection requirement      | require continuous connection.                                                                                                                                                 | no need continuous connection                                                                                                                                                                                                                                                                                                 |
| Decoupling                  | both endpoints to be ready for communication                                                                                                                                   | decouples the producer and consumer, independent operation                                                                                                                                                                                                                                                                    |
| Persistence and reliability | Relies on the connection being active                                                                                                                                          | Provides mechanisms despite failures                                                                                                                                                                                                                                                                                          |
| Use case                    | suitable for real-time bidirectional communication                                                                                                                             | suitable for async, where reliability and order are critical over time                                                                                                                                                                                                                                                        |
| Number of processes         | Typically 2                                                                                                                                                                    | Multiple                                                                                                                                                                                                                                                                                                                      |

## Threads
A process can have multiple threads, it comprises of:
- a thread id
- pc
- register set
- stack
A thread can execute any part of the process code, including parts currently being executed by another thread. Creating new threads requires cheaper resources than creating new processes. Thread context switch is also cheaper, process creation memory and resources is costly. Threads share resources with the process.
### Difference from processes
Threads have concurrency but no protection. No fault isolation.
IPC is expensive, thread communication has low overhead


|                            | Threads                                                                                  | Processes                                                                   |
| -------------------------- | ---------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Protection and concurrency | Concurrency but no protection                                                            | Concurrency and protection                                                  |
| Communication              | Low overhead, same address space                                                         | High overhead                                                               |
| Parallel execution         | depends on the types of threads                                                          | always available on multicore system                                        |
| synchronisation            | careful programming is required                                                          | rely on OS services                                                         |
| overhead cost              | cheap, managed by thread scheduler (user space). Threads live in the same address space. | expensive, managed by kernel scheduler. Need to cross virtual address space |

### User threads vs Kernel Threads

| Kernel-Level Threads                                                            | User Level Threads                                                                                                           |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Known to OS Kernel                                                              | Not known to OS kernel                                                                                                       |
| Runs in kernel mode                                                             | Runs entirely in user mode.                                                                                                  |
| Invoking a function in the API results in trap to kernel                        | Invoking a function in the api results in local function call                                                                |
| scheduled directly by kernel scheduler                                          | managed and scheduled by the run time system (user level library)                                                            |
| one thread control block per thread in the system in a sys wide thread table    |                                                                                                                              |
| one process control block per process in the system in a sys wide process table | the thread scheduler runs in user mode and exists in the thread library linked with the process                              |
| Kernel must manage the scheduling of both threads and processes                 |                                                                                                                              |
| multiple kernel threads can run on different processors                         | multiple user threads on same user process may not run on multiple cores since kernel dk about them. depend on mapping model |
| Take up kernel data structure, specific to the operating system                 | take up thread data struct. more generic and can run on any system                                                           |
| requires more resources to alloc/dealloc                                        | cheaper to allocate/deallocate                                                                                               |
| significant overhead and increase in kernel complexity                          | simple management, no kernel intervention                                                                                    |
| expensive, makes system call                                                    | fast and efficient, simple function call                                                                                     |
| scheduler may be more efficient                                                 | kernel may forcibly make poor desicion                                                                                       |


## Compute speedup in multicore programming
Maximum speed up that we can gain from multicore programming
$$
\frac{1}{\alpha + \frac{1 - \alpha}{N}}
$$
Where:
- $N$ is the CPU cores
- $\alpha$ is the fraction of the program that must be executed serially


# Week 4
Atomic is an operation done in a single instruction step
## Race condition
Indeterminate outcome due to shared memory between two processes/threads
## Critical Section
We define the **regions** in a program whereby atomicity must be guaranteed as the **critical section**.

In the critical section, the async processes may be:
- changing common variables
- updating a shared table
- writing to a common file
There are two main forms of synchronisation:
1. mutual exclusion: no other processes can execute the CS if there is already one process executing it
2. condition synchronisation: synchronise the execution of a process in a cs based on certain conditions

Requirements of a valid CS solution:
1. mutex: no other processes can execute the cs if there is already one process executing
2. progress: if there's no process in the critical section, and some other processes wish to enter, we need to grant this permission, and not postpone the permission indefinitely
3. bounded waiting: if process A has requested to enter the CS, there exists a bound on the number of times other processes are allowed to enter the CS. CS cannot loop forever and exit after a finite number of instructions

General protocol:
1. Request permission to enter the section
2. Execute the critical section when request granted
3. exit the cs solution
4. maybe execute the remainder section
## Peterson's solution
LD and ST must be atomic. 
init two variables `turn` and `flag[2]`
- if `turn == i`, `Pi` is allowed to enter the cs
- if `flag[i] == true`, `Pi` is ready to enter the critical section
Approach:
1. init `flag[i] == false` for all `i`
2. init `turn = i` for arbitrary `i`
Algorithm:
```c
do{
	flag[i] = true;
	turn = j;
	while (flag[j] && turn == j);
	// CS
	flag[i] = false;
	// remainder
}while(true)
```

### Proof of correctness
Show 3 things:
1. Mutual exclusion is preserved
2. Then progress requirement
3. Then bounded-waiting
Process is polite by setting its own flag into true to be ready, but lets the other process go first. i.e. `flag[i] = true; turn = j;`
Mutex:
`flag[j] == false` meaning that the other `Pj` is not ready to enter the CS and also not in CS
Progress:
`flag[j] == true; turn == i` means that `Pj` is about to enter the cs. No process is in the cs, but it is `Pi` turn, so `Pi` enters first ensuring progress
Bounded waiting:
when `Pj` is done, `flag[j] = false` ensuring `Pi` will break out of the `while` line and enter cs in the future. `Pi` will only wait a max of 1 cycle.
## Hardware synchronisation
### How does it work?
Locking the memory bus:
- swap two memory words `compare_and_swap()`
- test og value of word and set its value: `test_and_set()`
## Semaphores as a CS solution
Semaphore is an integer variable is maintained in the kernel, initialised to a certain value that represents the number of available resources for the sharing processes. Accessed through two standard atomic syscall ops: `wait()`/`acquire()` and `release()`/`signal()`

`wait()` reduces semaphore: if neg, then blocks itself. Added to waiting queue associated with that semaphore. Through syscall `block()`
`signal()` when a process is a completed. one process in queue is resumed, by using syscall `wakeup()`

If semaphore is neg, the magnitude is the number of processes in queue.
Semaphore could be a struct with int value and pointer to a list of PCBs.

Busy waiting due to `signal()` and `wait()` is relatively short due to the small CS of `signal()` and `wait()`. better than busy waiting in the CS of the program.
### Semaphores as a mutex
Applied to the MPC problem, `chars` and `space`
shared resources:
```c
char buf[N];
int in = 0; int out = 0;

semaphore chars = 0;
semaphore space = N;
semaphore mutex_p = 1;
semaphore mutex_c = 1;
```
Producer program:
```c
void send (char c){
   wait(space);
   wait(mutex_p);

   buf[in] = c;
   in = (in + 1)%N;

   signal(mutex_p);
   signal(chars);
}
```
consumer program:
```c
char rcv(){
   char c;
   wait(chars);
   wait(mutex_c);

   c = buf[out];
   out = (out+1)%N;

   signal(mutex_c);
   signal(space);
}
```
### Semaphores as a condition synchronisation
Condition variables allow a process or thread to wait for completion of a given **event** on a particular object. Condition variables are and **should** always be implemented with **mutex** locks. When implemented properly, they provide **condition synchronization**.

Always acquire the mutex before the while loop of the condition variable!
Always put the cond_wait in a while loop of the condition variable!

With mutex lock alone, we cannot easily block a process out of its CS based on any **arbitrary condition** even when the mutex is **available**. we need  A condition variable, effectively a **signalling** mechanism under the context of a given mutex lock.

## Java implementations of synchronized methods
Each object in java has a binary lock:
- lock is acquired by invoking a synchronised method
- released by exiting a synchronised method
mutex is guaranteed for this object's methods. only one thread at any time.

# Week 5
## Deadlock
A system always has finite resources to be distributed among a number of processes. Some examples are:
- CPU cores
- I/O devices
- Access to files or memory locations (guarded by locks or semaphores)
A process must request a resource before using it and must release the resource after using it. User managed resources are not protected by the kernel though.

Deadlock is a situation whereby a set of **blocked** processes (none can make **progress**) each holding a resource and waiting to acquire a resource held by another process in the set.

The critical section solutions we learned in the previous chapter _prevents_ race condition, but if not implemented properly it can cause **deadlock**.
### Why is it a problem?
### Necessary but not sufficient conditions
All these conditions must hold simultaneously for deadlock to be possible:
1. Mutual exclusion
	- $\geq 1$ resource must be held in a non-shareable mode
	- If another process requests that resource, the requesting process must be delayed until the resource has been released
2. Hold and wait
	- A process must be holding and waiting to acquire additional resources that are held by other processes
3. No preemption
	- Resources can only be released after that process has completed its task, voluntarily by the process holding it
4. Circular wait
	- There exists a cycle in the resource allocation graph
### Handling Deadlocks
#### Prevention
Ensure that at least 1 necessary condition never happens.
##### Mutex is not preventable
##### Hold and wait prevention:
- Protocol 1: Must have all of its resources at once before beginning execution, otherwise, wait empty handed
- Only can request for new resources when it has none
Disadvantages:
- Starvation: Process with protocol 1 may never start if resources are scarce and there are too many other processes requesting for it
- Low resource utilisation: Likely that resources aren't used simultaneously. Lots of time wasted for requesting and releasing many resources in the middle of execution
##### No preemption prevention (allow preemption)
A process is holding some resources and requests another resource that cannot be immediately allocated to it, then we can do one of 2 protocols:
- Protocol A: All resources the process is currently holding are implicitly released
- Protocol B: Perform some checks if the resources are held by other waiting processes. If held by other waiting processes, preempts the waiting process and give the resource to the requesting process. If neither held by waiting process nor available, requesting process must wait.
##### Circular wait prevention
Impose a total ordering of all resource types, and require that each process requests resources according to that order. But burdens the programmer to ensure order by design
#### Detection
Different from deadlock avoidance, lets the deadlock happen first. Then runs an algorithm to examine the state of system whether a deadlock has occurred from time to time. And then running algorithm to recover from deadlock condition if any.
##### Explain
Same as bankers algorithm just on actual system state
#### Recovery
After detected, we can
1. abort all deadlocked processes
2. abort deadlock processes one at a time, until no more deadlock
2nd method has high overhead. Might also need to decide which order, cost factors, how much rollback, etc
#### Avoidance
Spend some time to check before granting a resource request. Even if request is valid and resources are available.
##### Banker's algorithm
Resource allocation algorithm:
1. Whenever resource request received, run safety algorithm. then grant or reject based on system state. Release resources when process completes
2. Safety algorithm: assume everything safe, allocate memory, run algorithm, see if all processes can finish the task, $O(MN^2)$
#### Difference between avoidance and detection

## Resource allocation graphs
![[Pasted image 20240701181837.png]]
If at least 1 cycle
- If all resources have exactly one instance, then deadlock
- If cycle involves only a set of resource types with single instance, then deadlock
- if cycle involves a set of resource types with multiple instances then maybe deadlock
# Week 6
## File system
2 types of files on UNIX:
1. Directories
2. Regular files
They then have namespaces.

A file system controls how data is **stored** and **retrieved** in a system.


### What is a filesystem?
### What is a filesystem namespace?
A **namespace** is like a directory structure in a filesystem that organizes and manages names to avoid conflicts. `/home/user/documents` is a namespace
#### File format
File format is the actual data structure of a certain file, defining the **syntax** (permitted values, formal structure or grammar) and **semantics** (meaning and interpretation) of data within a file. The extension will indicate the file type.
#### Permission
#### How to use

## Possible operations on a file
- File creation
- File read and write: must first have permission to `open` using `chmod`. 
	1. `open()`
	2. read/write using file pointer to indicate where to read or write, byte by byte
	3. `close()`
- File delete: search directory to find the file, and remove it
- File truncation: keep file attributes but remove its content.
- File reposition: Requires permission to open. Reposition the value of the file pointer to be pointing to another portion of the file. `lseek()`
- Combining the above file operations

## File data vs metadata (attributes)
### File attributes
- Name: human readable symbolic name stored in directory
- Identifier: unique tag, identifies the file within the file system. Non-human-readable
- Type/format: Information for supporting different types of files
- Location: pointer to a device and to the location of the file on that device
- Size: the current size of the file, and possibly the maximum allowed size
- Protection: Access control information determines who can do reading writing, execution
- Time, date, and user identification: this info may be kept for creation, last modification, and last use

## File descriptors and file system data structure
![[Pasted image 20240701205806.png]]
File descriptor table exists per process. Everytime a process `open()` a file, it is associated with a file descriptor, which uniquely identifies an open file in a computer's operating system. And it has a file `pointer` field that is a pointer to the system wide open-file-table

### Mapping between table entries
#### System wide open file table
SWOFT contains a list of opened files and its entries created by `open()`. The fields include:
- `cp`: current pointer offset, pointing to a specific byte in the file
- Access status: such as read, write, append, execute
- Open count: how many fd table entries point to it. can't remove file table entry if ref count is > 0
- Pointer to inode table
#### Inode table
Index node table or FCB: a database of all file attributes and location of file contents. It contains many inodes, each with a unique id to identify diyfferent files.

There are 3 standard fd per process:
1. 0: stdin
2. 1: stdout
3. 2: stderr
`dup()` returns lowest available `fd`, `dup2()` allows us to explicitly set the new `fd`
### Directory structures
**Directories** are the file system cataloguing structure that contains metadata (references) to **organize** files in a **structured name space** (path). In computing, a namespace is a **set of symbols** that are used to organize objects of various kinds, so that these objects may be referred to by **name**.

`x` permission for directory is referred to as the search bit and it stands for permission to enter the directory.

Since a directory is also a file, it also has its own inode numbers, and its contents are other filenames and their inode numbers. One of the most famous directory is the inode 2 which is / (`root`) directory.

When searching for a file, it will start from root node and recursively search for the file name. And then convert the filename to the correct corresponding inode number.

A directory can also contain the name of other directories. We know this as a **subdirectory**.

## Symbolic and hard links
### Hard Links
A hard link is a feature in many file systems that allow multiple filenames to refer to the same physical location on disk. an AKA for an existing file. 

Ref count is count of all hard links

in creating a `subdir`, the ref count is 2 by default, as it will have the `.` which points to the directory itself, and `..` pointing to its parent

You can't use hardlinks to link directories
#### Purpose
Make it easy to 
- traverse the filesystem
- Categorise your stuffs

### Symbolic Links
A symbolic link is a file whose content is a text string that is automatically interpreted and followed by the operating system as a path to another file or directory.  AKA "short cuts" or "soft links". You can use symbolic links to link directories

#### Purpose
- shortcuts

If all hardlinks to a file is removed, the file is removed. And if there still exists a hardlink to a file, the file still exists. Ref count has to be zero. But might not reach zero if there is some self reference

# Process vs Program

| Process                                                                          | Program                                                                     |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| A program in execution; active, dynamic; changes state overtime during execution | Passive, static entity. The code and static data as they exist on the disk. |
## Process Context
![[Pasted image 20240527131306.png]]
The process' context include:
1. .text
2. value of program counter
3. Contents of processor's registers
4. dedicated address space
5. stack (temporary function data)
6. data (allocated memory at compile time)
7. heap (dynamically allocated memory)
The same program can be run `n` times to create `n` processes simultaneously.

### Concurrency and Protection
Each process runs in a different address space and sees itself as running in a virtual machine. Multiple process execution in a single machine is concurrent, managed by the kernel scheduler.

# Process Management
## Process Scheduling States
As a process executes, it changes its scheduling state which reflects the current activity of the process.

These states are (non-exhaustive):
1. New
2. Running
3. Waiting: The process is waiting for some event to occur (such as an I/O completion)
4. Ready: The process is waiting to be assigned to a processor
5. Terminated

![[Pasted image 20240527132034.png]]
# Process Table and Process Control Block
The system-wide process table is a data structure maintained by the Kernel to facilitate context switching and scheduling. A process table is made up of an array of PCBs, containing information of the current processes in the system.

PCBs are updated when a process is interrupted, these pieces are information are:
1. Process state: any of the state of the process - new, ready, running, waiting, terminated
2. Program Counter: the address of the next instruction for this process
3. CPU Registers: the contents of the registers in the CPU when an interrupt occurs
4. Scheduling information: access priority, pointers to scheduling queues, and any other scheduling parameters
5. Memory-management: page tables, MMU-related information, memory limits
6. Accounting: amount of CPU and real time used, time limits, account numbers, pid
7. I/O status: list of I/O devices allocated to the process

## The Linux task_struct
The Linux PCB is created in C using a data structure called `task_struct` 
![[Pasted image 20240527132609.png]]
All active processes are represented using a doubly linked list of task struct, creating the PT. The kernel maintains a current_pointer to the process that's currently running in the CPU
![[Pasted image 20240527132700.png]]
## Context Switching
When context switching, the kernel has to store all of the process states onto its corresponding PCB, and load the new process' information from its PCB before resuming.

## Mode Switch
Mode switch is a when the privilege of a process changes. And simply escalates the privilege from user mode to kernel mode to access kernel services. And is done by either: hardware interrupts, syscalls, excepts and reset. 

# Process Scheduling
- The objective of multiprogramming is to have some process running at all times, to maximise CPU utilisation.
- The objective of time sharing is to switch the CPU among processes so frequently that users can interact with each program while it is running.
In order to meet both objectives, the process scheduler selects an available process for program execution on the CPU.

## Process Scheduling Queues
There are three queues that are maintained by the process scheduler
1. Job Queue (set of all processes in the system)
2. Ready queue (set of processes ready and waiting to be executed)
3. Device queue (set of processes waiting for an I/O device)
Each queue contains the pointer to the corresponding PCBs that are waiting for the resource
![[Pasted image 20240527133924.png]]
This diagram shows a system with one ready queue, and four device queues. 

A common representation of process scheduling is using a queuing diagram as shown below: (rectangles represents queues)
![[Pasted image 20240527134127.png]]
If new -> ready queue. 

Once a process is allocated in the CPU, a few things could happen afterwards:
- If process issues an I/O request, it will be placed into the I/O queue
- If process `forks` (creating a new process), it may wait until the child process finishes
- The process could be forcibly removed from the CPU, and be put back in the ready queue

# Operations on Processes
## Process Creation
We can create new processes using a `fork()` call. Each of these new processes could create more child processes, forming a tree of processes.
![[Pasted image 20240527134525.png]]
Each process is identified by the `pid` and is unique.

## Child Process vs Parent Process
The new process consists of the entire copy of the address space of the original parent process at the point of `fork()`, and inherits the process' state at that point.

Parent and child processes operate in different address space. Since they are different processes, parent and children processes execute concurrently.

Parent waits for children to terminate with `wait()` before removing it from the PCB, but child processes is unable to `wait()` for their parents to terminate.

### How the fork does it work
```C 
#include <sys/wait.h>
#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>

int main(int argc, char const *argv[])
{
   pid_t pid;

   pid = fork();
   printf("pid: %d\n", pid);

   if (pid < 0)
   {
       fprintf(stderr, "Fork has failed. Exiting now");
       return 1; // exit error
   }
   else if (pid == 0)
   {
       execlp("/bin/ls", "ls", NULL);
   }
   else
   {
       wait(NULL);
       printf("Child has exited.\n");
   }
   return 0;
}
```
![[Pasted image 20240527135149.png]]
- Concurrent programs
- Same copy of the `.text` and resources (opened files, etc)
`fork()` then returns 0 in the child process while in the parent process it returns the `pid` of the child. There's no way to know which code is executed first (concurrency).
![[Pasted image 20240527135633.png]]
`execlp` is a systcall that loads a new program onto the child address space, effectively replacing its `.text`, data, and stack content.
`wait` suspends the parents' execution until the child process that is executing has returned.

## Orphaned Processes
If a parent process with live children is terminated, the children processes become orphaned processes:
- Some OSes designed to either abort all its orphaned children 
- Or adopt the orphaned children processes
For example, `init` is a user process like any other processes, and hence it is using virtual memory. `init` is started by the kernel, and is the direct or indirect ancestor of all other processes, and automatically adopts all orphaned processes.

## Zombie Processes
Zombie processes are processes that are already terminated, and memory as well as other resources are freed, but its exit status is not read by their parents, hence its entry (PCB) in the process table remains.

If the parent does not wait for the child, their child process becomes a zombie process. Children processes can terminate themselves after they have finished executing their tasks using `exit(int status)` 
1. The kernel will free the memory and other resources from this process, but not the PCB entry.
2. Parent processes are supposed to call `wait` or `waitpid` to obtain the exit status of a child.
3. Only after the function call, the kernel removes the child PCB entry from the system wide process table.
4. If no function call, a zombie process remains
Having too many zombie processes might result in inability to create new processes in the system, simply because we run out of pid.
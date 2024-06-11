# System Call

- A way to access OS services
- Controlled entry points to kernel
- System calls are the only way for user-mode processes to change into kernel-mode processes via software instructions.
And is supported by both
- Hardware, PC cannot jump to raw ram addresses of MSB 1
- Virtualization
System calls are processed in the context that they are called

## Accessing the System Calls
At the minimum, the entry points into the kernel have to be mapped into the current address space at all times. The virtual address space is usually divided into two parts
- kernel part
- user part
The kernel mapping exists primarily for the kernel related purposes. Processes running in user mode don't have access to the kernel's address space. There is a single mapping for the kernel, e.g: a fixed address

## System Calls via API
An API is an interface that provides a way to interact with the underlying library that makes the system calls, often named the same as the system calls they invoke.

An API specifies:
- A set of functions that are available to an application programmer
- the parameters that are passed to each function
- the return values the programmer can expect

The functions that make up an API invoke the actual system calls on behalf of the programmer.

Benefits:
- Adds layer of abstraction
- Supports program portability

# Parameter Passing
There are three general ways to pass the parameters required for system calls to the OS kernel.

### Registers
Pass parameters to the registers. Kernel examines certain special registers for the information

Pros: Simple and fast access
Cons: There might be more parameters than registers

### Stack
Push parameters to the program stack by process running in user mode, then invoke `syscall`. In kernel mode, pops the arguments from the calling program's stack

### Block or Table
Pass parameters that are stored in a persistent contiguous location in the RAM, and pass the pointer through registers to be read by the system call routine.
![[Pasted image 20240521113223.png]]

## Types of System Calls
1. Process Control: end, abort, load, execute, create, terminate, get set etc
2. File manipulation: create, delete, rename, open, etc
3. Device Manipulation: request and release device, read from, write to, etc
4. Information maintenance: get or set time, data, system data, process, etc
5. Communication: create and delete pipes, send or receive packets, etc
6. Protection: set network encryption, protocol

## Blocking vs Non-Blocking System Call
A blocking system call is one that must wait until the action can be completed.

`read()` is blocking.
- If no input is ready, the calling process will be suspended
	- `yield()` the remaining quanta, and schedule other processes first
- resume execution after some input is ready. depending on implementation,
	- Be scheduled again and retry (round robin)
	- process re-executes `read()` and may `yield()` again
	- Repeat until successful
- Not schedule, use some `wait` status flag to tell the scheduler to not schedule this again unless some input is received
	- `wait` flag cleared by interrupt handler

A non-blocking system call can return almost immediately without waiting for the I/O to complete.

`select()` is non-blocking.

- The `select()` system call can be used to check if there's new data or not
- A blocking system call like `read()` may be used afterwords knowing they will complete immediately

# Process Control
## Process Abort
A running process can either terminate normally or abruptly. In either case, a system call to abort a process is made.

If ended abnormally, this causes an error trap, a `core dump` is sometimes taken and an error message is generated.

## Process Load and Execute
Loading and executing a new process in the system require system calls. It is possible for a process to call upon the execution of another process, such as creating background processes, etc.

## Process Communication
Having created new jobs or processes, we may need to wait for them to finish their execution. 

- `(wait time)`
- `(wait event)`
- `(signal event)`

All thees features to `wait, signal event`, and other means of process communication are done by making system calls since each process is run in isolation by default, operation on virtual addresses
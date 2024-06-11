# System Programs

System programs, AKA system utilities are basic tools used by many users for common low-level activities. These tools are very generic, and can be considered part of the "system". 

The categories are:
- Package Managers: file management and modification
- Status information
- Programming-language support
- Program loading and execution
- Communications
- Background services

## System vs Application (User) Programs

| System Programs                                                            | User Programs                               |
| -------------------------------------------------------------------------- | ------------------------------------------- |
| Used for operating computer hardware and very common system-usage purposes | Used to perform specific user-related tasks |
| Typically comes with the OS                                                | Installed according to user requirements    |
| Commonly runs in the background, require minimal to no user interactions   | Requires interactions with users            |

# OS Design and Implementation

When designing an OS, it might be useful to consider the few standard things listed:

## User and System goals
Start by defining goals:
- User Goals
- System Goals

## Policy and Mechanism Separation

1. Policy: determines what will be done
2. Mechanism: determines how to do something

Usually the mechanism does not update. As it is built to be extremely robust to implement whatever policy change there might be. This separation is important for flexibility. Follow UNIX philosophy:

1. Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".
2. Expect the output of every program to become the input to another, as yet unknown, program. Don't clutter output with extraneous information. Avoid stringently columnar or binary input formats. Don't insist on interactive input.
3. Design and build software, even operating systems, to be tried early, ideally within weeks. Don't hesitate to throw away the clumsy parts and rebuild them.
4. Use tools in preference to unskilled help to lighten a programming task, even if you have to detour to build the tools and expect to throw some of them out after you've finished using them.

### Mechanism
Mechanism are the low-level operations or functions provided by the OS that enable the performance of basic tasks. These are the building blocks used to implement various policies. They define how something is done, such as:

- context switching
- memory management
- file system operations
- synchronisation primitives

### Policy
Policies are high-level strategies or rules that govern the behaviour of the system. They define what should be done under certain conditions and can be modified without changing the underlying mechanisms

- Scheduling Policy
- Memory Allocation Policy
- Access Control Policy
- Page Replacement Policy

# OS Structures

There are a few common OS structures.

## Monolithic Structure

The whole OS is working in kernel space. It can operate with or without dual mode.

### Without Dual Mode
![[Pasted image 20240521121336.png]]
### With Dual Mode
![[Pasted image 20240521121350.png]]
Pros: distinct performance advantage because there is very little overhead in the system call interface or in communication with the kernel
Cons: Difficult to implement and maintain.

## Layered Approach
![[Pasted image 20240521121525.png]]Pros:
- Simple to construct and debug
- Each layer is implemented only with operations provided by lower-level layers
- A layer does not need to know how these operations are implemented; it needs to know only what these operations do
- This abstracts and hides the existence of certain data structures, operations and hardware from higher-level layers

Cons:
- Need to appropriately define the various layers, needs careful planning.
- Tend to be less efficient

## Microkernel

A very small kernel that provides minimal process and memory management. That only does tasks pertaining to:
1. Inter-Process Communication
2. Memory Management
3. Scheduling
![[Pasted image 20240521121945.png]]
Pros: extending the operation system is easy. All new services are added to user space and consequently do not require modification of the kernel.

Cons: suffer in performance increased system-function overhead due to frequent requirement in performing context switching

## Hybrid Approach

They try to combine microkernel and monolithic kernel aspects and benefits.
![[Pasted image 20240521122124.png]]

# POSIX Shared Memory
1. Kernel allocates and establishes a region of memory and return to the caller process.
2. Once shared memory is established, all accesses are treated as routine user memory access for write/read.
## Procedure
1. Reader and writer get shared memory identifier with `shmget`
2. kernel returns `SHM_KEY` if it exists or creates it if it does not
```c
int shmid = shmget(SHM_KEY, 1024, 0666 | IPC_CREAT);
```
`1024` is the size of the shared memory
3. Both reader and writer should attach the shared memory onto its address space
# Message Passing
## Socket
Socket is one of the message passing interfaces. It is one endpoint of a two-way comms link between 2 programs running on the network with the help of the kernel.

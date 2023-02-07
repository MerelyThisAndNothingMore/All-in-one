# CS:APP

## Chapter 1 A Tour of Computer Systems

### 1.1 Information Is Bits + Context

C is the language of choice for system-level programming, C++ and Java are for application-level programs.

### 1.2 Programs Are Translated by Other Programs into Different Forms

The programs that perform the four phases (preprocessor, compiler, assembler, and linker) are known collectively as the compilation system.

### 1.3 It Pays to Understand How Compilation Systems Work

### 1.4 Processors Read and Interpret Instructions Stored in Memory

![](<../../.gitbook/assets/image (4).png>)

This particular picture is modeled after the family of recent Intel systems, but all systems have a similar look and feel.

**Buses**

Running throughout the system is a collection of electrical conduits called buses that carry bytes of information back and forth between the components. Buses are typically designed to transfer fixed-size chunks of bytes known as words.

**I/O Devices**

Input/output (I/O) devices are the system's connection to the external world. Our example system has four I/O devices: a keyboard and mouse for user input, a display for user output, and a disk drive (or simply disk) for long-term storage of data and programs.

Each I/O device is connected to the I/O bus by either a controller or an adapter.

**Main Memory**

The main memory is a temporary storage device that holds both a program and the data it manipulates while the processor is executing the program.

**Processor**

The central processing unit (CPU), or simply processor, is the engine that interprets (or executes) instructions stored in main memory. At its core is a word-size storage device (or register)called the program counter (PC).

#### 1.4.2 Running the hello Program

### 1.5 Caches Matter

A system spends a lot of time moving information from one place to another, so, there is a need to accelerate the run speed of copy operation.

### 1.6 Storage Devices Form a Hierarchy

The main idea of a memory hierarchy is that storage at one level serves as a cache for storage at the next lower level.

### 1.7 The Operating System Manages the Hardware

We can think of the operating system as a layer of software interposed between the application program and the hardware. All attempts by an application program to manipulate the hardware must go through the operating system.

![](<../../.gitbook/assets/image (5).png>)

As this figure suggests, files are abstractions for I/O devices, virtual memory is an abstraction for both the main memory and disk I/O devices, and processes are abstractions for the processor, main memory, and I/O devices.

#### 1.7.1 Processes

A process is the operating system's abstraction for a running program. Multiple processes can run concurrently on the same system, and each process appears to have exclusive use of the hardware.

#### 1.7.2 Threads

Although we normally think of a process as having a single control flow, in modern systems a process can actually consist of multiple execution units, called threads, each running in the context of the process and sharing the same code and global data.

#### 1.7.3 Virtual Memory

Virtual memory is an abstraction that provides each process with the illusion that it has exclusive use of the main memory. Each process has the same uniform view of memory, which is known as its virtual address space.

In Linux, the topmost region of the address space is reserved for code and data in the operating system that is common to all processes. The lower region of the address space holds the code and data defined by the user's process. Note that addresses in the figure increase from the bottom to the top.

#### 1.7.4 Files

A file is a sequence of bytes, nothing more and nothing less.

### 1.8 Systems Communicate with Other Systems Using Networks

In practice, modern systems are often linked to other systems by networks. From the point of view of an individual system, the network can be viewed as just another I/O device.

### 1.9 Important Themes

A system is more than just hardware. It is a collection of intertwined hardware and systems software that must cooperate in order to achieve the ultimate goal of running application programs.

#### 1.9.1 Amdahl's Law

The main idea is that when we speed up one part of a system, the effect on the overall system performance depends on both how significant this part was and how much it sped up.

#### 1.9.2 Concurrency and Parallelism


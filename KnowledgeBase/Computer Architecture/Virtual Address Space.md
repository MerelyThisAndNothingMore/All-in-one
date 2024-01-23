---
tags:
- computerArchitecture 
alias:
 - 虚拟空间地址
---
In Linux, the topmost region of the address space is reserved for code and data in the operating system that is common to all processes. The lower region of the address space holds the code and data defined by the user's process. Note that addresses in the figure increase from the bottom to the top.
![](https://img-blog.csdnimg.cn/img_convert/8c11f8d7391f6829e5de391b4887d4b7.png)
The virtual address space seen by each process consists of a number of well-defined areas, each with a specific purpose.
- Program code and data. Code begins at the same fixed address for all processes, followed by data locations that correspond to global C variables.
- The code and data areas are followed immediately by the run-time heap. Unlike the code and data areas, which are fixed in size once the process begins running, the heap expands and contracts dynamically at run time as a result of calls to C standard library routines such as malloc and free .
- Shared libraries. Near the middle of the address space is an area that holds the code and data for shared libraries such as the C standard library and the math library.
- Stack. At the top of the user's virtual address space is the user stack that the compiler uses to implement function calls. Like the heap, the user stack expands and contracts dynamically during the execution of the program. In particular, each time we call a function, the stack grows. Each time we return from a function, it contracts.
- [[Kernel]] virtual memory. The top region of the address space is reserved for the kernel. Application programs are not allowed to read or write the contents of this area or to directly call functions defined in the kernel code. Instead, they must invoke the kernel to perform these operations.


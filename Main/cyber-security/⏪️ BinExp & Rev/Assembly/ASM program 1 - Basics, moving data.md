Every assembly program is divided into 3 segments:

1. __Data section__: constants and data that doesn't change at runtime.
2. __BSS section__: This place is used to declare uninitialized variables
3. __Text section__: This stores the code the program is going to run, it begins with a global \_start, which tells the kernel where execution begins

Comments are declared with a `#`

A statement is usually setup like this:
**\[label\] mnemonic \[operands\] \[comment\]**

The first part is the name of the instruction, and the second part are the operands.

Now let's see how we move data between _registers_ and _memory_

Here is the assembly code:
```
# moving_immidiate_data: mov immidiate data between registers and memory  
  
.section .data  
  
.section .bss  
   .lcomm buffer 1  
  
.section .text  
   .globl _start  
  
_start: # used for debugging  
   nop  
  
mov_immdiate_data_to_register:  
   movl $100, %eax          # mov 100 into eax  
   movl $0x50, buffer      # mov 0x50 into the buffer  
  
exit:  
   movl $1, %eax           # sys_exit system call  
   movl $0, %ebx           # exit code 0, successful exection  
   int $0x80               # call sys_exit%
```

Let's focus on the last few lines, first we move `1` into `eax` , this specifies the sys_exit call, meaning our program executes normally, so there is no segmentation fault, then we move `0` into `ebx` meaning the program exited successfully, then we call the software interrupt with the instruction `0x80`

In Linux there are two distinct parts of the memory. At the very bottom of the memory in every program execution there is the Kernel Space, which is made up of the Dispatcher and the Vector table.

At the very top of the memory in any program, we have the User Space, made up of the stack, the heap and the text segment. Here is a diagram of it:

![[Drawing 2023-03-05 12.28.59.excalidraw|left]]

When we load the values as demonstrated, and call INT 0x80, the very next instruction's address is in the user space, ASM code section which is your code, is placed into the return address are in the stack. This is critical so that when INT 0x80 does it's work, it can know what instruction to be carried out next.

Modern Linux does not allow access to the Kernel Space.

At the bottom of the memory, where segment 0 offset 0 exists, there is a lookup table with _256_ entries.
Every entry is a __memory address__, including segments and offset portions which compromise of _4 bytes per entry_. As the first 1024 bytes are reserved for this table, and NO OTHER CODE can be manipulated here.
Each address is called an __interrupt vector__, which makes up the interrupt vector table. Every vector has a number 0 to 255, where vector 0 starts occupying up bytes 0 to 3, number 1, 4-7 etc.

These addresses are __NOT__ permanent memory. What is static is vector 0x80 which points to the service dispatcher which point to linux kernel service routines. 

When the return address is popped off the stack returns to the next instruction, the instruction is called __Instruction Return__ or __IRET__ for short, which completes the execution of program flow.

We can look at the entire table by opening a terminal emulator and typing: `cat /usr/include/asm/unistd_32.h`

These are the first few entries:

```
#ifndef _ASM_UNISTD_32_H  
#define _ASM_UNISTD_32_H  
  
#define __NR_restart_syscall 0  
#define __NR_exit 1  
#define __NR_fork 2  
#define __NR_read 3  
#define __NR_write 4  
#define __NR_open 5  
#define __NR_close 6  
#define __NR_waitpid 7  
#define __NR_creat 8  
#define __NR_link 9  
#define __NR_unlink 10  
#define __NR_execve 11  
#define __NR_chdir 12  
#define __NR_time 13  
#define __NR_mknod 14  
#define __NR_chmod 15  
#define __NR_lchown 16  
#define __NR_break 17  
#define __NR_oldstat 18  
#define __NR_lseek 19  
#define __NR_getpid 20
```


#assembly
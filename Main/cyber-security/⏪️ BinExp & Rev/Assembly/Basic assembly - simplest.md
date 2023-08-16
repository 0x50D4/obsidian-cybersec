```assembly
.section .data  
  
.section .bss  
  
.section .text  
       .globl _start  
  
_start:  
       nop                     # debugging breakpoint  
  
exit:  
       movl $1, %eax # sys_exit  
       movl $0, %ebx # display 0 if normal status  
       int $0x80         # call sys_exit
```

#assembly 
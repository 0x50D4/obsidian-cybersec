
```
.section .data
    constant:
        .int 10

.section .text
    .globl _start

_start:
    nop

mov_data_between_memory_and_register:
    movl constant, %ecx         # move constant data into ECX

exit:
    movl $1, %eax
    movl $0, %ebx
    int $0x80
```

Here we just move the value 10 into __ECX__

#assembly
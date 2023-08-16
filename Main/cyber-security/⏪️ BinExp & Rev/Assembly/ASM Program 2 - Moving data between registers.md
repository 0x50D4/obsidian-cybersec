
```assembly
.section .data

.section .text
    .globl _start

_start:
    nop

    movl $22, %edx

mov_data_between_registers:
    movl %edx, %eax

exit:
    movl $1, %eax
    movl $0, %ebx
    int $0x80
```

```ad-warning
You can only move data between similar, same size registers.
```

Let's debug!

#assembly

```assembly
$SG2989    DB 'hello, world', 0AH, 00H

main       PROC
		   sub   rsp, 40
		   lea   rcx, OFFSET FLAT:$SG2989
		   call  printf
		   xor   eax, eax
		   add   rsp, 40
		   ret   0
main       ENDP
```

In x86-64 all registers are 64 bit now, and they get an `r` prefix. 

In order to use the stack less, we pass the first 4 arguments with registers, and just the rest via stack. In Win64 the four arguments are passed in: `RCX`, `RDX`, `R8` and `R9`. The pointers are now 64 bit so they also require 64 bit registers. For backward compatibility we can still access the lower parts of registers.

![[Drawing 2023-04-08 10.43.44.excalidraw]]

The main function then returns an int, which in c/c++ is still 32 bit for better compability, that's why it uses `eax`.


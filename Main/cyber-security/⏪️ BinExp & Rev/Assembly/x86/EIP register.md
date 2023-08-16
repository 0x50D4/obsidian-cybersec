- AKA.: Instruction Pointer Register
- It keeps track of the next instruction in code to execute
- If you alter the value of it you can make the code do something it's not intended to

Let's get an example:
```c
#include <stdio.h>  
#include <stdlib.h>  
  
void unreachablefunction(void) {  
   printf("how the fu-?\n");  
}  
  
int main(void) {  
   printf("hah, you will never reach me :>\n");  
}
```
We shouldn't be able to reach the `unreachablefunction` manually, by running the code we get the output:
```zsh
❯ ./EIP-example  
hah, you will never reach me :>
```

now, let's open this in __gdb__, put a breakpoint at main and run it

```gdb
(gdb) set disassembly-flavor intel  
(gdb) b main  
Breakpoint 1 at 0x11d1: file EIP.c, line 9.  
(gdb) r  
Starting program: /home/soda/Documents/notes/code/EIP-example
Breakpoint 1, main () at EIP.c:9  
9           printf("hah, you will never reach me :>\n");
```

now, let's disassemble the main function, by typing `disas`
```gdb
(gdb) disas  
Dump of assembler code for function main:  
  0x565561b8 <+0>:     lea    ecx,[esp+0x4]  
  0x565561bc <+4>:     and    esp,0xfffffff0  
  0x565561bf <+7>:     push   DWORD PTR [ecx-0x4]  
  0x565561c2 <+10>:    push   ebp  
  0x565561c3 <+11>:    mov    ebp,esp  
  0x565561c5 <+13>:    push   ebx  
  0x565561c6 <+14>:    push   ecx  
  0x565561c7 <+15>:    call   0x565561f4 <__x86.get_pc_thunk.ax>  
  0x565561cc <+20>:    add    eax,0x2e28  
=> 0x565561d1 <+25>:    sub    esp,0xc  
  0x565561d4 <+28>:    lea    edx,[eax-0x1fdc]  
  0x565561da <+34>:    push   edx  
  0x565561db <+35>:    mov    ebx,eax  
  0x565561dd <+37>:    call   0x56556050 <puts@plt>  
  0x565561e2 <+42>:    add    esp,0x10  
  0x565561e5 <+45>:    mov    eax,0x0  
  0x565561ea <+50>:    lea    esp,[ebp-0x8]  
  0x565561ed <+53>:    pop    ecx  
  0x565561ee <+54>:    pop    ebx  
  0x565561ef <+55>:    pop    ebp  
  0x565561f0 <+56>:    lea    esp,[ecx-0x4]  
  0x565561f3 <+59>:    ret       
End of assembler dump.
```

We can continue with the execution and it exits normally:
```
(gdb) c  
Continuing.  
hah, you will never reach me :>  
[Inferior 1 (process 10195) exited normally]
```

But now let's restart it and look at what the __EIP__ points to when we are at the breakpoint

```gdb
(gdb) x/1xb $eip  
0x565561d1 <main+25>:   0x83  
(gdb) x/1xw $eip  
0x565561d1 <main+25>:   0x8d0cec83
```

we can see that the EIP is pointing to `0x8d0cec83` which is `main+25`

Let's examine the `unreachablefunction` and see where it is in memory
```
(gdb) disas unreachablefunction  
Dump of assembler code for function unreachablefunction:  
  0x5655618d <+0>:     push   ebp  
  0x5655618e <+1>:     mov    ebp,esp  
  0x56556190 <+3>:     push   ebx  
  0x56556191 <+4>:     sub    esp,0x4  
  0x56556194 <+7>:     call   0x565561f4 <__x86.get_pc_thunk.ax>  
  0x56556199 <+12>:    add    eax,0x2e5b  
  0x5655619e <+17>:    sub    esp,0xc  
  0x565561a1 <+20>:    lea    edx,[eax-0x1fec]  
  0x565561a7 <+26>:    push   edx  
  0x565561a8 <+27>:    mov    ebx,eax  
  0x565561aa <+29>:    call   0x56556050 <puts@plt>  
  0x565561af <+34>:    add    esp,0x10  
  0x565561b2 <+37>:    nop  
  0x565561b3 <+38>:    mov    ebx,DWORD PTR [ebp-0x4]  
  0x565561b6 <+41>:    leave     
  0x565561b7 <+42>:    ret       
End of assembler dump.
```

Let's note down the address, `0x5655618d`
Now let's set EIP to that adress
```gdb
(gdb) set $eip = 0x5655618d  
(gdb) x/1xw $eip  
0x5655618d <unreachablefunction>:       0x53e58955
```
Now that we are sure that it's at the right place, we can continue with the execution of the program
```gdb
(gdb) c  
Continuing.  
how the fu-?  
  
Program received signal SIGSEGV, Segmentation fault.  
0xffffcd40 in ?? ()
```
aaaand it worked, we got to the unreachable function :D

#x86
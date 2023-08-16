lets create a simple c program, the simplest we can think of.

```c
int main(void) {  
       return 0;  
}
```

now let's compile it with:
```bash
gcc simplest.c -m32 -ggdb -o simplest
```

Then dump the main functions code with objdump:
```bash
objdump -d simplest -M intel | grep main.: -A11
```

This is the output:

```
0000117d <main>:  
   117d:       55                      push   ebp  
   117e:       89 e5                   mov    ebp,esp  
   1180:       e8 0c 00 00 00          call   1191 <__x86.get_pc_thunk.ax>  
   1185:       05 6f 2e 00 00          add    eax,0x2e6f  
   118a:       b8 00 00 00 00          mov    eax,0x0  
   118f:       5d                      pop    ebp  
   1190:       c3                      ret  
```

Let's look at the line with address *1185*, we can see the instruction: `add eax, 0x2e6f`, which corresponds to  _05 6f 2e 00 00_ , we can see that the order of bytes is changed, this is because of little endian byte order.

Let's use AT&T syntax now:

```
0000117d <main>:  
   117d:       55                      push   %ebp  
   117e:       89 e5                   mov    %esp,%ebp  
   1180:       e8 0c 00 00 00          call   1191 <__x86.get_pc_thunk.ax>  
   1185:       05 6f 2e 00 00          add    $0x2e6f,%eax  
   118a:       b8 00 00 00 00          mov    $0x0,%eax  
   118f:       5d                      pop    %ebp  
   1190:       c3                      ret
```

Now let's use gdb to look at the program:

If we break at main, and use disassamble we see this: 
```
(gdb) disas  
Dump of assembler code for function main:  
   0x5655617d <+0>:     push   %ebp  
   0x5655617e <+1>:     mov    %esp,%ebp  
   0x56556180 <+3>:     call   0x56556191 <__x86.get_pc_thunk.ax>  
   0x56556185 <+8>:     add    $0x2e6f,%eax  
=> 0x5655618a <+13>:    mov    $0x0,%eax  
   0x5655618f <+18>:    pop    %ebp  
   0x56556190 <+19>:    ret  
End of assembler dump.
```

We can use both syntax, the main difference is:

AT&T Syntax : **movl %esp, %ebp** This means move __esp__ into __ebp__.

Intel Syntax : **mov esp, ebp** This means move __ebp__ into __esp__.

#assembly
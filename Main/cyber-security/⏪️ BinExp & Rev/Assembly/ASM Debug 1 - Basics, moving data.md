Load program into gdb
```bash
gdb -q moving_immidiate_data
```

Let's first set a breakpoint at start:
```gdb
(gdb) b _start
```

let's run the program with
```gdb
(gdb) r
```

Then disassemble with
```gdb
(gdb) disas
```

We get the output:
```gdb
Dump of assembler code for function _start:  
=> 0x08049000 <+0>:     nop  
End of assembler dump.
```

We coded a nop instruction, otherwise known as a __0x90__ instruction from and _OPCODE_ perspective, this is hit by the debugger.

```ad-note
This is good practice for assembly programs
```

Now let's use the command `si` to step into the next instruction

```gdb
(gdb) si  
0x08049001 in mov_immdiate_data_to_register ()
```

Now let's `disas` again:

```gdb
(gdb) disas  
Dump of assembler code for function mov_immdiate_data_to_register:  
=> 0x08049001 <+0>:     mov    $0x64,%eax  
   0x08049006 <+5>:     movl   $0x50,0x804a000  
End of assembler dump.
```

What we can see here at the start of \_start+0 is that we are moving `0x64` (100 decimal) into __eax__.

We step again, then use the command `i r` to see what register contains what value like so:
```gdb
(gdb) si  
0x08049006 in mov_immdiate_data_to_register ()  
(gdb) i r  
eax            0x64                100  
ecx            0x0                 0  
edx            0x0                 0  
ebx            0x0                 0  
esp            0xffffcd90          0xffffcd90  
ebp            0x0                 0x0  
esi            0x0                 0  
edi            0x0                 0  
eip            0x8049006           0x8049006 <mov_immdiate_data_to_register+5>  
eflags         0x202               [ IF ]  
cs             0x23                35  
ss             0x2b                43  
ds             0x2b                43  
es             0x2b                43  
fs             0x0                 0  
gs             0x0                 0
```

After we step into again, we can see that we moved `0x50` into buffer.

We can check this data by typing `print` as shown below:
```gdb
(gdb) si  
0x08049010 in exit ()  
(gdb) disas  
Dump of assembler code for function exit:  
=> 0x08049010 <+0>:     mov    $0x1,%eax  
  0x08049015 <+5>:     mov    $0x0,%ebx  
  0x0804901a <+10>:    int    $0x80  
End of assembler dump.
(gdb) print /x buffer
$1 = 0x50
(gdb) x/xb 0x804a000  
0x804a000 <buffer>:     0x50
```


#assembly
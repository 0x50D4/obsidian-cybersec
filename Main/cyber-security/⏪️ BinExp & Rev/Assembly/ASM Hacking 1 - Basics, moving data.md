Let's load the assembly again, set a break point at `_start` and run it.
```gdb
(gdb) break _start  
Breakpoint 1 at 0x8049000  
(gdb) r
```
Then let's step into once, then once more, then check if 0x64 has been moved into eax.

```gdb
(gdb) si  
0x08049001 in mov_immdiate_data_to_register ()  
(gdb) disas  
Dump of assembler code for function mov_immdiate_data_to_register:  
=> 0x08049001 <+0>:     mov    $0x64,%eax  
   0x08049006 <+5>:     movl   $0x50,0x804a000  
End of assembler dump.
```

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
(gdb)
```

We can see that __EAX__ has the value of 0x64, now let's set into something else, like 0x66 like so:

```gdb
(gdb) set $eax = 0x66  
(gdb) i r  
eax            0x66                102  
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

Boom, you've changed it :D

#assembly
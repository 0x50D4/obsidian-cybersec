
Let's fire up GDB, break on \_main and disas:

```
pwndbg> disassemble  
Dump of assembler code for function _start:  
=> 0x08049000 <+0>:     nop  
  0x08049001 <+1>:     mov    edx,0x16
```

Now let's `si` twice then view the registers with `i r`

```
eax            0x0                 0  
ecx            0x0                 0  
edx            0x16                22  
ebx            0x0                 0  
esp            0xffffcda0          0xffffcda0  
ebp            0x0                 0x0  
esi            0x0                 0  
edi            0x0                 0  
eip            0x8049006           0x8049006 <mov_data_between_registers>  
eflags         0x202               [ IF ]  
cs             0x23                35  
ss             0x2b                43  
ds             0x2b                43  
es             0x2b                43  
fs             0x0                 0  
gs             0x0                 0
```

we can see that 22 is in __edx__, and the next instruction will put that into __eax__ too.

```
eax            0x16                22  
ecx            0x0                 0  
edx            0x16                22  
ebx            0x0                 0  
esp            0xffffcda0          0xffffcda0  
ebp            0x0                 0x0  
esi            0x0                 0  
edi            0x0                 0  
eip            0x8049008           0x8049008 <exit>  
eflags         0x202               [ IF ]  
cs             0x23                35  
ss             0x2b                43  
ds             0x2b                43  
es             0x2b                43  
fs             0x0                 0  
gs             0x0                 0
```

now It's in __eax__ too.

#assembly
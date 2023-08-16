Now let's hack it by changing the register before it gets copied, break at \_start, `si` twice, do an `i r`, then let's use
```
set $edx = 0x19
```
and then do an `si` again followed by an `i r`

The result of the first `i r` is:

```
pwndbg> i r  
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

and after the `si` and the change:

```
pwndbg> i r  
eax            0x19                25  
ecx            0x0                 0  
edx            0x19                25  
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

Both changed to 25!

#assembly 
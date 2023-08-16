Break on start as usual:

Right now the ECX register has no value, let's do a double `si`

ECX now has the value 10, let's change that:

```
pwndbg> set $ecx = 1337  
pwndbg> i r  
eax            0x0                 0  
ecx            0x539               1337  
edx            0x0                 0  
ebx            0x0                 0  
esp            0xffffcdb0          0xffffcdb0  
ebp            0x0                 0x0  
esi            0x0                 0  
edi            0x0                 0  
eip            0x8049007           0x8049007 <exit>  
eflags         0x202               [ IF ]  
cs             0x23                35  
ss             0x2b                43  
ds             0x2b                43  
es             0x2b                43  
fs             0x0                 0  
gs             0x0                 0
```

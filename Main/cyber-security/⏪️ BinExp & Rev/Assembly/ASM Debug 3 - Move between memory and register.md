We run GDB and break on \_start, then run the program, we can see that there is nothing in ECX

```
eax            0x0                 0  
ecx            0x0                 0  
edx            0x0                 0  
ebx            0x0                 0  
esp            0xffffcdb0          0xffffcdb0  
ebp            0x0                 0x0  
esi            0x0                 0  
edi            0x0                 0  
eip            0x8049000           0x8049000 <_start>  
eflags         0x202               [ IF ]  
cs             0x23                35  
ss             0x2b                43  
ds             0x2b                43  
es             0x2b                43  
fs             0x0                 0  
gs             0x0                 0
```

We step into twice and we can see the value change!

```
eax            0x0                 0  
ecx            0xa                 10  
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

#assembly
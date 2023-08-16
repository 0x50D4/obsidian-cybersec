To locate the return address we need we need to understand how the stack frame is created.

Let’s look at this main function disassembled for example:

```
   0x56556239 <+0>:	push   ebp
   0x5655623a <+1>:	mov    ebp,esp
   0x5655623c <+3>:	push   ebx
   0x5655623d <+4>:	call   0x565560d0 <__x86.get_pc_thunk.bx>
   0x56556242 <+9>:	add    ebx,0x2db2
   0x56556248 <+15>:	cmp    DWORD PTR [ebp+0x8],0x1
   0x5655624c <+19>:	jg     0x5655626a <main+49>
   0x5655624e <+21>:	mov    eax,DWORD PTR [ebp+0xc]
   0x56556251 <+24>:	mov    eax,DWORD PTR [eax]
   0x56556253 <+26>:	push   eax
   0x56556254 <+27>:	lea    eax,[ebx-0x1fdb]
   0x5655625a <+33>:	push   eax
   0x5655625b <+34>:	call   0x56556060 <printf@plt>
   0x56556260 <+39>:	add    esp,0x8
   0x56556263 <+42>:	push   0x0
   0x56556265 <+44>:	call   0x56556090 <exit@plt>
   0x5655626a <+49>:	mov    eax,DWORD PTR [ebp+0xc]
   0x5655626d <+52>:	add    eax,0x4
   0x56556270 <+55>:	mov    eax,DWORD PTR [eax]
   0x56556272 <+57>:	push   eax
   0x56556273 <+58>:	call   0x565561cd <check_authentication>
```

first, `eax` is pushed onto stack, that’s our func argument, then we see the call to `0x565561cd`, this also pushes our `instruction pointer` to the stack, that’s our return address. 

```
gef➤  disass check_authentication 
Dump of assembler code for function check_authentication:
   0x565561cd <+0>:	push   ebp
   0x565561ce <+1>:	mov    ebp,esp
   0x565561d0 <+3>:	push   ebx
   0x565561d1 <+4>:	sub    esp,0x14
```

And in the `check_authentication` function, we can see the `base pointer (ebp)` being pushed onto stack. 

For more info:
[[Stack]]
[[The stack]]
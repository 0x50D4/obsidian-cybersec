We can use `objdump` to diasassemble a vulnerable program:

```
objdump -d -j .plt ./fmt_vuln
```

output:
```
Disassembly of section .plt:

00001030 <__libc_start_main@plt-0x10>:
    1030:	ff b3 04 00 00 00    	push   0x4(%ebx)
    1036:	ff a3 08 00 00 00    	jmp    *0x8(%ebx)
    103c:	00 00                	add    %al,(%eax)
	...

00001040 <__libc_start_main@plt>:
    1040:	ff a3 0c 00 00 00    	jmp    *0xc(%ebx)
    1046:	68 00 00 00 00       	push   $0x0
    104b:	e9 e0 ff ff ff       	jmp    1030 <_init+0x30>

00001050 <printf@plt>:
    1050:	ff a3 10 00 00 00    	jmp    *0x10(%ebx)
    1056:	68 08 00 00 00       	push   $0x8
    105b:	e9 d0 ff ff ff       	jmp    1030 <_init+0x30>

00001060 <strcpy@plt>:
    1060:	ff a3 14 00 00 00    	jmp    *0x14(%ebx)
    1066:	68 10 00 00 00       	push   $0x10
    106b:	e9 c0 ff ff ff       	jmp    1030 <_init+0x30>

00001070 <puts@plt>:
    1070:	ff a3 18 00 00 00    	jmp    *0x18(%ebx)
    1076:	68 18 00 00 00       	push   $0x18
    107b:	e9 b0 ff ff ff       	jmp    1030 <_init+0x30>

00001080 <exit@plt>:
    1080:	ff a3 1c 00 00 00    	jmp    *0x1c(%ebx)
    1086:	68 20 00 00 00       	push   $0x20
    108b:	e9 a0 ff ff ff       	jmp    1030 <_init+0x30>

00001090 <putchar@plt>:
    1090:	ff a3 20 00 00 00    	jmp    *0x20(%ebx)
    1096:	68 28 00 00 00       	push   $0x28
    109b:	e9 90 ff ff ff       	jmp    1030 <_init+0x30>
```


Well that’s quite long. What can we gather from this?

```
00001080 <exit@plt>:
    1080:	ff a3 1c 00 00 00    	jmp    *0x1c(%ebx)
    1086:	68 20 00 00 00       	push   $0x20
    108b:	e9 a0 ff ff ff       	jmp    1030 <_init+0x30>
```

Well this looks useful af. 

But sadly this part is read only 😞

BUT GOOD NEWS! These are just pointers to pointers, meaning if we overwrite the pointers they point to we can actually control the flow of execution.

```bash
objdump -R ./fmt_vuln
```

Reveals where the exit is stored:

```
./fmt_vuln:     file format elf32-i386

DYNAMIC RELOCATION RECORDS
OFFSET   TYPE              VALUE
00003ee8 R_386_RELATIVE    *ABS*
00003eec R_386_RELATIVE    *ABS*
00003fec R_386_RELATIVE    *ABS*
0000401c R_386_RELATIVE    *ABS*
00003fe0 R_386_GLOB_DAT    _ITM_deregisterTMCloneTable@Base
00003fe4 R_386_GLOB_DAT    __cxa_finalize@GLIBC_2.1.3
00003fe8 R_386_GLOB_DAT    __gmon_start__@Base
00003ff0 R_386_GLOB_DAT    _ITM_registerTMCloneTable@Base
00004000 R_386_JUMP_SLOT   __libc_start_main@GLIBC_2.34
00004004 R_386_JUMP_SLOT   printf@GLIBC_2.0
00004008 R_386_JUMP_SLOT   strcpy@GLIBC_2.0
0000400c R_386_JUMP_SLOT   puts@GLIBC_2.0
00004010 R_386_JUMP_SLOT   exit@GLIBC_2.0
00004014 R_386_JUMP_SLOT   putchar@GLIBC_2.0

```

So it is `00004010` in this program. We can overwrite it 😀


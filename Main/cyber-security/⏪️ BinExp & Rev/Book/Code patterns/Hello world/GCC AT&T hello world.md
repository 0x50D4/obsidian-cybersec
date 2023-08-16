
AT&T is much more popular in the Linux world. Let's see GCC with AT&T syntax:

```assembly
		.file    "hello_world.c"
		.section .rodata
.LC0
		.string  "hello, world\n"
		.text
		.globl   main
		.type    main, @function 
main:
.LFB0:
		.cfi_startproc
		pushl    %ebp
		.cfi_def_cfa_offset 8
		.cfi_offset 5, -8
		movl     %esp, %ebp
		.cfi_def_cfa_register 5
		andl     $-16, %esp
		subl     $16, %esp
		movl     $.LC0, (%esp)
		call     printf
		movl     $0, eax
		leave
		ret
```

```ad-note
This listing has a lot of macros, (everything beginning with a dot), that's not interesting for us for now, except for the .string, because that encodes a null-terminated character sequence just like a C-string
```

Simplified version:

```assembly
.LC0
		.string  "hello, world\n"

main:


		pushl    %ebp
		movl     %esp, %ebp
		andl     $-16, %esp
		subl     $16, %esp
		movl     $.LC0, (%esp)
		call     printf
		movl     $0, %eax
		leave
		ret
```

Here are some of the major differences between AT&T and Intel syntax:
- Source and destination operands are switched up.
- In AT&T, before registers we need to put a %, and before numbers a $
- A suffix is added to operands to define the operand size: 
	- q - quad (64 bits)
	- l - long (32 bits)
	- w - word (16 bits)
	- b - byte (8 bits)

Going back to the compiled result, it's almost identical to the one in IDA, there is one small difference, `0FFFFFFF0h` is presented as $-16, it means the same thing: `16` in the decimal system is `0x10` in hexadecimal so `-0x10` is equal to `0xFFFFFFF0` (for 32 bit.)


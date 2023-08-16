# GDB(gef)
## Break
You can put breaks into the code, so that when it runs, it stops at that point. You can do this serveral ways.
Put break at the main function or other functions:
```gdb
break main
```
```gdb
b *puts
```
Break at main+25:
```gdb
b *main+25
```
Break at a specific memory address:
```
b *0x08048414
```

To show info about breakpoints: 
```
info breakpoints
info b
i b
```
To delete a breakpoint:
```
delete BREAKPOINT_NUMBER
del BREAKPOINT_NUMBER
d BREAKPOINT_NUMBER
```
## Examine
`x/[number][type]` (cab be used standalone too)
**Number**: the number of bytes to print
**Type**: 
- `x/a` address
- `x/c` characters
- `x/s` string
- `x/g` qword
- `x/w` dword
[[Bytes, Doubles, Words etc.]]
## Info
View all the registers contents:
`info registers`/`i r`
View the stack frame:
`info frame`/`i f`

More at: [[General-purpose-registers]], [[Reserved registers]]
## Disassemble
Disassemble a function
`disas main`

[Understand the disassembled code](Assembly.md)

## Replace
**Can't replace with longer data!**
Examples.
1. Change a string`set {char [LENGTH]} MEMORY_ADDRESS = STRING` `set {char [12]} 0x80484b0 = "hello venus"`
2. Change the value stored at a memory address: `set *MEMORY_ADDRESS = VALUE` `set *0x08048451 = 0xfacade`

## Jump
Jump to an instruction, and skip all instructions in between.
`jump ADRESS`/`j ADRESS`

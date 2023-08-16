```
.text:00000000 main  
.text:00000000 10 40 2D E9   STMFD SP!, {R4,LR}  
.text:00000004 1E 0E 8F E2   ADR   R0, aHelloWorld      ; "hello, world"  
.text:00000008 15 19 00 EB   BL    __2printf  
.text:0000000C 00 00 A0 E3   MOV   R0, #0  
.text:00000010 10 80 BD E8   LDMFD SP!, {R4,PC}  
.text:000001EC 68 65 6C 6C+aHelloWorld DCB "hello, world",0 ; DATA XREF:  
main+4
```

In this example we can see every instruction is __4 bytes__, this is because we compiled in __arm__ mode.

The first instruction `STMFD SP!, {R4,LR}`, works as a x86 `PUSH` instruction, writing the values of two registers on the stack. It's not actually `PUSH` tho, that's only available in thumb mode.

What does it do exactly?
- Decrements stack pointer
- Saves the values of the R4 and LR registers at the address pointed to by the modified SP

STMFD may be used to store a set of registers at the specified memory address.

The `ADR R0, aHelloWorld` instruction adds or substract the value in the PC register, to the offset where the hello world string is located, this is called position independent code. It can be executed anywhere in memory, since it will know where the other memory address is, by relative position.

The `BL __2printf` instruction call the printf() function, here's how it works
- Store the address following the BL instruction in `LR`
- Then pass control to printf by writing it's address into the `PC` register
- When returning printf gives back control to the program in the LR register

This is different from x86, where the return address is stored on the stack.

We can't store a whole 32bit address on the stack since the whole instruction can be 32 bits, so we can only store 24 bits. This means that the last 2 bits of an instruction address can be omitted. 

The `LDMFD SP!, {R4,PC} ` just works like a pop here, getting stuff off of the stack

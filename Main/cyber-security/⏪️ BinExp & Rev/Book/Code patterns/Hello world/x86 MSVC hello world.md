
```assembly
CONST    SEGMENT
$SG4840  DB        'Hello world', 0AH, 00H
CONST    ENDS
PUBLIC   _main
EXTRN    _printf:PROC

_TEXT    SEGMENT
_main    PROC
		 push   ebp
		 mov    ebp, esp
		 push   OFFSET $SG3830
		 call   _printf
		 add    esp, 4
		 xor    eax, eax
		 pop    ebp
		 ret    0
_main    ENDP
_TEXT    ENDS
```

This is intel syntax.

It contains two segments, __CONST__ (for data constants) and __\_TEXT__ for code.

The string doesn't have a name so the compiler gives a name to it: \$SG4840.

We can see 0AH(\\n) and 00H (null byte)

In the code segment, there is one function, main.

After the function prologue we can see a `call` to another function, `printf()`. But before the call, we can see an address of a string getting placed onto the stack with a `PUSH` instruction.

When we return from the function, the pointer to our string hello world is still on the stack, so we add __4__ to the `ESP` register. Why four? Because this is a 32 bit program (x86), meaning we need 4  bytes for an address on the stack, if this was an x64 program we'd need 8 bytes.

After we returned and popped off 4 from the stack, we now have to return 0. This time it is done by using `xor eax, eax` setting eax to zero, but why use this over something like `mov eax, 0`? Well, simply put it's __shorter__ in opcode, 2 bytes for xor, and 5 for mov.

and then we use `ret` to return to the caller, which in this case is the OS.
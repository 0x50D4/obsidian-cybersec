
Let's compile with GCC and open in IDA

```assembly
main        proc near

var_10      = dword ptr -10h

			push   ebp
			mov    ebp, esp
			and    esp, 0FFFFFFF0h
			sub    esp, 10h
			mov    eax, offset aHelloWorld
			mov    [esp+10h+var_10], eax
			call   _printf
			mov    eax, 0
			leave
			retn
main        endp
```

It's almost the same, the address of the hello world string is loaded into `eax` first, and then saved onto the stack.

We also have `and esp, 0FFFFFFF0h` in the function prologue, this aligns the ESP register value on a 16 byte boundary. This results in all values on the stack being aligned in the same way, this is useful because the cpu works better if all values are aligned to a 4 byte or 16 byte boundary, that's why you usually see memory addresses being dividable with 4.

`sub esp 10h` allocates 16 bytes on the stack, altho we can see later that only 4 are needed. This is because the size of the allocated stack is also aligned on a 16 bytes boundary.

The string address then is directly stored on the stack without the use of the `push` instruction. `var_10` is a local variable and also the argument for printf.

Then we just mov 0 into eax to set the return value to 0.

The `leave` instruction is basically `mov esp, ebp` and `pop ebp` in one instruction.


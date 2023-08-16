
```assembly
.string   "hello, world\n"
main:
		sub   rsp, 8
		mov   edi, OFFSET FLAT:.LC0
		xor   eax, eax
		call  printf
		xor   eax, eax
		add   rsp, 8
		ret
```

Linux also passes function arguments in registers. The first 6 are passed in: `RDI`, `RSI`, `RDX`, `RCX`, `R8` and `R9`

So the pointer to the string is passed into the 32 bit part of the register, why not the 64 bit part?
It's important to keep in mind that if we `mov` into a 64 bit register, it also clears the upper part of the register even if only the lower part is used.

We can see that it does so so it can save some space, moving to the 64 bit takes up 7 bytes in opcodes while moving into 32 bit only takes up 5 bytes. 

We can also see `eax` getting cleared, this is because the number of used vector registers is passed in using the `eax` register. sdfsdfsdf
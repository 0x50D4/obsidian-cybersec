```assembly
f    PROC
	 BX       lr
	 ENDP
```

In ARM the return address is _not_ saved on the stack, rather in the __link register__. So the `BX LR` instruction causes execution to return to the address in the link register, thus, returning execution to the caller.

#reversing
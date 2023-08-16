There are a few differences in ARM compared to x86:

```assembly
f       PROC
		MOV       r0,#0x7b 
		BX        lr
		ENDP
```

ARM uses register `r0` for the return value, and it just copies with the `MOV` instruction __123__ into `r0`, then does the usual return stuff.

#reversing
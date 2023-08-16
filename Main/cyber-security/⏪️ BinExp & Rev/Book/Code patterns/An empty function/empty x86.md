This is what both GCC and MSVC produce on x86:
```assembly
f:
	ret
```

There is only one instruction, RET, that returns execution to the caller.

#reversing
This is what both GCC and MSVC output (with optimization) on x86:
```assembly
f:
	mov eax, 123
	ret
```

There are 2 instructions, one stores the value __123__ in __EAX__, which is used by convention to store the return value, and the other returns to the caller. The caller will use the result from __EAX__.

#reversing
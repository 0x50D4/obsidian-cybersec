When a program is started, a certain continuous memory space is set aside for the program, this is called the __stack__

The [Stack Pointer](cyber-security/Reversing/x86/General-purpose-registers#ESP) contains the top of the stack. The stack pointer contains the smallest address, let's say for example `0x00001000` so that any address _smaller_ than this is considered _garbage_, and any address _bigger_ is considered _valid_.

This address is of course random. Let's see how it looks like on graphics:

![[Drawing 2023-02-26 12.00.57.excalidraw|600]]

This is how it really works, but we will show it as if it's growing upwards.

In the addMe example, below, the Stack Pointer (ESP), when examined, at the breakpoint, lists `0xffffd050` When the program calls the addMe function, it's at `0xffffd030` which is lower in memory. So the stack grows downwards. 

The stack bottom is the largest valid address of the stack, and is located in the larger address area on the top. This can be confusing since the stack bottom is higher in memory. But it's growing downwards.

The stack limit is the smallest valid address of the stack. If the stack pointer gets smaller then this, there is a stack overflow which can corrupt a program to allow the attacker to take control of the system. There are protections built in now against this.

There are 2 operations for the stack, push and pop. You can push one or more registers by setting the stack pointer to a smaller value. This is usually done by subtracting four times the number of registers to be pushed onto the stack, and copying the registers to the stack.

You can pop one or more registers by copying the data from the stack to the registers, then to add a value to the stack pointer. This is usually done by adding four times the number of registers to be popped on the stack.

```c
#include <stdio.h>
#include <stdlib.h>

void unreachableFunction(void) {
	printf("I'm hacked, oh no :( ");
	exit(0);
}

int main(void) {
	printf("Hello world\n");
	return 0;
}
```

Let's look at this example program, there are 2 functions here, 

![[Drawing 2023-02-26 13.27.12.excalidraw]]

A stack frame exists whenever a function has started but not yet completed.

For example, inside the body of `int main(void)`, there is a call to `int addMe(int a, int b)` which takes 2 arguments, a and b. There needs to be assembly language code in `int main(void)` to push the arguments for `int addMe(int a, int b)`. 

Let's look at some code:
```c
#include <stdio.h>

int addMe(int a, int b);

int main(void) {
	int result = addMe(2, 3);

	printf("The result of the add me function is: %d!\n", result);

	return 0;
}

int addMe(int a, int b) {
	return a + b;
}
```

Simply, `int main(void)` calls `int addMe(int a, int b)` first, and will get put on the stack like this: 

![[Drawing 2023-02-26 13.42.37.excalidraw]]

You can see that the stack frame, for `int main(void)` increased in size by including arguments for the add me function. We also reserve space for the return value of int addMe(). 

Once we get the instructions for int addMe we need to push the stack a little bit coz we may need local variables.

![[Drawing 2023-02-26 13.48.09.excalidraw]]

The add me function can access the arguments because the code in int main places the arguments just as addMe expects it.

__FP__ is the frame pointer that points to the location where the stack pointer was, just before int addMe moved the stack pointer for it's own local variables.

While FP stays the same, SP can change freely and can be set back when it's done.

We can use the FP to compute locations in memory for both the arguments and local variables. It's a fixed offset from the FP.

Once it's time to exit, SP is set to FP, and the whole addMe stack poppes off.



#x86 
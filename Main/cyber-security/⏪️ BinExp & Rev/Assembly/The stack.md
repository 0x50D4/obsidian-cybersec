# The stack
2 operations are used:

### Push
Put an item on the top of the stack
### Pop
Remove an element from the top of the stack

Elements on the stack are assigned a stack address. Elements higher on the stack have a lower adress.

![e12122f0a77dbde762ccc471d4ba5951.png](e12122f0a77dbde762ccc471d4ba5951.png)

Stack grows thoward lower memory addresses.

When a function is called they are set up with a stack frame. Ther variables and data from the function is stored in that stackframe.

![96278572a65cd107beb90f6485bc374a.png](96278572a65cd107beb90f6485bc374a.png)

**EBP** aka **base pointer**, bottom element of the current stack frame
**ESP** aka **stack pointer**, the top element of the current stack frame

Let's take a look at an example code:
```c
#include <stdio.h>
#include <stdlib.h>

void func(int x)
{
	int a = 0;
	int b = x;
}

int main()
{
	func(10);
}
```




The first 5 instructions here are the prologue.
1. The value of the arguement
2. return address
3. EBP (base pointer) get's set
4. ESP (stack pointer) inherits the value of EBP
5. ESP is decremented to make place for longer varaibles
the return address is the 4 byte address of the instruction that will be executed after the function ran.
6. The value of the a variable is moved 4 bytes below the base pointer. This is because an INT is 4 bytes.
7. The second variable we need to assign is stored 8 bytes above the base pointer. That's NOT in the stack frame. **PROBLEM:** values can't be directly moved to the stack frame from the arguments because they are not on the frame. This is where [General-purpose-registers](General-purpose-registers.md) come into play. We stored the value in one of them, then move the value from that to the stack.
![fb28af47e7c5ac5b0017648a4d5b8043.png](fb28af47e7c5ac5b0017648a4d5b8043.png)





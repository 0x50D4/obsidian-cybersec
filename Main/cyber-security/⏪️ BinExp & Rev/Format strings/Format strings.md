| Paramter | Input type | Output type                 |
| -------- | ---------- | --------------------------- |
| %d       | Value      | Decimal                     |
| %u       | Value      | Unsigned Decimal            |
| %x       | Value      | Hexadecimal                 |
| %s       | Pointer    | String                      |
| %n       | Pointer    | Num of bytes written so far |

To explain format strings, here is this example code
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    int a = 5;
    int b = 8;
    int count_one; 
    int count_two;

    // example of n format strings
    printf("The number of bytes written up to this point X%n is being stored in count_one, and the number of bytes up to here X%n is being stored in count_two. \n",
    &count_one, &count_two);

    printf("count_one: %d\n", count_one);
    printf("count_two: %d\n", count_two);

    // stack example
    printf("A is %d, and is at %08x. B is %x.\n", a, &a, b);

    return 0;    
}
```

Output:

```
The number of bytes written up to this point X is being stored in count_one, and the number of bytes up to here X is being stored in count_two. 
count_one: 46
count_two: 113
A is 5, and is at ffffcb7c. B is 8.
```

The stack example part is not really relevant to us rn.

```ad-summary
Basically the `%n` parameter, makes it so that when the function reaches it, it counts every single character that came before it in the format string (excluding itself), and then puts thet into the variable provided. In this case `count_one` or `count_two`.
```

>[!important] 
> The %n parameter has __NO__ output.

# In memory

Now let’s look at this line

```c
printf("A is %d, and is at %08x. B is %x.\n", a, &a, b);
```

When the `printf` function is called, the arguments are pushed onto the stack in reverse order.

```ad-note
This happens in every function.
```

So, first the value of `b` is pushed onto the stack, then comes the address of `a`, and then finally the value of `a`.

Here is how the stack will look like:
![[Drawing 2023-07-06 17.46.50.excalidraw|300|left]]

```ad-question
There is a question tho, what happens if there are 3 format strings and two arguments for some reason?
```

Well, the function will just try incrementing the second argument, to try to reach the place of the 3rd argument. 

Luckily, there is a programming mistake that happens, that allows us to control the number of arguments a format function expect.

[[Format string read]]


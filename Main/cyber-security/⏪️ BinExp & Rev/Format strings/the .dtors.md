A binary program compiled with the GNU C compilers create tables called `.dtors` and `.ctors`.

They are made for destructors and constructors. 

A destruction occours when the program exits. So overwriting something there would cause the program to jump somewhere else in memory if we could overwrite it.

```c
#include <stdio.h>
#include <stdlib.h>

static void cleanup(void) __attribute__ ((destructor));

main() {
    printf("Some actions happen in the main function...\n");
    printf("and when the main exits a destructor is called.\n");

    exit(0);
}

void cleanup(void) {
    printf("In cleanup function now...\n");
}
```

This is an example of a code calling a destructor function that you can make.

One more important thing is that in these tables, the array always begins with `0xffffffff`, and always gets terminated with a NULL address `0x00000000`.

We can use the `nm` command to find the cleanup function example, and use `objdump` to examine the memory there.

```bash
nm ./dtors_example
```

And we can find `080491ce t cleanup` or something similar in the output.

We can check memory with:

```bash
objdump -s -j .dtors ./dtors_sample
```

Here is a sample output:

```
./dtors_sample:
 file format elf32-i386
Contents of section .dtors:
080491ce ffffffff e8830408 00000000
 ............
```

Here we can see our address.

This is writable on some systems for some reason.

Since this place is writable we can just write whatever we want there basically. So we can get a root shell if we wanted.
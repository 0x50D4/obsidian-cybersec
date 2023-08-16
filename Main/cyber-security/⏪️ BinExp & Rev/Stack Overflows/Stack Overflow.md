Let’s take our previous code as an example, just with a few things switched up:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int check_authentication(char *password) {
	char password_buffer[16]; // 16 long buffer, this is the issue.
	int auth_flag = 0; // if this flag is 1, or bigger than 0, it's good


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

The only thing I’ve changed, is the place of the `auth_flag` and the `password_buffer`, now they have switched places, in practice this means that we can’t overwrite the `auth_flag` variable now. That’s sad 😕

There is something else we could overwrite tho, and it exists in every single C program. We will overwrite the return address of the function. Since the function has to exit at sometime, it has to know where it exists, this is done by a saved version of the instruction pointer. That is conveniently on the stack we can overwrite. This most commonly results in a crash. But it doesn’t have to.

![[Drawing 2023-07-02 10.09.05.excalidraw]]

Let’s set a few breakpoints to see why we can’t just overwrite the `auth_flag`.

```bash
gef➤  list 1
1	#include <stdio.h>
2	#include <stdlib.h>
3	#include <string.h>
4	
5	int check_authentication(char *password) {
6		char password_buffer[16]; 
7	   int auth_flag = 0;
8		
9	
10		strcpy(password_buffer, password);
gef➤  
11	
12		// set the flag if the password is right.
13		if(strcmp(password_buffer, "brillig") == 0)
14			auth_flag = 1;
15		if(strcmp(password_buffer, "outgrabe") == 0)
16			auth_flag = 1;
17	
18		return auth_flag;
19	}
20	
gef➤  
21	int main(int argc, char *argv[]) {
22		if(argc < 2) {
23			printf("Usage: %s <password>\n", argv[0]);
24			exit(0);
25		}
26		if(check_authentication(argv[1])) {
27			printf("\n-=-=-=-=-=-=-=-=-=-=-=-=-=-\n");
28			printf("      Access Granted.\n");
29			printf("-=-=-=-=-=-=-=-=-=-=-=-=-=-\n");
30		} else {
gef➤  break 26
Breakpoint 1 at 0x126a: file auth_overflow2.c, line 26.
gef➤  break 10
Breakpoint 2 at 0x11e6: file auth_overflow2.c, line 10.
gef➤  break 18
Breakpoint 3 at 0x1231: file auth_overflow2.c, line 18.
```

Let’s look at the stack pointer rn, while we are still in `main()`.

```bash
gef➤  i r esp
esp            0xffffcad4          0xffffcad4
gef➤  x/32xw $esp
0xffffcad4:	0xf7e22e34	0x00000000	0xf7c1f6e9	0x00000002
0xffffcae4:	0xffffcb94	0xffffcba0	0xffffcb00	0xf7e22e34
0xffffcaf4:	0x56556239	0x00000002	0xffffcb94	0xf7e22e34
0xffffcb04:	0xffffcba0	0xf7ffcb80	0x00000000	0x34a6f7ba
0xffffcb14:	0x48de7daa	0x00000000	0x00000000	0x00000000
0xffffcb24:	0xf7ffcb80	0x00000000	0x05d4f000	0xf7ffda20
0xffffcb34:	0xf7c1f676	0xf7e22e34	0xf7c1f7ad	0xf7fc9c48
0xffffcb44:	0x56558eec	0x00000000	0xf7ffd000	0x00000000
```

We can see it is at `0xffffcad4` for example.

Let’s continue.

```bash
gef➤  i r esp
esp            0xffffcab0          0xffffcab0
gef➤  x/32xw $esp
0xffffcab0:	0xffffffff	0xf7c0cdf8	0xf7fc1380	0x00000000
0xffffcac0:	0x00000000	0x56558ff4	0xffffcad8	0x56556278
0xffffcad0:	0xffffce02	0xf7e22e34	0x00000000	0xf7c1f6e9
0xffffcae0:	0x00000002	0xffffcb94	0xffffcba0	0xffffcb00
0xffffcaf0:	0xf7e22e34	0x56556239	0x00000002	0xffffcb94
0xffffcb00:	0xf7e22e34	0xffffcba0	0xf7ffcb80	0x00000000
0xffffcb10:	0x34a6f7ba	0x48de7daa	0x00000000	0x00000000
0xffffcb20:	0x00000000	0xf7ffcb80	0x00000000	0x05d4f000
```

Now we can see it’s at `0xffffcab0`, this is a lower number, confirming that the stack grows **downwards** towards smaller addresses. 

We just made place for the stack frame of `check_authentication()`.

Let’s first see the difference in bytes between these two addresses:

```bash
gef➤  print 0xffffcad4 - 0xffffcab0
$1 = 0x24
```

The difference is 36 bytes this time.

Let’s look at our vars and find them on the stack:

```bash
gef➤  x/s password_buffer
0xffffcab0:	"\377\377\377\377\370\315\300\367\200\023\374", <incomplete sequence \367>
gef➤  x/x &auth_flag
0xffffcac0:	0x00
```

We can see that the `password_buffer` is just random data, since it hasn’t been assigned a value yet.

`auth_flag` is already set.

```ad-note
Understanding stack frames: [[Understanding stack frames]]
```


For tricks using stack overflows: [[Stack Overflow Tricks]]

For basic integer overflow: [[Stack Overflow (INT)]]
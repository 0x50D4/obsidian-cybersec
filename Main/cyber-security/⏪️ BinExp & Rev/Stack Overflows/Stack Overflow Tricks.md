# Return address spam
If we want to overwrite the `return address` without knowing the exact length that’s between the `buffer` and the address, then we should try spamming it in memory like so:
```bash
./overflow `perl -e 'print "\x25\xfe\x43\x54"x10'`
```

# NOP sled
By spamming the value `0x90` (on x86). Why we use this? Let’s say our memory layout is like this:

![[Drawing 2023-07-04 10.29.09.excalidraw|700]]

Now, when our program hits the return address, it jumps back in memory, it hits the `NOP SLED`, and starts skipping until it gets to the shellcode, and then that runs.

```ad-important
On modern computers you can't execute the stack unless it's specified in the compiler settings with: `-z execstack`
```

# Brute force offset

Script to easily do this:

```bash
for i in $(seq 0 30 300)
    do
        echo Trying offset $i
        ./exploit-notesearch.o $i
    done
```

This script does one thing, it tries to get the offset from our variable, so we can set the end point of the return address to that, hitting our [[#NOP sled]].


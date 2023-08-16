In case anyone wants to compile and exploit programs on their own systems, here is how you compile with security checks off:

```
gcc -z execstack -fno-stack-protector -mpreferred-stack-boundary=2 -m32 -g notesearch.c -o notesearch
```

- `-z execstack` enables the execution of the stack 
- `-fno-stack-protector` disables some additional checks. (from what I read this might be off on most distros) 
- `-mpreferred-stack-boundary=2` sets it so the stack boundary is set to `2^2` so 4 bytes. 
- `-m32` 32 bit mode 
- `-g` debugging symbols. (not necessary) It also might be useful to disable ASLR on Linux by doing:

```
sudo bash -c 'echo 0 > /proc/sys/kernel/randomize_va_space'
```

```ad-warning
This does reset after a reboot.
```

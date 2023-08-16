# Info

A set of tools made to make exploit development easier, aimed at CTFs!
There are 2 versions, a python2 and a python3 version!

# Install

```bash
sudo pip install pwn
```

# Usage

Import:

```python
from pwn import *
```

Connecting to a server:

```python
target = remote("github.com", 9999) # IP/DNS and PORT
```

Run a target binary:
```python
target = process("./binary")
```

Attach the GDB debugger to a process, and pass a command:
```python
gdb.attach(target, gdbscript='b *main')
```

Sending the `x` variable to the target:
```python
target.send(x)
```

Sending the `x` variable to the target (with new line):
```python
target.sendline(x)
```

Print a single line of text from target:
```python
print(target.recvline())
```

Print all text  from the target up to the string "out":
```python
print(target.recvuntil("out"))
```




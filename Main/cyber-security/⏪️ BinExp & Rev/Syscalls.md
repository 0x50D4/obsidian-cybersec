Syscalls are a way for your application to interact with the kernel. For more info:

```
man syscall
```


In assembly, we do syscalls with the `SYSCALL` instruction, that has the opcode: `0F 05` 

On boot, the CPU starts at level 0, and eventually you get put into level 3, where you can only access level 0 things using syscalls, where the execution flow is limited, since you jump to a predefined address. This is how a computer prevents u from doing level 0 operations.

The WRMSR (opcode: `0F 30`) is responsible for setting these addresses at boot time. This can only be used in kernel mode, aka level 0.

<iframe src="https://x64.syscall.sh/" height=700 width=800></iframe>

## write

The write syscall is responsible for writing. The `printf` function in C is just a wrapper around `write`. So let’s use this in C:

```c
int main(int argc, char *argv[]) {
  write(1, "test\n", 5);
}
```

1st param: file descriptor
2nd param: pointer to a string
3rd param: length to write
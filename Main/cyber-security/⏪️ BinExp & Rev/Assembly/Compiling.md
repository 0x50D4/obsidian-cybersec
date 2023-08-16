
lets create a simple c program, the simplest we can think of.

```c
int main(void) {  
       return 0;  
}
```

now let's compile it with:
```bash
gcc simplest.c -m32 -ggdb -o simplest
```

We can also convert this C code into assembly:

```bash
gcc simplest.c -m32 -S -O0
```
here, with the `-S` tag we specify it to output assembly with AT&T syntax, and with the `-O0` we specify that it should have no optimization

We can compile assembly easily with this script:
```python
from os import system  
  
file = input("Enter assembly file name to compile and link: ")  
  
first_command = f"as --32 -o {file[:-2]}.o {file}"  
  
second_command = f"ld -m elf_i386 -o {file[:-2]} {file[:-2]}.o"  
  
system(first_command)  
system(second_command)
```

#assembly
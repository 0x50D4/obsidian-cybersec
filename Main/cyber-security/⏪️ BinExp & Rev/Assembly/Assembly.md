# Assembly Instructions

## Instruction format

- `operation arg`
- `operation arg1, arg2`

## Mov instruction

`mov arg1, arg2`
It copies the value of the second arguement into the location of the first one. Example:
![49f79f90959983cae5e79855e88756c7.png](49f79f90959983cae5e79855e88756c7.png)

Here we can see, that if we try to say `mov eax, ebp-0x8` then it copies the address `ebp-0x8` to the register. To copy the value of it, we have to use "\[\]" to show that we want the value that it points to.

For more on general purpose registers and reserved registers: [[General-purpose-registers]], [[Reserved registers]]

## ADD instruction

It takes 2 arguemnts and adds them together, and stores the result in the FIRST argument. `add arg1, arg2`. 

## SUB instruction
The value of the second instruction is substracted from the first one. `sub arg1, arg2` is just basically arg1 - arg2.

## Push / Pop
- Push places it's operand on the top of the stack `push arg` . More specifically, it first decrements the stack pointer (esp), then places the operand in the location that it points to. It decrements the stack pointer by 8 bytes at x64 and 4 bytes on x86 systems.
- Pop, takes a register as it's argument `pop arg`. It moves the top element of the stack, into a register specified by the arg, and then increments the stack pointer.  Popping the top element of the stack.

## LEA instruction (load effective address) 

It places the address, specified by it's second operand, into the register specified by it's first argument. `lea reg, addr`

## EIP address
Always contains the address of the instruction being executed.

## CMP instruction.
It substracts the second argument from the first one (exactly like SUB), but instead of storing it like SUB does in the first argument, it sets a flag in the process, that contains one of 3 arguements. 
1. zero `0`
2. greater than zero `0<`
3. less than zero `0>`
CMP is always followed by a jump instruction.

## JMP instruction
They take an instruction address as their argument. `jmp addr`. It checks the current state of the flag, and if it's zero, then jumps to the specified address, meaning it sets the EIP (instruction pointer) to it's argument. `eip=arg`
TYPES:
- je: jump at equal to
- jne: jump at not equal to
- jg: jump at greater than
- jl: jump at less than

## Call instruction
It calles a function. It can be a user defined or PLT function(eg.: printf, scanf). it takes one argument.
`call <func>`.  
`call <func>` = `push eip` + `jump func`

## leave/ret instruction
It destroys the current stack frame, by setting the stack pointer to the base pointer, and popping the base pointer off the top of the stack. The return instruction always follows the leave instruction. Return instruction pops off the return address and sets the instruction pointer to that address.

## xor instruction
This will perform the binary operation XOR on the two arguments it is given, and stores the result in the first. the `and` and `or` instructions do the same just with their operations.

## Blocky brackets mean dereferencing ([])
It means that it points that it returns the value the pointer points to. It means that you treat the pointer like the value that it points to. For example
```assembly
mov rax, [rdx]
```
Here we moved the value pointed to by `rdx` into `rax`. On the other side:
```assembly
mov [rax], rdx
```
We move the value of rdx into the place in memory pointed to by rax. The actual value of the rax register does NOT change because it's still just the same memory address.

## NOT
Flips all the bits, 0 to 1 and 1 to 0

# Assembly register uses in C
Registers have different sizes:
```text
+-----------------+---------------+---------------+------------+
| 8 Byte Register | Lower 4 Bytes | Lower 2 Bytes | Lower Byte |
+-----------------+---------------+---------------+------------+
|   rbp           |     ebp       |     bp        |     bpl    |
|   rsp           |     esp       |     sp        |     spl    |
|   rip           |     eip       |               |            |
|   rax           |     eax       |     ax        |     al     |
|   rbx           |     ebx       |     bx        |     bl     |
|   rcx           |     ecx       |     cx        |     cl     |
|   rdx           |     edx       |     dx        |     dl     |
|   rsi           |     esi       |     si        |     sil    |
|   rdi           |     edi       |     di        |     dil    |
|   r8            |     r8d       |     r8w       |     r8b    |
|   r9            |     r9d       |     r9w       |     r9b    |
|   r10           |     r10d      |     r10w      |     r10b   |
|   r11           |     r11d      |     r11w      |     r11b   |
|   r12           |     r12d      |     r12w      |     r12b   |
|   r13           |     r13d      |     r13w      |     r13b   |
|   r14           |     r14d      |     r14w      |     r14b   |
|   r15           |     r15d      |     r15w      |     r15b   |
+-----------------+---------------+---------------+------------+

```


## Passed arguements
 In **x64** the first few arguements of a function are passed with these registers:
```text
rdi:    First Argument
rsi:    Second Argument
rdx:    Third Argument
rcx:    Fourth Argument
r8:     Fifth Argument
r9:     Sixth Argument
```
In **x86** args are passed on the stack.
## Return value
**x64** :  rax register
**x86** :  eax register


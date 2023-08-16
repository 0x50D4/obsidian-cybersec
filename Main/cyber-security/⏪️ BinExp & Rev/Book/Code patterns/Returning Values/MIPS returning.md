GCC assembly output shows:
```assembly
j      $31
li     $2,123
```

and IDA does it by pseudo name:

```assembly
jr      $ra
li      $v0, 0x7B
```

The $2 or $v0 register is used to store the functions return value. `li` stands for "Load Immediate" and is MIPS equivalent to MOV.

The other instruction is `j` or `jr`, that returns the execution to the caller. 

Why is it that the position of the `li` instruction and the `j` instruction are swapped? This is due to a feature called "branch delay slot".

We have to remember that the instruction _after_ the jump instruction is executed first, only after that is the jump executed.

#reversing
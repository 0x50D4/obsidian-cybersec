There are 2 naming conventions used in MIPS, by number (from \$0 to \$31) or by pseudo name (\$V0, \$A0, etc.).

GCC assembly lists registers by number:

```assembly
j       $31
nop
```

While IDA does it by pseudo name:

```assembly
j       $ra
nop
```

The first instruction, (_J or JR_), returns the execution to the caller by jumping to the address in the register $31 / $ra.
The second instruction is `nop` which does nothing, we will ignore it for now

#reversing
Flag help to control, check and verify program execution, and are a mechanism to determine weather each operation performed by the processor is successful or not.

There are 3 kinds:
- Status flags
- Control flags 
- System flags

## Status flags:

#### CF: Carry Flag
This flag is set when a math operation on an unsigned integer value generates a carry or borrow for the most significant bit. 

This is an overflow condition.

When set, the remaining data in the register is not the correct answer to the math operation.

#### PF: Parity Flag
Is used to indicate corrupt data as a result of a math operation in a register.

When checked, the PF is set if the total number of 1 bits in the result is even, and is cleared if the total number of 1 bits in the result is odd.

When the parity flag is checked, an application can determine weather the register has been corrupted since the operation.

#### AF: Adjust Flag
Is used in Binary Coded Decimal math operations, and is set if a carry or borrow operation occurs from bit 3 of the register used for calculation.

#### ZF: Zero Flag
The zero flag is set if a result of an operation is zero

#### SF: Sign Flag
It's set to the most significant bit of the result which is the sign bit, it indicates weather the result is positive or negative

#### OF: Overflow Flag
It's used in signed integer arithmetic when a positive value is too big or a negative value is too small to be represented in the register

## Control flags

Control flags are used to control specific behavior in the processor. For example the DF flag is used to handle in which way strings are handled in the processor. __When set__, string instructions automatically automatically __decrement__ memory addresses to get the next byte of the string. __When cleared__, they __increment__ them.

## System flags

Used to control OS level operations which should __NEVER__ be modified by any respective program or application.

### TF: Trap Flag
It's set to enable _single step mode_, when in this mode, the processor performs only one instruction at a time, waiting for the signal for the next instruction, this is essential when debugging.

### IF: Interrupt Enable Flag
It controls how the processor handles signals received from external sources.

### IOPL: I/O Privilige Level Flag
It indicates the I/O privilege level of the currently running task and defines access levels for the I/O address space, which must be less then or equal to the access level required to access the respective address space. If it's not, access to the address space is denied 

### NT: Nested Task Flag
Controls whether the currently running task is linked to the previously executed task and is used for chaining interrupted and called tasks

### RF: Resume Flag
It controls how the processor responds to exceptions when in debugging mode.

### VM: Virtual-8086 Mode Flag
Indicated that the processor is working in Virtual-8086 mode instead of real or protected mode.

### AC: Alignment Check Flag
It is used in conjunction with the AM bit in the CR0 control register to enable alignment checking of memory references.

### VIF: Virtual Interrupt Flag
Replicates the IF flag just in virtual mode.

### VIP: Virtual Interrupt Pending Flag
It is used to indicate that an interrupt is pending, when in virtual mode. 

### ID: Identification Flag
Indicated whether the processor supports the CPUID function.


#x86
# General pupose registers
These registers are used for the storing of data that a compiler might need at any time. They are used on an as needed basis.

32 bit: **EAX, EBX, ECX, EDX, ESI, EDI**

## EAX
- Main register used in arithmatic calculations, aka: accumulator, as it holds the results of arithmetic operations and function return values

## EBX
- The base register, it points to data in the DS segment, used to store  the base address of a program
#notfinished 

## ECX
- The counter register, often used to hold the value of how many times something has to be  repeated, often used in loops and string operations

## EDX
- General purpose register. Additionally used for I/O operations. In addition it will extend EAX to 64 bit.

## ESI
- Source index register, pointer to data in the segment pointed to by the DS register. Used as an offset address in string and array operations. It holds the address from where to read data.

## EDI
- Destination Index register, pointer to data (or destination) in the segment pointed to by the ES register. Used as an offset address in string and array operations. It holds the implied write address of all string operations.
#notfinished

## EBP
- Base pointer, pointer to data on the stack (SS segment), points to the bottom of the current stack frame, used to reference local variables

## ESP
- Stack pointer (in the SS segment), points to the top of the stack frame, it is used to reference local variables



Each of the above registers are 32 bit in length so 4 bytes. Each of the lower 2 bytes of the __EAX, EBX, ECX, EDX__ registers can be referenced by _AX_, and then subdivided by the names _AH, BH, CH, DH_ for the __high bytes__ and for the __low bytes__, _AL, BL, CL, DL_

In addition, __ESI, EDI, ESP, EBP__, can be referenced by their 16 bit equivalent which is _SI, DI, BP, SP_.

Explanation:
![[registers-in-memory-32.excalidraw|5600]]

__EAX__ would have __AX__ as it's 16-bit segment, that you can further cut up into _AL_ for the lower 8 bits, and _AH_ for the higher 8 bits. Same holds true for EBX, ECX, EDX.
For example for __EBX__, it would be __BX__ for the 16 bit segment, and _BH_ and _BL_ for the 8 bit segments.

![[registers-in-memory-16.excalidraw|7000]]

ESI, would have SI for it's 16 bit segment,
EDI: DI
EBP: BP
ESP: SP

#x86
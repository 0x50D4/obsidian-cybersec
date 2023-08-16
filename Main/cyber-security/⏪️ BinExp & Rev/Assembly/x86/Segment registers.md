User for referencing memory locations

## CS : Code Segment
- Stores the base location of the code section (.text section) which is used for data access
- The processor knows what code to execute based on the CS value and an offset in the EIP register
- No program can explicitly load or change the CS register
## DS : Data Segment
- Stores the default location for variables (.data section), used for data access
## ES : Extra Segment
- Used during string operations
## SS : Stack Segment
- Stores the base location of the stack segment, used when implicitly using the stack pointer, or explicitly the base pointer
## FS : Extra segment register
## GS : Extra segment register

Each segment register is 16 bits and stores the adress to the start of the specific memory segment

#x86
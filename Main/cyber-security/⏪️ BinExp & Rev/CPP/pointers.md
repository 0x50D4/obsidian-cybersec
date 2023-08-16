## The address of operator `&`

We can return the addresses of any variable by using `&`

```cpp
#include <iostream>

int main() {

	int x { 12 };

	std::cout << &x << "\n";  

	return 0;
}
```

This code for example can return something like:
```shell
❯ ./a.out  
0x7fff8d81a1b4
```

## So what is a pointer?
A pointer is a an object holding a memory address, (usually of another variable), this allows us to store the address of some object to use it later.

```cpp
int; // a normal int
int&; // an lvalue reference to and int value
int*; // a pointer to and int value (holds the address of an integer value)
```

## Creating pointer variables
```cpp
int main()
{
    int x { 5 };    // normal variable
    int& ref { x }; // a reference to an integer (bound to x)

    int* ptr;       // a pointer to an integer

    return 0;
}
```

They need to be initailized, otherwise they will hold garbage
```cpp
int main()
{
    int x{ 5 };

    int* ptr;        // an uninitialized pointer (holds a garbage address)
    int* ptr2{};     // a null pointer (we'll discuss these in the next lesson)
    int* ptr3{ &x }; // a pointer initialized with the address of variable x

    return 0;
}
```


## So how do we use this weirdo?

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    std::cout << x << '\n'; // print the value of variable x

    int* ptr{ &x }; // ptr holds the address of x
    std::cout << *ptr << '\n'; // use dereference operator to print the value at the address that ptr is holding (which is x's address)

    return 0;
}
```

so yeah, simple as that... to put this into graphics:
![[Drawing 2023-02-18 17.36.18.excalidraw|500]]

And where does the name come from?
We say `ptr` is holding the address of `x`, so `ptr` is pointing to `x`

## Changing where it points

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    int* ptr{ &x }; // ptr initialized to point at x

    std::cout << *ptr << '\n'; // print the value at the address being pointed to (x's address)

    int y{ 6 };
    ptr = &y; // // change ptr to point at y

    std::cout << *ptr << '\n'; // print the value at the address being pointed to (y's address)

    return 0;
}
```

This returns:
```
5
6
```

## Changing the value being pointed at

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    int* ptr{ &x }; // initialize ptr with address of variable x

    std::cout << x << '\n';    // print x's value
    std::cout << *ptr << '\n'; // print the value at the address that ptr is holding (x's address)

    *ptr = 6; // The object at the address held by ptr (x) assigned value 6 (note that ptr is dereferenced here)

    std::cout << x << '\n';
    std::cout << *ptr << '\n'; // print the value at the address that ptr is holding (x's address)

    return 0;
}
```

The output is: 
```
5
5
6
6
```
because we changed the value.

## Similarities with reference
```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    int& ref { x };  // get a reference to x
    int* ptr { &x }; // get a pointer to x

    std::cout << x;
    std::cout << ref;  // use the reference to print x's value (5)
    std::cout << *ptr << '\n'; // use the pointer to print x's value (5)

    ref = 6; // use the reference to change the value of x
    std::cout << x;
    std::cout << ref;  // use the reference to print x's value (6)
    std::cout << *ptr << '\n'; // use the pointer to print x's value (6)

    *ptr = 7; // use the pointer to change the value of x
    std::cout << x;
    std::cout << ref;  // use the reference to print x's value (7)
    std::cout << *ptr << '\n'; // use the pointer to print x's value (7)

    return 0;
}
```

## So what is the difference?

- We need to worry about addresses with pointers
- References must be initialized, pointers don't have to be (but should be)
- References can't be changed where they point, pointers can
- References must point to something, objects can be bound to nothing
- References are "safe" pointers are dangerous and scawy

#cpp
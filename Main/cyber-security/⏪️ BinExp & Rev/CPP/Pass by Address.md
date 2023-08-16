There are 3 ways we can pass a value to a function, we can pass it by value, by reference and with a pointer, here are the 3 ways:
```cpp
#include <iostream>
#include <string>

void printByValue(std::string val) // The function parameter is a copy of str
{
    std::cout << val << '\n'; // print the value via the copy
}

void printByReference(const std::string& ref) // The function parameter is a reference that binds to str
{
    std::cout << ref << '\n'; // print the value via the reference
}

void printByAddress(const std::string* ptr) // The function parameter is a pointer that holds the address of str
{
    std::cout << *ptr << '\n'; // print the value via the dereferenced pointer
}

int main()
{
    std::string str{ "Hello, world!" };

    printByValue(str); // pass str by value, makes a copy of str
    printByReference(str); // pass str by reference, does not make a copy of str
    printByAddress(&str); // pass str by address, does not make a copy of str

    return 0;
}
```

## Basic checking for null pointers

```cpp
#include <iostream>

void print(int* ptr)
{
    if (ptr) // if ptr is not a null pointer
    {
        std::cout << *ptr;
    }
}

int main()
{
	int x{ 5 };

	print(&x);
	print(nullptr);

	return 0;
}
```

otherwise, we could use the `assert()` command, so if a ptr is passed in debug mode, the debugger complains. We just have to check the pointer with
```cpp
assert(ptr)
```


#cpp
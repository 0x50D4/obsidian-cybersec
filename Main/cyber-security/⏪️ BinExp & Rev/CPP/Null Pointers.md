A null pointer is when a pointer points to no address, so `null`
The easiest way to create a null pointer:
```cpp
int main()
{
    int* ptr {}; // ptr is now a null pointer, and is not holding an address

    return 0;
}
```
Best practice is to always initialize your pointers as null pointers if you don't know the value yet

A null ptr can later be changed

We can also use the keyword and literal `nullpointer` in several contexts, and it's best practice to use it if we initialize a null pointer

```cpp
int main()
{
    int* ptr { nullptr }; // can use nullptr to initialize a pointer to be a null pointer

    int value { 5 };
    int* ptr2 { &value }; // ptr2 is a valid pointer
    ptr2 = nullptr; // Can assign nullptr to make the pointer a null pointer

    someFunction(nullptr); // we can also pass nullptr to a function that has a pointer parameter

    return 0;
}
```

we can easily check if a pointer is not a null pointer by:
```cpp
// Assume ptr is some pointer that may or may not be a null pointer
if (ptr) // if ptr is not a null pointer
    std::cout << *ptr << '\n'; // okay to dereference
else
    // do something else that doesn't involve dereferencing ptr (print an error message, do nothing at all, etc...)
```

In older code, we might use other values to make null pointers, such as 0 or NULL

## Const pointers
Similar to references, we can't set a regular pointer to a constant value, we need a const pointer.

But, we can change it to point at another constant

```cpp
int main()
{
    const int x{ 5 };
    const int* ptr { &x }; // ptr points to const int x

    const int y{ 6 };
    ptr = &y; // okay: ptr now points at const int y

    return 0;
}
```

A constant pointer can point to a non constant value, but can't change that value.

We can also make the pointer itself constant by putting the const after the asterisk like so:
```cpp
int main()
{
    int x{ 5 };
    int* const ptr { &x }; // const after the asterisk means this is a const pointer

    return 0;
}
```
Now the constant pointer can't be changed, it points to an address until it dies.

#cpp
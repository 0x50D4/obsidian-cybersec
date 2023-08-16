A data type where every possible value is defined as a symbolic constant (enumerator)

## Unscoped enumerations

Example:
```cpp
// Define a new unscoped enumeration named Color
enum Color
{
    // Each enumerator is separated by a comma, not a semicolon
    red,
    green,
    blue, // trailing comma optional but recommended
}; 
int main()
{
    Color apple { red };   // my apple is red
    Color shirt { green }; // my shirt is green
    Color cup { blue };    // my cup is blue

    Color socks { white }; // error: white is not an enumerator of Color
    Color hat { 2 };       // error: 2 is not an enumerator of Color

    return 0;
}
```

## Error handling better
We could use numbers for error handling but that's not very descriptive, this is where enums might come into play:
```cpp
enum FileReadResult
{
    readResultSuccess,
    readResultErrorFileOpen,
    readResultErrorFileRead,
    readResultErrorFileParse,
};

FileReadResult readFileContents()
{
    if (!openFile())
        return readResultErrorFileOpen;
    if (!readFile())
        return readResultErrorFileRead;
    if (!parseFile())
        return readResultErrorFileParse;

    return readResultSuccess;
}
```


We can pollute the global namespace by using names such as "blue", because if there are 2 enums with "blue", they could collide and throw out a compile error

## Better solution: namespace
```cpp
namespace color
{
    // The names Color, red, blue, and green are defined inside namespace color
    enum Color
    {
        red,
        green,
        blue,
    };
}

namespace feeling
{
    enum Feeling
    {
        happy,
        tired,
        blue, // feeling::blue doesn't collide with color::blue
    };
}

int main()
{
    color::Color paint { color::blue };
    feeling::Feeling me { feeling::blue };

    return 0;
}
```

Enums automatically get assigned a number, starting from 0, but we can explicitly say what number we want to assign to which enum.

```cpp
enum Animal
{
    cat = -3,
    dog,         // assigned -2
    pig,         // assigned -1
    horse = 5,
    giraffe = 5, // shares same value as horse
    chicken,      // assigned 6
};
```

```ad-warning
Altho you can assign the same value to an enum, it should be avoided, so `giraffe` and `horse` should have different values.
```

```ad-note
If for example you want to cout out an enum, for example I want to say `std::cout << chicken;`, then the compiler is going to convert it to it's value, in this case `6`
```

## Scoped enumerations

Here is an example of a bad, unscoped enumeration:
```cpp
#include <iostream>

int main()
{
    enum Color
    {
        red,
        blue,
    };

    enum Fruit
    {
        banana,
        apple,
    };

    Color color { red };
    Fruit fruit { banana };

    if (color == fruit) // The compiler will compare color and fruit as integers
        std::cout << "color and fruit are equal\n"; // and find they are equal!
    else
        std::cout << "color and fruit are not equal\n";

    return 0;
}
```

We can solve this quite easily with scoped enumerations:
```cpp
#include <iostream>
int main()
{
    enum class Color // "enum class" defines this as a scoped enumeration rather than an unscoped enumeration
    {
        red, // red is considered part of Color's scope region
        blue,
    };

    enum class Fruit
    {
        banana, // banana is considered part of Fruit's scope region
        apple,
    };

    Color color { Color::red }; // note: red is not directly accessible, we have to use Color::red
    Fruit fruit { Fruit::banana }; // note: banana is not directly accessible, we have to use Fruit::banana

    if (color == fruit) // compile error: the compiler doesn't know how to compare different types Color and Fruit
        std::cout << "color and fruit are equal\n";
    else
        std::cout << "color and fruit are not equal\n";

    return 0;
}
```

```ad-warning
These don't convert to integers by themselves!
```

#cpp
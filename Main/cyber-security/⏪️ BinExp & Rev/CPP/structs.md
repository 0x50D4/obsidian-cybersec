## Defining structs

```cpp
struct Employee {
	int id {};
	int age {};
	double wage {};
};
```

## Accessing values of structs

```cpp
#include <iostream>

struct Employee
{
    int id {};
    int age {};
    double wage {};
};

int main()
{
    Employee joe;

    joe.age = 32;  // use member selection operator (.) to select the age member of variable joe

    std::cout << joe.age << '\n'; // print joe's age

    return 0;
}
```

## Aggregate

What is it?
- an aggregate data type is a data type that can contain multiple data members. This is a struct for example
What is it in CPP?
- Is a class type (struct, class, union) or an array
- Has no private or protected non-static data members ??
- Has no user declared, or inherited constructors
- Has no base classes
- Has no virtual member function

## Initialization (3 ways)
```cpp
employee Frank = {1, 43, 8000.0};
employee Frank (1, 43, 8000.0);
employee Frank {1, 43, 8000.0}; // this is prefered
```

```ad-info
Variables of a struct type can be constnats, and they __must__ be initialized
```

## Default member initialization 

```cpp
struct something {
	int x; // not initailized (bad)
	int y {}; // initailized value
	int z { 2 }; // default value
};
```

All possibilities recapped:
```cpp
struct Something
{
    int x;       // no default initialization value (bad)
    int y {};    // value-initialized by default
    int z { 2 }; // explicit default value
};

int main()
{
    Something s1;             // No initializer list: s1.x is uninitialized, s1.y and s1.z use defaults
    Something s2 { 5, 6, 7 }; // Explicit initializers: s2.x, s2.y, and s2.z use explicit values (no default values are used)
    Something s3 {};          // Missing initializers: s3.x is value initialized, s3.y and s3.z use defaults

    return 0;
}
```

## Passing structs by reference

```cpp
#include <iostream>

struct Employee
{
    int id {};
    int age {};
    double wage {};
};

void printEmployee(const Employee& employee) // note pass by reference here
{
    std::cout << "ID:   " << employee.id << '\n';
    std::cout << "Age:  " << employee.age << '\n';
    std::cout << "Wage: " << employee.wage << '\n';
}

int main()
{
    Employee joe { 14, 32, 24.15 };
    Employee frank { 15, 28, 18.27 };

    // Print Joe's information
    printEmployee(joe);

    std::cout << '\n';

    // Print Frank's information
    printEmployee(frank);

    return 0;
}
```

## Returning structs

A common way:
```cpp
#include <iostream>

struct Point3d
{
    double x { 0.0 };
    double y { 0.0 };
    double z { 0.0 };
};

Point3d getZeroPoint() // this is Point3D type, because it returns that!
{
    // We can create a variable and return the variable (we'll improve this below)
    Point3d temp { 0.0, 0.0, 0.0 };
    return temp;
}

int main()
{
    Point3d zero{ getZeroPoint() };

    if (zero.x == 0.0 && zero.y == 0.0 && zero.z == 0.0)
        std::cout << "The point is zero\n";
    else
        std::cout << "The point is not zero\n";

    return 0;
}
```

 This is fine but we can do better then this thing.
 One improvement would be:
```cpp
 Point3d getZeroPoint()
{
    return Point3d { 0.0, 0.0, 0.0 }; // return an unnamed Point3d
}
```

To further improve, we can rely on auto data type detection and do this:
```cpp
Point3d getZeroPoint()
{
    // We already specified the type at the function declaration
    // so we don't need to do so here again
    return { 0.0, 0.0, 0.0 }; // return an unnamed Point3d
}
```

## Structs with program-defined members

There are two types, first where we define one program-defined type (in the global scope) and use it in another like so:

```cpp
#include <iostream>

struct Employee
{
    int id {};
    int age {};
    double wage {};
};

struct Company
{
    int numberOfEmployees {};
    Employee CEO {}; // Employee is a struct within the Company struct
};

int main()
{
    Company myCompany{ 7, { 1, 32, 55000.0 } }; // Nested initialization list to initialize Employee
    std::cout << myCompany.CEO.wage; // print the CEO's wage
}
```

We can also just nest structs to be used inside of other structs, we would use this if that struct only needs to be accessed there

```cpp
#include <iostream>

struct Company
{
    struct Employee // accessed via Company::Employee
    {
        int id{};
        int age{};
        double wage{};
    };

    int numberOfEmployees{};
    Employee CEO{}; // Employee is a struct within the Company struct
};

int main()
{
    Company myCompany{ 7, { 1, 32, 55000.0 } }; // Nested initialization list to initialize Employee
    std::cout << myCompany.CEO.wage; // print the CEO's wage
}
```

#cpp
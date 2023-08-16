## How to  declare them?

```cpp
int // this is a regular int
int& // this is an int reference
double& // this is a double reference
```

## Using them as lvalue reference variables
```cpp
#include <iostream>

int main() {
    int num { 12 }; // regular number
    int& ref { num }; // reference lvalue

    std::cout << num << "\n"; // we call num directly
    std::cout << ref << "\n"; // we call num, so the value 12, with the reference

    return 0;
}

```

## Changing values with reference variables

```cpp
#include <iostream>

int main() {

int x { 5 };
int& ref { x };

std::cout << x << ref << "\n"; // returns 55

x = 6;

std::cout << x << ref << "\n"; // returns 66

ref = 7;

std::cout << x << ref << "\n"; // returns 77

}
```

`ref` is an alias for `x`

we __can't__ initialize a reference without a value

they __must__ be bound to a modifiable value

we can only bound to values that are of the same type

references __can't__ be changed to point to another value

we can use const references to refer to constants, but can't change the value:

```cpp
#include <iostream>


int main() {

const int num { 2 };

const int& ref { num };


std::cout << num << ref << "\n";

ref = 2; // this gives an error since we are trying to change a const

return 0;

}
```

what we can do, is assign a _non_ constant value to a constant reference, meaning we can't change the value using the reference, but can otherwise

## But what is this useful for?

well, it's expensive to copy variables into functions like so:
```cpp
#include <iostream>
#include <string>

void print(std::string x) {
std::cout << x << "\n";
} // x is destroyed here

int main() {
std::string text {"Hello world!"}; // text is an std::string, big data

print(text); // text get's copied into the print function (expensive)
return 0;
}
```

so, how do we solve this with l-value references?

```cpp
#include <iostream>
#include <string>

void print(std::string& x) {
	std::cout << x << "\n";
} // x, the reference dies here

int main() {

	std::string text {"Hello world!"};
	print(text); // this is inexpensive, since this is just a reference to the variable

	return 0;
}
```

## Using this to change values:
```cpp
#include <iostream>

void addOne(int& x) { // x here is bound to the object, y
	++x; // this modifies the actual y
}

int main() {

	int y { 3 };
  
	std::cout << y << "\n";
  
	addOne(y); // beceause it's passed as a reference, the value can be changed

	std::cout << y << "\n"; // y has been modified
	return 0;

}
```


#cpp
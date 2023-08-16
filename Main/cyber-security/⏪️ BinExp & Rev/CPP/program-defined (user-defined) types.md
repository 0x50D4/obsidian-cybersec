## Alias
```cpp
#include <iostream>

using length = int;

int main() {
	length rope { 12 };
	std::cout << rope;
}

```

## Creating our own data types with structs

```cpp

// This only defines what a Fraction type looks like, it doesn't create one
struct Fraction
{
	int numerator {};
	int denominator {};
};

int main()
{
	Fraction f{ 3, 4 }; // this actually instantiates a Fraction object named f

	return 0;
}
```

```ad-info
The naming convention is that we name our structs starting with a capital letter, so `Fraction` is fine but `fraction`, `fraction_t`, or `Fraction_t` is not.
```

```ad-info
We usually use header files to declare new structs like so:
```

```cpp
#ifndef FRACTION_H
#define FRACTION_H

// Define a new type named Fraction
// This only defines what a Fraction looks like, it doesn't create one
// Note that this is a full definition, not a forward declaration
struct Fraction
{
	int numerator {};
	int denominator {};
};

#endif
```

We have to include it:
```cpp
#include "Fraction.h"
```


#cpp
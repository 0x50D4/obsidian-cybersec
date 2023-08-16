## Return by reference example

```cpp
#include <iostream>
#include <string>

const std::string& getProgramName() // returns a const reference
{
    static const std::string s_programName { "Calculator" }; // has static duration, destroyed at end of program

    return s_programName;
}

int main()
{
    std::cout << "This program is named " << getProgramName();

    return 0;
}
```

```ad-note
This works, the only danger is, that if we don't use `static` the object referenced gets destroyed and we leave a reference dangling
```

```ad-warning
References returned should be static, we should avoid making them non static!
```

```ad-hint
It’s okay to return reference parameters by reference
```

## Return by address
It works almost identical to return by reference, but there are some advantages:
- A major advantage is that we can return a `nullptr`
- Major disadvantage is that we have to do nullptr check before dereferencing the value

#cpp
# C++ Classes

## 1. Class Fundamentals

A class is a user-defined type that groups data and functions operating on that data.

It provides a way to organize related information and behavior into a single unit.

### Data Members and Member Functions

A class can contain:

* **Data members:** Variables that represent the state of an object.
* **Member functions:** Functions that operate on the object's data.

Example:

```cpp
class Counter {
private:
    int value{0};

public:
    void increment() {
        value++;
    }

    int getValue() const {
        return value;
    }
};
```

In this example:

* `value` is a data member.
* `increment()` and `getValue()` are member functions.
* `increment()` modifies the object's state.
* `getValue()` returns the current value.

### Access Specifiers

C++ provides access specifiers to control which parts of a class can be accessed from outside.

**`public`**

Members declared under `public` can be accessed from outside the class.

**`private`**

Members declared under `private` can only be accessed by the class's member functions and its friends.

This allows a class to control how its internal data is accessed and modified.

For example:

```cpp
Counter counter;

counter.increment();  // Valid
counter.getValue();   // Valid
counter.value = 10;   // Error: value is private
```

This principle is called **information hiding**.

### Objects

An object is an instance of a class.

Each object has its own non-static data members.

```cpp
Counter a;
Counter b;

a.increment();
a.increment();

std::cout << a.getValue(); // 2
std::cout << b.getValue(); // 0
```

Changing `a` does not change `b`, because they are separate objects.

### Key Takeaways

* A class defines a type and its behavior.
* Data members represent object state.
* Member functions operate on that state.
* `public` and `private` control access.
* Objects are independent instances of a class.


## 2. Constructors and Initialization

A constructor is a special member function that is called when an object is created.

Its name is the same as the class name, and it has no return type.

### Basic Constructor

```cpp
class Box {
private:
    int value;

public:
    Box() {
        value = 0;
    }
};
```

When a `Box` object is created, its constructor initializes `value`.

### Constructor Parameters and Default Arguments

A constructor can accept parameters to initialize an object with a specific value.

```cpp
class Box {
private:
    int value;

public:
    Box(int initialValue = 0)
        : value{initialValue} {
    }

    int getValue() const {
        return value;
    }
};
```

Now we can create objects in different ways:

```cpp
Box a;      // value = 0
Box b{12};  // value = 12
Box c(20);  // value = 20
```

The default argument allows the constructor to be called without providing a value.

### Member Initializer List

The syntax:

```cpp
Box(int initialValue)
    : value{initialValue} {
}
```

uses a member initializer list.

It initializes the data member directly before the constructor body executes.

An alternative is assigning inside the constructor body:

```cpp
Box(int initialValue) {
    value = initialValue;
}
```

Both work for ordinary assignable members, but the initializer list is generally preferred.

It is also necessary for initializing certain members, such as `const` members and references.

### Initializing Multiple Members

```cpp
class Person {
private:
    std::string name;
    int age;
    double height;

public:
    Person(std::string n, int a, double h)
        : name{n}, age{a}, height{h} {
    }
};
```

Each member is initialized through the initializer list.

### Key Takeaways

* Constructors run when objects are created.
* Constructors have the same name as their class and no return type.
* Parameters allow objects to start with different values.
* Default arguments allow omitted constructor arguments.
* Member initializer lists initialize members directly.


## 3. Accessors, Mutators, and const

Member functions can be classified based on whether they modify the object's state.

### Accessors

An accessor examines or returns information about an object without changing its state.

```cpp
int getBalance() const {
    return balance;
}
```

The `const` after the parameter list indicates that the function does not modify the object's state.

### Mutators

A mutator changes the object's state.

```cpp
void deposit(double amount) {
    balance += amount;
}
```

### Constant Member Functions

Consider:

```cpp
int getBalance() const {
    return balance;
}
```

The `const` qualifier:

* Prevents the function from modifying non-mutable data members.
* Allows the function to be called on constant objects.
* Expresses that the function is intended to observe, not modify, the object's state.

For example:

```cpp
const BankAccount account{100};

std::cout << account.getBalance(); // Valid
```

If `getBalance()` were not declared `const`, this call would be invalid.

### Constant Data Members

A data member can also be declared `const`.

```cpp
class Person {
private:
    const std::string nationalCode;

public:
    Person(std::string code)
        : nationalCode{code} {
    }

    std::string getNationalCode() const {
        return nationalCode;
    }
};
```

A `const` data member must be initialized, and it cannot be reassigned afterward.

### Key Takeaways

* Accessors examine an object's state.
* Mutators modify an object's state.
* `const` member functions provide compiler-enforced protection against modifying the object's state.
* Constant objects can call only compatible `const` member functions.
* Constant data members must be initialized and cannot be reassigned.




## 4. Explicit Constructors

In C++, a constructor with one parameter can allow implicit type conversions.

Consider:

```cpp
class Age {
private:
    int value;

public:
    Age(int initialValue)
        : value{initialValue} {
    }
};
```

Without `explicit`, this may be allowed:

```cpp
Age age = 20;
```

C++ implicitly converts the integer `20` into an `Age` object.

### Using `explicit`

```cpp
class Age {
private:
    int value;

public:
    explicit Age(int initialValue)
        : value{initialValue} {
    }
};
```

Now:

```cpp
Age a{20};   // Valid
Age b(20);   // Valid
Age c = 20;  // Error
```

The `explicit` keyword prevents the constructor from being used for implicit conversions.

### Why Use It?

Implicit conversions can introduce unintended behavior.

For example, a function expecting a `Money` object might accidentally accept an integer if a converting constructor exists.

Using `explicit` requires the caller to construct the intended type deliberately.

```cpp
deposit(Money{500});
```

This makes the code's intention clearer.

### Key Takeaways

* One-parameter constructors can enable implicit conversions.
* `explicit` prevents those implicit conversions.
* Direct construction remains valid.
* It helps avoid unintended type conversions and improves type safety.


## 5. Separation of Interface and Implementation

In C++, a class's interface and implementation are commonly separated into different files.

* **Interface (`.h`)**: Declares the class, its data members, and its member functions.
* **Implementation (`.cpp`)**: Defines how the member functions work.

This allows other source files to use a class without needing to know its internal implementation.

### Example Structure

```text
Counter.h
Counter.cpp
main.cpp
```

### Header File — `Counter.h`

```cpp
#ifndef COUNTER_H
#define COUNTER_H

class Counter {
private:
    int value{0};

public:
    void increment();
    int getValue() const;
};

#endif
```

The header describes the class and its available operations.

### Implementation File — `Counter.cpp`

```cpp
#include "Counter.h"

void Counter::increment() {
    value++;
}

int Counter::getValue() const {
    return value;
}
```

The `ClassName::functionName` syntax uses the **scope resolution operator (`::`)** to specify which class a function belongs to.

The `const` qualifier must match the declaration.

### Main File — `main.cpp`

```cpp
#include <iostream>
#include "Counter.h"

int main() {
    Counter counter;

    counter.increment();
    counter.increment();

    std::cout << counter.getValue() << '\n';

    return 0;
}
```

The main file uses the class through its interface.

### Compilation and Linking

The compiler uses declarations to understand and check function calls.

The implementation must also be compiled and linked into the final executable.

```bash
g++ main.cpp Counter.cpp -o app
./app
```

If the implementation is missing, compilation may succeed, but the linker can report an `undefined reference` error.

### Include Guards

Include guards prevent a header from being processed multiple times within the same translation unit.

```cpp
#ifndef COUNTER_H
#define COUNTER_H

// Class declaration

#endif
```

### Key Takeaways

* `.h` describes what a class exposes.
* `.cpp` defines how its functions work.
* `::` identifies a class member's scope.
* Declarations and definitions must match.
* The implementation must be linked into the final executable.
* Include guards prevent duplicate header inclusion.

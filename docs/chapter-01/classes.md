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

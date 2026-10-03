## 1. Pointers

A pointer is a variable that stores the address of another object.

```cpp
int age = 20;
int* ptr = &age;
```

* `int* ptr`: declares a pointer to an integer.
* `&age`: obtains the address of `age`.
* `ptr`: contains the address.
* `*ptr`: accesses the value at the address stored in the pointer.

### Dereferencing

```cpp
int x = 10;
int* p = &x;

*p = 25;

cout << x; // 25
```

Dereferencing allows us to access or modify the original object.

### Reassigning a pointer

```cpp
int a = 10;
int b = 20;

int* p = &a;
p = &b;

*p = 50;
```

Now `p` points to `b`, so the assignment modifies `b`.

```text
a = 10
b = 50
```

A pointer can point to different objects during its lifetime.

### Pointer vs. value copy

```cpp
int x = 10;
int* p = &x;

int y = *p;
y = 30;
```

`y` is an ordinary integer containing a copy of `x`'s value. Changing `y` does not affect `x`.

### Null pointers

`nullptr` represents a pointer that does not point to an object.

```cpp
int* p = nullptr;
```

A null pointer can be checked:

```cpp
if (p != nullptr) {
    cout << *p;
}
```

Dereferencing a null pointer causes undefined behavior.

```cpp
int* p = nullptr;
cout << *p; // Undefined behavior
```

A pointer should point to a valid object before it is dereferenced.



## 2. References

A reference is another name, or alias, for an existing object.

```cpp
int x = 10;
int& ref = x;

ref = 25;

cout << x; // 25
```

`ref` is not a separate copy. It refers to the same object as `x`.

### Reference initialization

```cpp
int a = 10;
int b = 20;

int& ref = a;
ref = b;
```

The assignment `ref = b` does not make `ref` refer to `b`.

Instead, it copies the value of `b` into `a`.

```text
a = 20
b = 20
```

A reference must be initialized when declared and cannot be reseated to refer to another object.

### Pointer vs. reference

| Pointer                        | Reference                        |
| ------------------------------ | -------------------------------- |
| Stores an address              | Acts as an alias                 |
| Can be reassigned              | Cannot be reseated               |
| Can be `nullptr`               | Must bind to a valid object      |
| Uses `*p` to access the target | Uses the reference name directly |

## 3. Lvalues and Rvalues

C++ expressions have different value categories. For now, the basic distinction is:

* **Lvalue:** An expression that identifies an object.
* **Rvalue:** An expression representing a value or temporary result.

```cpp
int x = 10;

x = 20;      // Valid
10 = x;      // Invalid
x + 5 = 20;  // Invalid
```

`x` is an lvalue, while `10` and `x + 5` are rvalues in these examples.

### Lvalue references

```cpp
int x = 10;
int& ref = x;
```

An ordinary lvalue reference (`T&`) binds to an appropriate lvalue.

### Rvalue references

```cpp
int&& ref = 20;
```

An rvalue reference (`T&&`) can bind to an rvalue.

Rvalue references are important for move semantics.

One subtle rule: although `ref` has an rvalue-reference type, using the name `ref` in an expression makes that expression an lvalue.


## 4. Parameter Passing

Function parameters determine whether a function receives a copy, can modify the original object, or can read it without copying.

### Pass by value

```cpp
void change(int x) {
    x = 100;
}

int main() {
    int a = 10;
    change(a);

    cout << a; // 10
}
```

The function receives a copy. Modifying the parameter does not modify the original variable.

### Pass by reference

```cpp
void change(int& x) {
    x = 100;
}

int main() {
    int a = 10;
    change(a);

    cout << a; // 100
}
```

The parameter is an alias for the original variable. Changes affect the caller's object.

### Pass by constant reference

```cpp
void print(const int& x) {
    cout << x;
}
```

A constant reference:

* Allows reading the original object.
* Prevents modification through that reference.
* Avoids copying the object.

This is especially useful for large objects such as strings and vectors.

### Choosing a parameter type

| Parameter        | Behavior                | Typical use                                       |
| ---------------- | ----------------------- | ------------------------------------------------- |
| `T value`        | Receives a copy         | Small values or when a copy is needed             |
| `T& value`       | Can modify the original | Functions that need to modify the caller's object |
| `const T& value` | Reads without copying   | Large objects that should not be modified         |

Example:

```cpp
void process(const std::vector<int>& values) {
    for (int value : values) {
        cout << value << '\n';
    }
}
```

The vector is not copied, and the function cannot modify it through `values`.


## 5. Return Passing

A function can return a value or a reference.

### Return by value

```cpp
int getValue(int x) {
    return x;
}
```

The function returns a value, not an alias to the original argument.

Returning a local variable by value is safe:

```cpp
int getValue() {
    int x = 10;
    return x;
}
```

The returned value remains usable after the local variable is destroyed.

### Return by reference

```cpp
int& getValue(int& x) {
    return x;
}
```

This returns a reference to the original object.

```cpp
int a = 10;

getValue(a) = 50;

cout << a; // 50
```

Because the function returns a reference, its result can be used to modify the original object.

### Dangling references

Never return a reference to a local variable:

```cpp
int& getValue() {
    int x = 10;
    return x; // Dangerous: x is destroyed on return
}
```

The returned reference would dangle because the referenced object no longer exists.

### Return by value vs. reference

| Return type | Result                                 |
| ----------- | -------------------------------------- |
| `T`         | Returns a value                        |
| `T&`        | Returns an alias to an existing object |

Returning by reference is only safe when the referenced object outlives the use of the returned reference.


## 6. `std::swap` and `std::move`

### `std::swap`

`std::swap` exchanges the values of two objects.

```cpp
#include <utility>

int a = 10;
int b = 20;

std::swap(a, b);

cout << a; // 20
cout << b; // 10
```

Conceptually, swapping can be understood as:

```cpp
int temp = a;
a = b;
b = temp;
```

The standard library provides a generic implementation for supported types.

### Copying vs. moving

Consider:

```cpp
std::string a = "Hello";
std::string b = a;
```

Copying creates an independent string. Modifying one does not modify the other.

Moving is different:

```cpp
#include <utility>
#include <string>

std::string a = "Hello";
std::string b = std::move(a);
```

`std::move` allows the object to be treated as an rvalue, enabling move semantics when supported by its type.

For types such as `std::string` and `std::vector`, moving can transfer resources rather than copying all their contents.

**Important:** `std::move` itself does not move anything. It is a cast that enables the appropriate move operation.

### State after moving

After moving from an object, it remains valid, but its value is generally unspecified.

```cpp
std::string a = "Hello";
std::string b = std::move(a);

a = "World";

cout << a; // World
cout << b; // Hello
```

The moved-from string can safely be assigned a new value.

### Copy vs. move

| Copy                                      | Move                                                   |
| ----------------------------------------- | ------------------------------------------------------ |
| Creates an independent value              | Can transfer resources                                 |
| Source retains its value                  | Source remains valid, but its value may be unspecified |
| May involve copying elements or resources | Can avoid expensive copying                            |

For standard library containers, moving is often useful when transferring ownership of their allocated resources.





## 1.5.7 C-Style Arrays and Strings

### The array name is a pointer

```cpp
int arr[3] = {10, 20, 30};
```

`arr` is not a real array object. It is a **constant pointer** to memory large enough for 3 ints.

```
arr → [10][20][30]
        0   1   2
```

Consequences:

* `b = arr;` is illegal, because `arr` is a constant pointer.
* When an array is passed to a function, only the address is passed. The size is lost, so it must be passed as an extra parameter.
* There is no range checking. `arr[5] = 1;` compiles and writes into memory that belongs to something else (undefined behavior).

```cpp
int sum(int* a, int length) {
    int total = 0;
    for (int i = 0; i < length; ++i) {
        total += a[i];
    }
    return total;
}

sum(arr, 3);
```



### Dynamic arrays with `new[]` and `delete[]`

A C-style array size must be a compile-time constant. If the size is only known at runtime, allocate with `new[]`:

```cpp
int n = 5;
int* arr2 = new int[n];
// use arr2[0] ... arr2[n - 1]
delete[] arr2;
```

Memory from `new[]` must be freed with `delete[]`. Forgetting it causes a **memory leak**.

| | `new int[5]` | `int arr[5] = {1, 2, 3, 4, 5}` |
|---|---|---|
| Memory | heap, allocated at runtime | stack, part of the function |
| Initial values | garbage | the listed values |
| Freed by | you, with `delete[]` | automatically at end of scope |
| Size | can be a variable | must be a constant |
| The name | normal pointer, can be reassigned | constant pointer |

Rules:

* Every `new[]` is paired with `delete[]`.
* Do not use the pointer after `delete[]`.



### C-style strings

A C-style string is a `char` array that ends with the null terminator `'\0'`:

```cpp
char s[6] = "Hello";
```

```
s → [H][e][l][l][o][\0]
```

`"Hello"` has 5 letters but needs **6** boxes, one for `'\0'`.

Functions from `<cstring>`:

```cpp
strlen(s);           // 5, counts until '\0'
strcmp(s, "Hello");  // 0 means equal
strcpy(t, s);        // copies including '\0'
```

`strcpy` does not check that the target is big enough. If it is too small, the `'\0'` lands outside the array and corrupts memory.



### Which one to use

`vector` and `string` are built on top of C-style arrays and strings and hide their dangers: they know their own size and free their own memory. It is almost always better to use them.

Use C-style only when:

* a C library function requires it, or
* (rarely) a section of code must be optimized for speed.

Extra context (not from the book): `std::string::c_str()` returns a `const char*`, so you can keep `std::string` and convert only at the call to such a function.




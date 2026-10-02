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

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

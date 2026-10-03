# Templates

Many algorithms and data structures do not depend on the type of the data they work on. Finding the largest item in an array works the same for ints, doubles, or strings, as long as items can be compared. A **template** lets us write that logic once instead of once per type. Every data structure later in this book (`vector`, `list`, stacks, trees, heaps) is written as a template.

## 1.6.1 Function Templates

A function template is not a function. It is a **pattern** from which the compiler generates real functions.

```cpp
// Returns the largest item in a.
// Assumes a.size() > 0.
// Comparable must provide operator< and operator=.
template <typename Comparable>
const Comparable& findMax(const vector<Comparable>& a) {
    int maxIndex = 0;

    for (int i = 1; i < a.size(); ++i) {
        if (a[maxIndex] < a[i]) {
            maxIndex = i;
        }
    }

    return a[maxIndex];
}
```

`Comparable` is the template argument. It can be replaced by any type.

```cpp
vector<int> v1 = {3, 9, 4};
vector<string> v3 = {"a", "c", "b"};

findMax(v1);   // compiler generates a version with Comparable = int
findMax(v3);   // compiler generates a version with Comparable = string
```

### Things to remember

* **Expansion is automatic.** One real function is generated for each distinct type used. Many types in a big project means more generated code, known as **code bloat**.
* **The type must support what the template uses.** `findMax` uses `<`. Calling it with a class that has no `operator<` is a compile-time error.
* **Document the assumptions** in a comment above the template (which operators and constructors the type needs).
* **Pass and return by const reference.** The template argument may be a large class, so assume it is not a primitive type.
* **Overload rules:**
  * If a non-template function and a template both match, the non-template wins.
  * If two templates match equally well through approximate matches, the call is ambiguous and the code is illegal.




## 1.6.2 Class Templates

A class template works like a function template, but the pattern generates whole classes.

```cpp
template <typename Object>
class MemoryCell {
public:
    explicit MemoryCell(const Object& initialValue = Object{})
        : storedValue{initialValue} {
    }

    const Object& read() const {
        return storedValue;
    }

    void write(const Object& x) {
        storedValue = x;
    }

private:
    Object storedValue;
};
```

Usage:

```cpp
MemoryCell<int> m1;
MemoryCell<string> m2{"hello"};

m1.write(37);
m2.write(m2.read() + " world");

cout << m1.read() << '\n';   // 37
cout << m2.read() << '\n';   // hello world
```

### Things to remember

* `MemoryCell` is **not a class**, it is a class template. `MemoryCell<int>` and `MemoryCell<string>` are the actual classes.
* `Object` must have a zero-parameter constructor, a copy constructor, and a copy assignment operator.
* The constructor's default argument is `Object{}`, not `0`, because `0` may not be a valid `Object`.
* `Object` is passed by const reference because it may be large.
* Most class templates are written entirely in the header (see 1.6.5).
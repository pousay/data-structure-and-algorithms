# Recursion

Recursion is a technique where a function solves a problem by calling itself with a smaller or simpler version of the same problem.

A recursive function needs a way to eventually stop. Without that, the function keeps calling itself until the program runs out of stack space.

## The Four Rules of Recursion

### 1. Base Case

Every recursive function needs at least one **base case**.

The base case is the situation where the function already knows the answer and does not need another recursive call.

```cpp
int f(int n)
{
    if (n == 0)
        return 0;

    return n + f(n - 1);
}
```

Here, `n == 0` is the base case.

---

### 2. Make Progress Toward the Base Case

Every recursive call should move the problem closer to a base case.

```cpp
f(n - 1);
```

moves `n` toward `0`.

If the function keeps calling itself without getting closer to a base case, the recursion will never terminate.

---

### 3. Design Rule

When designing a recursive function, assume that the recursive call already works correctly.

For example:

```cpp
int f(int n)
{
    if (n == 0)
        return 0;

    return n + f(n - 1);
}
```

Instead of trying to mentally solve `f(n - 1)` again, we can assume it correctly returns the sum from `n - 1` down to `0`.

Then we only need to add `n`.

---

### 4. Compound-Interest Rule

Avoid recursion that repeatedly performs the same work.

A recursive algorithm can look elegant while doing a huge amount of unnecessary computation.

This becomes especially important with recursive algorithms that make multiple recursive calls.

> **Don't compute the same thing more than necessary.**

The Fibonacci example later shows why this rule matters.

---

# Example: Factorial

Factorial is a simple example of recursion.

```cpp
long factorial(int n)
{
    if (n <= 1)
        return 1;

    return n * factorial(n - 1);
}
```

For:

```cpp
factorial(4)
```

the calls are:

```text
factorial(4)
→ 4 * factorial(3)
→ 4 * 3 * factorial(2)
→ 4 * 3 * 2 * factorial(1)
```

The base case returns `1`.

Then the calls finish in reverse order:

```text
factorial(1) = 1
factorial(2) = 2
factorial(3) = 6
factorial(4) = 24
```

---

# Tracing Recursion

Consider:

```cpp
int f(int n)
{
    if (n == 0)
        return 0;

    return n + f(n - 1);
}
```

For `f(3)`:

```text
f(3)
→ 3 + f(2)
→ 3 + 2 + f(1)
→ 3 + 2 + 1 + f(0)
```

At `f(0)`, the base case is reached.

The function then returns back through the previous calls:

```text
f(0) = 0
f(1) = 1
f(2) = 3
f(3) = 6
```

The important idea is that recursive calls first go **down toward the base case**, and then the unfinished work is completed while the calls return.

---

# Another Example

```cpp
int f(int x)
{
    if (x == 0)
        return 0;

    return 2 * f(x - 1) + x * x;
}
```

For `f(4)`:

```text
f(4)
→ 2 * f(3) + 16
→ 2 * f(2) + 9
→ 2 * f(1) + 4
→ 2 * f(0) + 1
```

Starting from the base case:

```text
f(0) = 0
f(1) = 1
f(2) = 6
f(3) = 21
f(4) = 58
```

The recursive call does not immediately give us the final answer. Each function call has unfinished work that must be completed when the recursive call returns.

---

# The Most Important Distinction

Where the code appears relative to the recursive call matters.

### Work before the recursive call

```cpp
cout << n;
f(n - 1);
```

The output happens while the recursion is going **down**.

For `f(4)`:

```text
4 3 2 1
```

### Work after the recursive call

```cpp
f(n - 1);
cout << n;
```

The output happens while the recursion is **returning back up**.

For `f(4)`:

```text
1 2 3 4
```

> **Code before the recursive call runs while going down.
> Code after the recursive call runs while coming back up.**

This distinction is one of the most important things to understand when tracing recursive functions.

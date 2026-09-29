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




# Recursive Calls and the Call Stack

Every time a function calls another function, the program needs to remember enough information to return to the correct place afterward.

Recursive calls are handled using a **stack**.

Each active function call has a **stack frame** (also called an activation record) containing the information needed for that call.

## Example

Consider:

```cpp
int f(int n)
{
    if (n == 0)
        return 0;

    return n + f(n - 1);
}
```

When calling:

```cpp
f(3);
```

the calls build up like this:

```text
f(3)
  f(2)
    f(1)
      f(0)
```

At this point, `f(0)` reaches the base case and returns.

The stack then unwinds:

```text
f(0) returns
    ↓
f(1) finishes
    ↓
f(2) finishes
    ↓
f(3) finishes
```

The stack follows **LIFO**:

> Last In, First Out.

The most recent function call must finish before the previous one can continue.

---

## Why Does the Stack Matter?

Every active recursive call requires a stack frame.

For a small recursion depth this is normally fine.

But if recursion creates a very large number of simultaneously active calls, the program can run out of stack space.

This is called **stack overflow**.

For example, recursion without a working base case can continue indefinitely:

```cpp
void bad(int n)
{
    bad(n - 1);
}
```

There is no base case, so the number of active calls keeps increasing until the stack is exhausted.

---

## Recursion Is Not Automatically Bad

Recursion can be very useful when the problem naturally has a recursive structure.

The important questions are:

* Is there a correct base case?
* Does every call make progress toward it?
* Is the recursive structure actually useful?
* Are we repeatedly doing the same work?

A recursive solution can be much clearer than an equivalent iterative solution.

On the other hand, recursion that simply behaves like a loop may not be a good use of recursion.

For example, factorial:

```cpp
long factorial(int n)
{
    if (n <= 1)
        return 1;

    return n * factorial(n - 1);
}
```

essentially performs the same work as a simple loop.

The important lesson is not "avoid recursion."

It is:

> **Use recursion when the recursive structure actually helps solve the problem.**




## Tail Recursion

A recursive function is **tail recursive** when the recursive call is the last operation performed by the function.

For example:

```cpp
void count(int n)
{
    if (n == 0)
        return;

    cout << n << " ";
    count(n - 1);
}
```

Here, after `count(n - 1)` returns, there is nothing left to do. This makes it possible to replace the recursion with a loop:

```cpp
void count(int n)
{
    while (n != 0)
    {
        cout << n << " ";
        n = n - 1;
    }
}
```

### Why Tail Recursion Can Be a Problem

Recursive calls use the call stack. With:

```cpp
count(4);
```

the calls build up like this:

```text
count(4)
count(3)
count(2)
count(1)
count(0)
```

But none of the previous calls need to do anything after the recursive call finishes.

So the stack is being used unnecessarily. For a sufficiently large number of recursive calls, this can even cause a stack overflow.

### Tail Recursive vs Non-Tail Recursive

Consider:

```cpp
int sum(int n)
{
    if (n == 0)
        return 0;

    return n + sum(n - 1);
}
```

This is **not** tail recursive.

The recursive call is not the final operation because after `sum(n - 1)` returns, we still need to perform:

```cpp
n + result
```

For example:

```text
sum(3)
  ↓
3 + sum(2)
      ↓
    2 + sum(1)
          ↓
        1 + sum(0)
              ↓
              0
```

Then the calls return upward:

```text
sum(0) → 0
sum(1) → 1 + 0 = 1
sum(2) → 2 + 1 = 3
sum(3) → 3 + 3 = 6
```

The stack is necessary because each call must remember its value of `n` while waiting for the recursive call to finish.

> **Important:** Code before the recursive call runs while going down. Code after the recursive call runs while coming back up.

The main distinction is:

```cpp
count(n - 1);          // tail recursion
```

Nothing remains to do.

```cpp
n + sum(n - 1);        // not tail recursion
```

Work remains after the recursive call returns.

The book describes tail recursion as a recursive call at the last line and notes that it can be mechanically eliminated by replacing it with a loop.



## Redundant Recursive Work

Recursion itself is not necessarily inefficient. A major problem appears when different recursive calls repeatedly solve the same subproblem.

A classic example is the recursive Fibonacci function:

```cpp
long fib(int n)
{
    if (n <= 1)
        return 1;

    return fib(n - 1) + fib(n - 2);
}
```

At first, this looks like a natural implementation of the Fibonacci definition. However, it performs a large amount of duplicated work.

For example, `fib(5)` creates a tree like this:

```text
fib(5)
├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   └── fib(1)
│   └── fib(2)
└── fib(3)
    ├── fib(2)
    └── fib(1)
```

Notice that `fib(3)` is calculated twice, and `fib(2)` is calculated three times.

As `n` becomes larger, the number of repeated calculations grows extremely quickly.

### Why It Becomes Exponential

Let `T(N)` represent the running time of `fib(N)`.

Each non-base call performs two recursive calls:

```text
T(N) = T(N - 1) + T(N - 2) + 2
```

The two recursive calls cause the amount of work to grow very quickly. The resulting running time is exponential.

This means that increasing `N` by a relatively small amount can cause a huge increase in the number of function calls.

For comparison:

```text
O(N)       → grows linearly
O(N²)      → grows quadratically
O(2^N)     → grows exponentially
```

The important issue is not simply that recursion is being used. The problem is that the same subproblems are being solved over and over again.

### Avoiding Repeated Work

Instead of recalculating the same Fibonacci values, we can calculate each value once and store the results.

For example, we can use an array and a loop:

```cpp
long fib(int n)
{
    if (n <= 1)
        return 1;

    vector<long> values(n + 1);

    values[0] = 1;
    values[1] = 1;

    for (int i = 2; i <= n; ++i)
        values[i] = values[i - 1] + values[i - 2];

    return values[n];
}
```

Now each Fibonacci value is calculated only once.

The book uses this example to demonstrate the **compound-interest rule** of recursion: avoid recursive algorithms that repeatedly perform the same work.

### Main Lesson

> **Recursion is not automatically bad. Repeatedly solving the same subproblem is the real problem.**

When designing a recursive algorithm, ask:

1. Do I have a clear base case?
2. Does every recursive call make progress toward it?
3. Is there u

# Mathematical Preliminaries

Before getting into data structures and algorithm analysis, we need a few mathematical tools that will appear throughout the book.

This section covers:

* Exponents (1.2.1)
* Logarithms and logarithm rules (1.2.2)
* Series (1.2.3)
* Modular arithmetic (1.2.4)
* Proof techniques: induction, contradiction, counterexamples (1.2.5)

---

## 1.2.1 Exponents

```text
X^A * X^B   = X^(A+B)
X^A / X^B   = X^(A-B)
(X^A)^B     = X^(AB)
X^N + X^N   = 2 * X^N     (not X^(2N))
2^N + 2^N   = 2^(N+1)
```

The last two are the easy ones to get wrong. Adding two equal powers doubles the value, it does not double the exponent.

Example: `2^10 + 2^10 = 1024 + 1024 = 2048 = 2^11`.

---

## 1.2.2 Logarithms

A logarithm answers a simple question:

> **What power do we need to raise the base to, to get a certain number?**

For example:

```text
2³ = 8
```

Therefore:

```text
log₂ 8 = 3
```

In general:

```text
log_b x = y
```

means:

```text
bʸ = x
```

### Examples

```text
log₂ 8   = 3    → 2³ = 8
log₂ 16  = 4    → 2⁴ = 16
log₂ 32  = 5    → 2⁵ = 32
log₂ 64  = 6    → 2⁶ = 64
```

### Why are logarithms important?

Logarithms appear frequently in computer science because many algorithms repeatedly reduce the size of a problem.

For example, if we repeatedly divide a number by 2:

```text
1024 → 512 → 256 → 128 → 64 → 32 → 16 → 8 → 4 → 2 → 1
```

We divided by 2 exactly 10 times.

Therefore:

```text
log₂ 1024 = 10
```

This becomes especially useful when analyzing algorithms that repeatedly cut their input in half.

### Base 2

In computer science, logarithms are commonly written with base 2:

```text
log N
```

usually means:

```text
log₂ N
```

unless another base is explicitly specified.

For example:

```text
log 8 = 3
log 16 = 4
log 1024 = 10
```

because the base is assumed to be 2.

### Logarithm Rules

Logarithms have several useful rules that make calculations easier.

#### Product Rule

When multiplying two values:

```text
log_b(xy) = log_b(x) + log_b(y)
```

For example:

```text
log₂(8 × 32)
```

can be written as:

```text
log₂ 8 + log₂ 32
```

which gives:

```text
3 + 5 = 8
```

Therefore:

```text
log₂(8 × 32) = 8
```

#### Quotient Rule

When dividing two values:

```text
log_b(x / y) = log_b(x) - log_b(y)
```

For example:

```text
log₂(256 / 16)
```

becomes:

```text
log₂ 256 - log₂ 16
```

so:

```text
8 - 4 = 4
```

Therefore:

```text
log₂(256 / 16) = 4
```

#### Power Rule

When a value is raised to a power:

```text
log_b(xᵏ) = k log_b(x)
```

For example:

```text
log₂(4⁵)
```

becomes:

```text
5 log₂ 4
```

Since:

```text
log₂ 4 = 2
```

we get:

```text
5 × 2 = 10
```

Therefore:

```text
log₂(4⁵) = 10
```

### Logarithms and Exponents

Logarithms and exponents are essentially inverse operations.

If:

```text
log₂ N = 12
```

then:

```text
N = 2¹²
```

which means:

```text
N = 4096
```

Likewise:

```text
2²⁰ = 1,048,576
```

so:

```text
log₂(1,048,576) = 20
```

Understanding this relationship is important because algorithm analysis frequently moves between exponential and logarithmic expressions.



---

## 1.2.3 Series

### Geometric series

```text
sum of 2^i  for i = 0..N   =  2^(N+1) - 1
sum of A^i  for i = 0..N   =  (A^(N+1) - 1) / (A - 1)
```

Check: `1 + 2 + 4 + 8 = 15 = 2^4 - 1`.

If `0 < A < 1`, the sum is bounded:

```text
sum of A^i  for i = 0..N   <=  1 / (1 - A)
```

and as `N` goes to infinity the sum approaches `1 / (1 - A)`. For `A = 1/2`: `1 + 1/2 + 1/4 + ... = 2`.

### Deriving the infinite sum

Let `S = 1 + A + A^2 + A^3 + ...`. Then `A*S = A + A^2 + A^3 + ...`. Subtract the two equations. Almost everything cancels:

```text
S - A*S = 1   =>   S = 1 / (1 - A)
```

This subtraction is only allowed for a convergent series.

The same trick gives `sum of i / 2^i for i = 1..infinity`. Write `S = 1/2 + 2/4 + 3/8 + ...`, then `2S = 1 + 2/2 + 3/4 + 4/8 + ...`. Subtracting gives `S = 1 + 1/2 + 1/4 + ... = 2`.

### Other sums that appear often

```text
sum of i      for i = 1..N  =  N(N+1)/2          ~  N^2 / 2
sum of i^2    for i = 1..N  =  N(N+1)(2N+1)/6    ~  N^3 / 3
sum of i^k    for i = 1..N  ~  N^(k+1) / |k+1|        (k != -1)
```

For `k = -1` the last formula does not work. That case is the **harmonic sum**:

```text
H_N = sum of 1/i for i = 1..N  ~  ln N
```

The error in this approximation tends to Euler's constant, about `0.5772`. The harmonic sum shows up far more in computer science than in other fields.

### Two algebra rules

```text
sum of f(N) for i = 1..N      =  N * f(N)          (f(N) does not depend on i)
sum of f(i) for i = n0..N     =  (sum for i = 1..N) - (sum for i = 1..n0-1)
```

### Why this matters

Loops translate into sums. A loop that runs `i` times for each `i` from 1 to `N` costs `1 + 2 + ... + N = N(N+1)/2`, which is `O(N^2)`. Chapter 2 relies on these formulas constantly.
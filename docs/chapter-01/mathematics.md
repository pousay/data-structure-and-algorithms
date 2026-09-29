# Mathematical Preliminaries

This section covers:

* Logarithms
* Logarithm rules

---

## Logarithms

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

---

## Logarithm Rules

Logarithms have several useful rules that make calculations easier.

### Product Rule

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

---

### Quotient Rule

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

---

### Power Rule

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

---

## Logarithms and Exponents

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

## Summary

The main ideas from this section are:

* A logarithm tells us which power produces a number.
* `log_b x = y` means `bʸ = x`.
* In this book, logarithms are assumed to have base 2 unless otherwise specified.
* Repeatedly dividing a value by 2 leads to a logarithmic number of steps.
* Product, quotient, and power rules allow logarithmic expressions to be simplified.
* Logarithms and exponents are inverse operations.

These concepts will become particularly useful as we move into **algorithm analysis and recursion**.




# Proof Techniques

## Mathematical Induction

Mathematical induction is a technique used to prove that a statement is true for **every positive integer**.

A useful way to think about it is with dominoes:

1. Prove that the first domino falls.
2. Prove that whenever one domino falls, the next one also falls.
3. Therefore, all the dominoes fall.

The same idea is used in mathematical induction.

### The Three Steps

A typical induction proof has three parts:

#### 1. Base Case

First, prove that the statement is true for the starting value, usually `n = 1`.

#### 2. Inductive Hypothesis

Assume that the statement is true for some arbitrary value `n`.

This assumption is called the **inductive hypothesis**.

#### 3. Inductive Step

Using the inductive hypothesis, prove that the statement is also true for `n + 1`.

If the base case is true and the inductive step works, the statement is true for all positive integers.

---

## Example

Consider the statement:

```text
1 + 3 + 5 + ... + (2n - 1) = n²
```

We want to prove that this is true for every positive integer `n`.

### Step 1 — Base Case

For `n = 1`:

```text
1 = 1²
```

which is true.

So the base case works.

### Step 2 — Inductive Hypothesis

Assume the statement is true for some `n`:

```text
1 + 3 + 5 + ... + (2n - 1) = n²
```

We don't need to prove this assumption. We temporarily assume it is true so that we can prove the next case.

### Step 3 — Inductive Step

We need to prove:

```text
1 + 3 + 5 + ... + (2n - 1) + (2(n + 1) - 1) = (n + 1)²
```

From the inductive hypothesis:

```text
1 + 3 + 5 + ... + (2n - 1) = n²
```

So we can replace the first part with `n²`:

```text
n² + (2(n + 1) - 1)
```

Simplify:

```text
n² + 2n + 1
```

which is:

```text
(n + 1)²
```

Therefore, if the statement is true for `n`, it is also true for `n + 1`.

Since the base case is true and the inductive step works, the statement is true for every positive integer `n`.

---

## Why Induction Matters

Induction is useful when we need to prove that something works for an entire sequence of values rather than checking each value individually.

In computer science, induction can be used to reason about:

* Recursive algorithms
* Properties of data structures
* Mathematical formulas
* Algorithm correctness

The important pattern to remember is:

```text
Base Case
    ↓
Assume it works for n
    ↓
Prove it works for n + 1
    ↓
Therefore it works for all n
```

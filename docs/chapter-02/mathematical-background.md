# Mathematical Background

To compare algorithms we need a way to ignore the machine, the compiler, and small constant factors. The tool is **growth rate**: how fast the cost increases as the input size `N` increases.

## Why Growth Rate, Not Speed

```text
A takes 1000 * N steps
B takes N * N steps
```

```text
N = 10       A = 10,000       B = 100
N = 1000     A = 1,000,000    B = 1,000,000
N = 100000   A = 100,000,000  B = 10,000,000,000
```

`B` is faster for small inputs, but `A` wins for every `N` past 1000. Comparing the cost at one size is misleading. What matters is what happens as `N` grows.

## The Four Notations

`T(N)` is the running time. `c` and `n0` are positive constants we get to choose.

| Notation | Name | Meaning | Reads as |
|---|---|---|---|
| `T(N) = O(f(N))` | Big-Oh | `T(N) <= c * f(N)` for all `N >= n0` | at most (upper bound) |
| `T(N) = Ω(g(N))` | Big-Omega | `T(N) >= c * g(N)` for all `N >= n0` | at least (lower bound) |
| `T(N) = Θ(h(N))` | Big-Theta | both `O(h(N))` and `Ω(h(N))` | exactly (tight bound) |
| `T(N) = o(p(N))` | little-oh | `T(N) = O(p(N))` but not `Θ(p(N))` | strictly less |

Together these are called **asymptotic notations**.

### Example: proving a Big-Oh

Show that `1000N = O(N²)`:

```text
1000 * N <= 1 * N * N    whenever N >= 1000
```

So `c = 1` and `n0 = 1000` work. Any single valid pair is a complete proof, and `c = 100`, `n0 = 10` would work too.

Show that `3N² + 10N = O(N²)`:

```text
For N >= 1:   10N <= 10 * N²
so            3N² + 10N <= 3N² + 10N² = 13 * N²
```

So `c = 13` and `n0 = 1`.

### Upper bounds can be loose

For `T(N) = 3N² + 10N`:

```text
O(N²)   true
O(N³)   true   (an upper bound can be loose)
Θ(N²)   true   (it is O(N²) and also Ω(N²), because 3N² + 10N >= 3N²)
Θ(N³)   false  (N³ outgrows it, so it cannot be a lower bound)
```

The useful answer is the tight one, `Θ(N²)`.






## Simplification Rules

In practice we do not write `c` and `n0` again. We simplify:

1. **Drop constants:** `5N²` is `O(N²)`.
2. **Drop lower-order terms:** `N² + N` is `O(N²)`.
3. **Sum:** `O(f) + O(g)` is `O(max(f, g))`.
4. **Product:** `O(f) * O(g)` is `O(f * g)`.

```text
N³ + 50N² + 7                 → O(N³)
4N log N + 7N² + 1000         → O(N²)
(N) * (log N)                 → O(N log N)
```

### The growth ladder (slowest to fastest)

```text
1  <  log N  <  N  <  N log N  <  N²  <  N³  <  2^N
```

Logarithms grow very slowly: `log^k N = O(N)` for any constant `k`. In this book, `log` means base 2 unless stated otherwise. Changing the base only changes a constant factor.

## Comparing Two Functions

Take the limit of the ratio as `N` goes to infinity:

```text
lim f(N) / g(N)  =  0          →  f = o(g)     (f grows slower)
lim f(N) / g(N)  =  constant   →  f = Θ(g)     (same rate)
lim f(N) / g(N)  =  ∞          →  g = o(f)     (f grows faster)
```

Calculus is rarely needed. Cancel the common factor and compare what is left:

```text
N log N   vs   N^1.5
divide both by N:    log N   vs   N^0.5
```

`N^0.5` pulls away quickly:

```text
N = 10^4:   log N is about 13,  N^0.5 = 100
N = 10^8:   log N is about 27,  N^0.5 = 10,000
```

So `N^1.5` grows faster than `N log N`.

**Rule:** any power of `N` eventually beats any power of `log N`.

## Style Rules

* Write `O(N²)`, not `O(3N² + N)`. Drop constants and lower-order terms inside the Big-Oh.
* Do not write `f(N) <= O(g(N))`. The "at most" is already part of `O`.
* Do not write `f(N) >= O(g(N))`. It means nothing.
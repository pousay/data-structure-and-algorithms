# 2.3 What to Analyze

The most important resource to analyze is **running time**. It depends on the compiler and the computer (outside the model), on the algorithm used, and on the input. The input size `N` is usually the main factor.

## Worst case, average case, best case

```text
T_worst(N)  =  running time of the slowest input of size N
T_avg(N)    =  average running time over inputs of size N

T_avg(N) <= T_worst(N)
```

* **Best case** is rarely useful, because it does not show typical behavior.
* **Average case** often shows typical behavior, but it is much harder to compute, and the definition of "average input" can change the answer.
* **Worst case** is a guarantee for every possible input, so it is the one normally asked for unless stated otherwise.

## Algorithms, not programs

Our bounds are for **algorithms**, not for a particular program. The programming language rarely changes a Big-Oh answer. If a program is much slower than the analysis predicts, look for an implementation mistake. In C++ a typical one is copying a whole array or vector by passing it by value instead of by reference.

## The running example: maximum subsequence sum

> Given (possibly negative) integers `A1, A2, ..., AN`, find the maximum value of the sum of `Ak` for `k = i..j`. The answer is `0` if all the integers are negative.

```text
input:   -2, 11, -4, 13, -5, -2
answer:  20      (11 - 4 + 13, the items A2 through A4)
```

There are many algorithms for this problem and their speeds differ enormously. We will see four, with running times `O(N³)`, `O(N²)`, `O(N log N)`, and `O(N)`.

## Measured running times (seconds)

| N | Algorithm 1 `O(N³)` | Algorithm 2 `O(N²)` | Algorithm 3 `O(N log N)` | Algorithm 4 `O(N)` |
|---:|---:|---:|---:|---:|
| 100 | 0.000159 | 0.000006 | 0.000005 | 0.000002 |
| 1,000 | 0.095857 | 0.000371 | 0.000060 | 0.000022 |
| 10,000 | 86.67 | 0.033322 | 0.000619 | 0.000222 |
| 100,000 | NA | 3.33 | 0.006700 | 0.002205 |
| 1,000,000 | NA | NA | 0.074870 | 0.022711 |

## What the table shows

* **Small inputs:** every algorithm runs in the blink of an eye, so a clever algorithm may not be worth the effort. Programs written for small inputs often become too slow years later when the input grows.
* **Reading the input** is not included in these times. For the linear algorithm, reading the data from disk can take longer than solving the problem. Efficient algorithms should not be the bottleneck.
* **Growing `N` by 10 times:**
  * linear: the time grows about **10 times**
  * quadratic: about **100 times** (`10²`)
  * cubic: about **1000 times** (`10³`)

  Example: algorithm 2 takes 3.33 s at `N = 100,000`, so we expect about **333 s** at `N = 1,000,000`. The real time can be a little longer, because larger inputs can cause slower memory access (cache effects).
* **For large inputs** algorithm 4 is clearly the best, and algorithm 3 is still usable.
* **Plots can mislead.** The `N log N` curve looks almost linear, and the linear curve looks flat for small `N`, because its constant term is larger than its linear term at that size.
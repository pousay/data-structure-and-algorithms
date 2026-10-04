# 2.2 Model

To analyze algorithms formally we need a model of computation. Ours is a simple, idealized computer:

* instructions run **one after another**
* every simple operation (addition, multiplication, comparison, assignment) takes **exactly one time unit**
* integers have a **fixed size** (for example 32 bits)
* there are **no magic operations**: matrix inversion or sorting cannot be done in one time unit
* memory is **infinite**

## Weaknesses of the model

* Real operations do not all cost the same. A disk read counts the same as an addition in the model, even though the addition can be orders of magnitude faster.
* Infinite memory ignores that memory access gets slower when more or slower memory is needed.

The model is not exact, but it is good enough to compare algorithms by how their cost grows.
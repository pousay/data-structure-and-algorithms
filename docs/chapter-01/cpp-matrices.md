# Using Matrices

Some algorithms later in the book (Chapter 10) use two-dimensional arrays, called **matrices**. The C++ library has no matrix class, but a good one is short to write. The idea is a **vector of vectors**.

## 1.7.1 Data Members, Constructor, and Accessors

```cpp
#ifndef MATRIX_H
#define MATRIX_H

#include <vector>
using namespace std;

template <typename Object>
class matrix {
public:
    matrix(int rows, int cols) : array(rows) {
        for (auto& thisRow : array) {
            thisRow.resize(cols);
        }
    }

    matrix(vector<vector<Object>> v) : array{v} {
    }

    matrix(vector<vector<Object>>&& v) : array{std::move(v)} {
    }

    const vector<Object>& operator[](int row) const {
        return array[row];
    }

    vector<Object>& operator[](int row) {
        return array[row];
    }

    int numrows() const {
        return array.size();
    }

    int numcols() const {
        return numrows() ? array[0].size() : 0;
    }

private:
    vector<vector<Object>> array;
};

#endif
```

How the constructor `matrix(int rows, int cols)` works:

1. `array(rows)` creates `rows` entries, each an empty `vector<Object>`. Parentheses are required here, so it means "size `rows`" and not "a list containing `rows`".
2. The body then resizes every row to `cols` entries.

The result looks like a 2D array. `numrows()` is the size of the outer vector. `numcols()` is the size of the first row, or `0` if there are no rows.

The two extra constructors build a matrix from an existing `vector<vector<Object>>`, by copy or by move.



## 1.7.2 `operator[]`

For a matrix `m`, `m[i]` should return row `i`, which is a `vector<Object>`. Then `m[i][j]` uses the normal vector indexing on that row.

```cpp
matrix<int> m(2, 3);
m[1][2] = 7;
cout << m[1][2] << '\n';   // 7
```

So `operator[]` returns a `vector<Object>`, not an `Object`.

### Which return style?

| Option | Verdict |
|---|---|
| By value | No. The row is large, and it is guaranteed to exist after the call. Copying wastes time. |
| By const reference only | No. Then `to[i] = from[i]` could never compile, because `to[i]` could not be assigned. |
| By plain reference only | No. Then `from[i] = to[i]` would compile even when `from` is a const matrix. |

What we need: a const reference when the matrix is const, and a plain reference when it is not.

### The loophole

Two functions cannot differ only in return type, but **const-ness of the member function is part of its signature**. So we write two versions:

```cpp
const vector<Object>& operator[](int row) const {   // accessor
    return array[row];
}

vector<Object>& operator[](int row) {               // mutator
    return array[row];
}
```

The compiler picks the const version for a const matrix and the other for a non-const one.

```cpp
void copy(const matrix<int>& from, matrix<int>& to) {
    for (int i = 0; i < to.numrows(); ++i) {
        to[i] = from[i];   // reads from const, writes to non-const
    }
}
```
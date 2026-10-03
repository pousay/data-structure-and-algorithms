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
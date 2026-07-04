---
draft: false
title: 'Mex Grid Construction'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Mex Grid Construction"
weight: 1
---

{{< problem "cses-mex-grid-construction" >}}

## Solution

We fill the grid row by row, left to right.
For each cell, we collect all values already placed to its left in the same row and above it in the same column.
The cell is assigned the **mex** (smallest non-negative integer not present in those values).

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<vector<int>> grid(n, vector<int>(n, 0));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            // Track used numbers
            vector<bool> used(2 * n, false);

            // Left in the same row
            for (int k = 0; k < j; k++) {
                used[grid[i][k]] = true;
            }

            // Above in the same column
            for (int k = 0; k < i; k++) {
                used[grid[k][j]] = true;
            }

            // Find mex
            int mex = 0;
            while (used[mex]) mex++;

            grid[i][j] = mex;
        }
    }

    // Output the grid
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << grid[i][j] << (j + 1 < n ? ' ' : "\n");
        }
    }

    return 0;
}

```

This question can actually be solved in $O(n^2)$ time instead of $O(n^3)$. We'll leave this as an exercise to you the reader. A future version for the book will contain the solution.

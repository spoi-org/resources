---
draft: false
title: 'Grid Coloring I'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Grid Coloring I"
weight: 1
---

{{< problem "cses-grid-coloring-i" >}}

## Solution

Observe that there are a maximum of 3 colours which we can't use for the current cell in our `ans` grid:

+ The current cell's original color.
+ The color of the cell above it in the `ans` grid if it exists.
+ The color of the cell to the left of it in the `ans` grid if it exists.

Now, we can assign the first available color to the `ans` grid because there will always be at least 1 valid colour. We can store a boolean array which marks all available colours `true` and then marks the illegal ones as `false`. Then assign the first `true` colour in the boolean array.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<string> v(n);        // Input grid
    char ans[n][m];             // Output grid with adjusted characters

    // Read the input grid
    for (int i = 0; i < n; i++)
        cin >> v[i];

    // Construct the output grid
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {

            // Tracks whether letters A, B, C, D are allowed at this cell
            bool isValid[4] = {true, true, true, true};

            // Block the original character in this position
            isValid[v[i][j] - 'A'] = false;

            // Block the character above (if exists)
            if (i > 0)
                isValid[ans[i - 1][j] - 'A'] = false;

            // Block the character to the left (if exists)
            if (j > 0)
                isValid[ans[i][j - 1] - 'A'] = false;

            // Choose the first valid character using your four ifs
            if (isValid[0])
                ans[i][j] = 'A';
            else if (isValid[1])
                ans[i][j] = 'B';
            else if (isValid[2])
                ans[i][j] = 'C';
            else if (isValid[3])
                ans[i][j] = 'D';
        }
    }

    // Print the final grid
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++)
            cout << ans[i][j];
        cout << "\n";
    }
}

```

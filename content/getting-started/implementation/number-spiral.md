---
draft: false
title: 'Number Spiral'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Number Spiral"
weight: 1
---

{{< problem "cses-number-spiral" >}}

## Solution

The spiral fills outward in square layers, where layer $L$ contains all cells with $max(x, y) = L$. Each layer's diagonal cell $(L, L)$ holds the value $L^2-(L-1)$ because at the $L$th layer, the value $L^2$ is at one of the edges and then to go from there to the diagonal, subtract $(L-1)$. This value serves as our anchor point.

**The key insight:**

- Even layers fill downward then leftward, while odd layers fill rightward then upward. So for even layers, if you're on the rightmost edge ($x=L$), you subtract how far down you are from the diagonal; otherwise you're on the top edge, so add how far left you are.

- Odd layers work inversely: if you're on the top edge ($y=L$), subtract your leftward distance; otherwise you're on the left edge, so add your downward distance.
This directional pattern emerges because the spiral alternates its filling direction with each layer to maintain continuity.

**Example:** $y = 5, x = 3$

<!-- Diagram: A 5×5 grid showing the number spiral for coordinates (y, x). Cell (4, 2) is highlighted in green, the anchor cell (4, 4) in yellow, and the entire 4th row/column in red to illustrate layer 5 and the walk from the anchor to (5, 3). -->

| 1 | 2 | 9 | 10 | 25 |
|---|---|---|---|---|
| 4 | 3 | 8 | 11 | 24 |
| 5 | 6 | 7 | 12 | 23 |
| 16 | 15 | 14 | 13 | 22 |
| 17 | 18 | 19 | 20 | 21 |

Here's the approach step by step:

+ As $max(5, 3) = 5$, It is on the 5th layer.

+ $L^2 - (L - 1)$ = $25 - (5 - 1)$ = $21$. 21 serves as our anchor point.

+ It is important to keep it mind that we are on an odd layer (as 5 is odd).

+ And as we have to go two cells to the left from our anchor point we subtract our leftward distance. Thus, answer is $21 - 2 = 19$.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t; // Number of test cases
    cin >> t;
    while (t--) {
        // Cell coordinates
        long long y, x;
        cin >> y >> x;
        long long layer = max(x, y); // Layer is max(x, y)
        long long val = layer * layer - layer + 1; // Base value for layer's diagonal cell

        if (layer % 2 == 0) // Even layer
            if (x == layer) // Adjust for y
                cout << val - (layer - y) << "\n";
            else // Adjust for x
                cout << val + (layer - x) << "\n";

        else // Odd layer
            if (y == layer) // Adjust for x
                cout << val - (layer - x) << "\n";
            else // Adjust for y
                cout << val + (layer - y) << "\n";
    }
    return 0;
}
```

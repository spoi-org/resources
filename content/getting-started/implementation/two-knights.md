---
draft: false
title: 'Two Knights'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Two Knights"
weight: 1
---

{{< problem "cses-two-knights" >}}

## Hint

Think of all the possible ways to arrange 2 knights and then think of how many ways are there to place 2 knights so that they attack each other. You can then subtract the two values to get the final answer.

## Solution

The problem asks: for each board size $k \\times k$ (from $k = 1$ to $k = n$), count the number of ways to place two knights such that they do not attack each other.

**Step 1: Count all possible placements**

First, we count the total number of ways to place two knights on a $k \\times k$ board without any restrictions. There are $k^2$ squares, and we need to choose 2 of them. This is simply:

$ "Total placements" = \binom(k^2, 2) = (k^2 (k^2 - 1)) / 2 $

**Step 2: Subtract attacking pairs**

Now we subtract the number of placements where the two knights attack each other. A knight attacks in an "L-shape": it moves 2 squares in one direction and 1 square perpendicular to that.

The key insight is that **two knights that attack each other always fit inside a $2 \times 3$ or $3 \times 2$ rectangle**. Within each such rectangle, there are exactly **2 pairs** of squares that attack each other (the two diagonal corners of the "L").

<!-- Diagram: Two chessboard mini-diagrams side by side, captioned "The 2 pairs of same coloured knights attack each other from the corners." The left board is 2×2 with knights on opposite corners of a 2×3 rectangle; the right board is 3×3 showing the two attacking knight pairs within a 3×2 rectangle. -->

**Step 3: Count the rectangles**

- Number of $2 \\times 3$ rectangles in a $k \\times k$ board: $(k - 1) \\times (k - 2)$
- Number of $3 \\times 2$ rectangles in a $k \\times k$ board: $(k - 2) \\times (k - 1)$

Each rectangle contains 2 attacking pairs, so:

$ "Attacking pairs" = 2 \\times (k-1)(k-2) + 2 \\times (k-2)(k-1) = 4(k-1)(k-2) $

**Final Formula:**

$ "Answer" = (k^2 (k^2 - 1)) / 2 - 4(k-1)(k-2) $

**Examples:**

- **k = 1**: Only 1 square, so 0 ways to place two knights. Answer = $0$.

- **k = 2**: Total = $\binom(4,2) = 6$ placements. No $2 \\times 3$ or $3 \\times 2$ rectangles fit, so 0 attacking pairs. Answer = $6$.

- **k = 3**: Total = $\binom(9,2) = 36$ placements. Attacking pairs = $4 \\times 2 \\times 1 = 8$. Answer = $36 - 8 = 28$.

- **k = 4**: Total = $\binom(16,2) = 120$ placements. Attacking pairs = $4 \\times 3 \\times 2 = 24$. Answer = $120 - 24 = 96$.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;

  for (long long k = 1; k <= n; ++k) {
    long long total = k * k;

    // Total ways to place 2 knights on k×k board
    long long ways = total * (total - 1) / 2;

    // Subtract attacking pairs (only exist when k > 2)
    if (k > 2)
      ways -= 4 * (k - 1) * (k - 2);

    cout << ways << "\n";
  }

  return 0;
}
```

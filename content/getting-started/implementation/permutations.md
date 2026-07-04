---
draft: false
title: 'Permutations'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Permutations"
weight: 1
---

{{< problem "cses-permutations" >}}

## Solution

The trick we exploit here is to first print all the numbers up to $n$ of one parity (odd or even), and then print all the numbers of the opposite parity. This is because the difference between consecutive odd or even numbers is always greater than $1$.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    // Special case: if n = 2 or 3, it is impossible to arrange
    // numbers from 1...n so that no two consecutive numbers
    // differ by 1. Hence, print "NO SOLUTION".
    if (n == 2 || n == 3)
        cout << "NO SOLUTION";

    // Base case: if n = 1, the only permutation is "1".
    else if (n == 1)
        cout << "1";

    // General case: n >= 4
    else {
        // First print every other number from n-1 in descending order.
        // This ensures that the gap between every number is more than 1.
        for (int i = n - 1; i >= 1; i -= 2)
            cout << i << " ";

        // Then print every other number from n in descending order.
        for (int i = n; i >= 1; i -= 2)
            cout << i << " ";
    }
}
```

Note that if you first print every other number from $n$ and then $n-1$, $n = 4$ will produce the wrong output of $4 2 3 1$ instead of $3 1 4 2$. If you do it this way just put an if statement for $n = 4$.

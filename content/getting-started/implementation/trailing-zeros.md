---
draft: false
title: 'Trailing Zeros'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Trailing Zeros"
weight: 1
---

{{< problem "cses-trailing-zeros" >}}

## Solution 

The problem asks for the number of trailing zeros in n factorial. A trailing zero occurs if a number contains a factor of 10. A factor of 10 contains a pair of 2 and 5. There will be excess number of 2s because they occur every other number vs 5s which only occur every 5th number. Therefore the number of 5s alone determine the number of zeros.

Each multiple of 5 (5, 10, 15, 20, 25…) contributes one 5. Each multiple of 25 (25, 50, 75, 100, 125...) contributes an additional 5.  Each multiple of 125 contributes another 5, and so on. The code loops through powers of 5 and counts the total number of the factor 5 present in n factorial.

For example, take $n = 27$:

- $floor(27/5) = 5$  (5's from 5, 10, 15, 20, 25).

- $floor(27/25) = 1$ (extra 5 from 25).

- $floor(27/125) = 0$ (stop).

- Total: 5 + 1 + 0 = 6 zeros.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, count = 0;
    cin >> n; // Read input number n

    // Count factors of 5 in n! by summing floor(n/5) + floor(n/25) + floor(n/125) + ...

    for (int i = 5; n / i >= 1; i *= 5) {
        count += n / i; // Add number of multiples of i (powers of 5)
    }

    cout << count << endl; // Output the number of trailing zeros
    return 0;
}

```

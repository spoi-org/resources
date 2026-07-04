---
draft: false
title: 'Apple Division'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Apple Division"
weight: 1
---

{{< problem "cses-apple-division" >}}

## Hint

Notice the low $n <= 20$. This means the approach will likely have a time complexity of $O(2^n)$ or $O(n \\cdot 2^n)$.
See  as a useful concept needed for this question.

## Solution

The problem asks you to split the apples into two groups so that their total weights differ as little as possible. By checking every subset with bitmasks, you compute the sum of one subset and compare it with the other using `abs(total - 2*sum)`. The reason for this is as follows: 

Let $a$ and $b$ be the sum of the 2 subsets. Let $t$ be the total. Then $a + b = t => b = t - a$. 
$|b - a|$ can be written as $|(t - a) - a| = |t - 2 a|$ which is the same as `abs(total - 2*sum)`.

The smallest such difference across all subsets gives the optimal answer.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> v(n);
    long long total = 0;

    // Read weights and compute total sum.
    for (int i = 0; i < n; i++) {
        cin >> v[i];
        total += v[i];
    }

    // ans = minimum possible difference between the two groups.
    long long ans = total;

    // Enumerate all subsets using bitmasks from 0 to (2^n - 1).
    // Each mask chooses some apples for the first group;
    // the rest naturally fall into the second group.
    for (int mask = 0; mask < (1 << n); mask++) {

        long long sum = 0;

        // Compute sum of elements included in this subset.
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {
                sum += v[i];
            }
        }

        // If subset sum is S, the other group's sum is total - S.
        // Difference is |(total - S) - S| = |total - 2S|.
        ans = min(ans, llabs(total - 2 * sum));
    }

    cout << ans;
}
```

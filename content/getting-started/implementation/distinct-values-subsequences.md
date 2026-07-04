---
draft: false
title: 'Distinct Values Subsequences'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Distinct Values Subsequences"
weight: 1
---

{{< problem "cses-distinct-values-subsequences" >}}

## Solution

For each distinct value with `occ` occurrences, we have `(occ + 1)` choices: exclude it (0 copies) or choose 1 of the `occ` identical copies to include.
Multiplying choices for all distinct numbers gives total possible combinations including the empty subsequence.
Subtract 1 to remove the empty subsequence case, leaving the count of all distinct-value subsequences.

**Example:**

For the array {1, 3, 5, 2, 9, 3, 2}

The frequency table stores:

| Key | Value |
|-----|-------|
| 1 | 1 |
| 2 | 2 |
| 3 | 2 |
| 5 | 1 |
| 9 | 1 |

The number of distinct value subsequences is:

$ 
&= (1 + 1) \\cdot (2 + 1) \\cdot (2 + 1) \\cdot (1 + 1) \\cdot (1 + 1)

&= 2 \\cdot 3 \\cdot 3 \\cdot 2 \\cdot 2

&= 72
$

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
const int MOD = 1e9 + 7;

int main() {

    int n;
    cin >> n;

    // Count frequency of each distinct value
    map<ll, ll> freq;
    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;
        freq[x]++;
    }

    // For each distinct value, we can include 0, 1, 2, ..., or occ copies
    // This gives (occ + 1) choices per value; multiply all choices together
    ll ans = 1;
    for (auto [num, occ] : freq) {
        ans = (ans * (occ + 1)) % MOD;
    }

    // Subtract 1 to exclude the empty subsequence
    // The reason for adding MOD before taking the modulo is because in C++, a number less than 0 can give a negative remainder which is bad for future calculation. Hence you make sure it's possible by adding MOD.
    ans = (ans - 1 + MOD) % MOD;
    cout << ans << "\n";

    return 0;
}
```

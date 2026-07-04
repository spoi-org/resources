---
draft: false
title: 'Towers'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Towers"
weight: 1
---

{{< problem "cses-towers" >}}

## Solution

The idea is to maintain the top blocks of all towers in a `multiset`. For each new block, place it on the leftmost tower whose top is strictly greater; if no such tower exists, you start a new one. This greedy strategy works because always using the smallest possible valid tower keeps future placements flexible. The number of towers equals the size of the `multiset`.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {

    int n;
    cin >> n;

    multiset<int> tops; // stores the current top element of each tower

    for (int i = 0; i < n; i++) {
        int x;
        cin >> x;

        // Find first tower whose top > x (we can place x on that tower)
        auto it = tops.upper_bound(x);// multiset<int>::iterator is the type of auto

        if (it != tops.end()) {
            // Reuse this tower: remove old top and replace with x
            tops.erase(it);
        }
        // Start a new tower or update reused one with top = x
        tops.insert(x);
    }

    // Number of towers equals the number of distinct tops
    cout << tops.size() << "\n";

    return 0;
}
```

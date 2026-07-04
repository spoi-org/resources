---
draft: false
title: 'Factory Machines'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Factory Machines"
weight: 1
---

{{< problem "cses-factory-machines" >}}

## Hint

Observe that the amount of items the machines in total produce increase consistently with time(i.e monotonically). You can also in $O(n)$ compute the amount of items the machines can make in a given amount of time. Think about how you could use this to find the minimum time to make $t$ items.

## Solution 

The key idea is to use binary-search on answers. This means you assume the answer(minimum time to make $t$ items) is some time `mid`. You then compute how many items can be made in time `mid` by summing up `mid/v[i]` for each machine. If the amount of products you made is greater than or equal to `t`, you try a smaller time by halving the range towards smaller values. If you made less than `t` products, you try a larger time by halving the range towards larger values. This guarantees we find the earliest moment when production meets the target.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {

    ll n, t;
    cin >> n >> t;

    // v[i] = time taken by machine i to produce ONE item
    vector<ll> v(n);
    for (int i = 0; i < n; i++) {
        cin >> v[i];
    }

    // Binary search on time.
    // low  = minimum possible time
    // high = a very large upper bound (1e18 works for all constraints)
    ll low = 1, high = 1e18, ans = -1;

    while (low <= high) {
        ll mid = (low + high) / 2;

        // Count how many items all machines can produce in 'mid' time
        ll total = 0;
        for (int i = 0; i < n; i++) {
            total += mid / v[i];

            // If already enough, stop early (avoid overflow + speedup)
            if (total >= t) break;
        }

        // If we can produce at least t items in 'mid' time,
        // try to find an even smaller valid time by halving the range
        if (total >= t) {
            ans = mid;
            high = mid - 1;
        }
        else {
            // Need more time, so halve the range towards larger values.
            low = mid + 1;
        }
    }

    cout << ans << "\n";
    return 0;
}
```

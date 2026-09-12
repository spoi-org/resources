---
draft: false
title: 'Increasing Array'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Increasing Array"
weight: 1
---

{{< problem "cses-increasing-array" >}}

## Solution

We need to make the given array non-decreasing: that is, every element must be at least as large as the one before it. Whenever a number is smaller than the previous one, we must increase it until the current element is at least equal to the previous element. 

We wish to know the total number of increments to satisfy the non-decreasing property. For the current element (`cur`) and the previous element (`prev`) in our array, there are 2 cases:

1. If `prev` $\le$ `cur`, then the array is non-decreasing. Hence, we don't need to increment `cur`.

2. If `prev` > `cur`, then `cur` should be at least `prev`. This takes <br> `prev - cur` increments as `cur + (prev - cur) * 1 = prev`.

Finally, we update our total number of increments for each `cur` that uses `prev - cur` increments.

Since our approach only requires the current and previous element, we can use only 2 variables (`cur` and `prev`) to store the relevant information and solve the question while accepting input.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, prev;
    long long total = 0;
    cin >> n >> prev;  // prev is arr[0]

    for (int i = 1; i < n; i++) {
        int cur;
        cin >> cur;

        if (cur < prev) {
            total += prev - cur;
            cur = prev;  // cur = (cur + (prev - cur) * 1) = prev
        }

        prev = cur;
    }

    cout << total << "\n";
    return 0;
}
```

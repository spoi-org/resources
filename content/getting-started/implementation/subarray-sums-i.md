---
draft: false
title: 'Subarray Sums I'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Subarray Sums I"
weight: 1
---

{{< problem "cses-subarray-sums-i" >}}

## Solution

Because all numbers are positive adding or removing an element to the current subarray will increase or decrease the sum respectively. This lets us use a sliding window to keep track of the current sum of the subarray. 

For every element `r`, we first increase the size of the current subarray from `l`,`r-1` to `l`,`r` and increase the sum by `v[r]`. If this sum is greater than the target `x`, we shrink the window from the left and remove those elements from the sum until the sum is less than or equal to `x`. If the sum is equal to `x` we increase our answer by 1.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    ll n, x;
    cin >> n >> x;

    vector<int> v(n);
    for (int i = 0; i < n; i++)
        cin >> v[i];

    ll sum = 0;   // current window sum
    ll ans = 0;  // number of subarrays equal to x
    ll l = 0;      // left pointer of sliding window

    for (int r = 0; r < n; r++) {
        sum += v[r]; // expand window to the right

        // shrink window from the left while sum > x
        while (sum > x) {
            sum -= v[l];
            l++;
        }

        // if current window sum = x, increase the answer by 1
        if (sum == x) ans++;
    }

    cout << ans << "\n";
}

```

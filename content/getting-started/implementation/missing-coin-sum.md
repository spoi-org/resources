---
draft: false
title: 'Missing Coin Sum'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Missing Coin Sum"
weight: 1
---

{{< problem "cses-missing-coin-sum" >}}

## Hint

## Solution

By sorting the coins in non-decreasing order, we can process them greedily. The greedy approach is as follows:

Initialize a variable `sumSoFar` to 0, representing the maximum sum we can create with the coins processed so far.

For each coin value `currCoin` :
If `currCoin` is greater than `sumSoFar + 1`, it means we cannot create the sum `sumSoFar + 1` (since all remaining coins are too large). Thus, `sumSoFar + 1` is the answer. Otherwise, add `currCoin to sumSoFar`, as we can now create all sums up to `sumSoFar + currCoin` by including or excluding `currCoin` in subsets.

If we process all coins without finding a gap, the smallest sum we cannot create is `sumSoFar + 1`.

This works because  we can create all sums from 0 to `sumSoFar`, and f the next coin `currCoin` is at most `sumSoFar + 1`, we can extend the range of creatable sums to `sumSoFar + currCoin`.

A gap occurs when a coin is too large to fill the next sum (`sumSoFar + 1`), making that sum impossible to form.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {

    int n;
    cin >> n;

    vector<long long> coins(n);
    for (int i = 0; i < n; i++) cin >> coins[i];
    sort(coins.begin(), coins.end());

    long long sumSoFar = 0;
    // We can currently form all sums from 1 to sumSoFar.

    for (long long currCoin : coins) {
        // If the next needed sum is sumSoFar + 1 and currCoin is bigger,
        // we cannot fill that gap.
        if (currCoin > sumSoFar + 1) {
            cout << sumSoFar + 1 << "\n";
            return 0;
        }

        // Otherwise, currCoin helps us extend reachable sums.
        sumSoFar += currCoin;
    }

    // If all coins processed and no gap found, next unreachable sum is sumSoFar + 1.
    cout << sumSoFar + 1 << "\n";
    return 0;
}
```

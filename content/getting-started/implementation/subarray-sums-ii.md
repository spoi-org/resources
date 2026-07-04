---
draft: false
title: 'Subarray Sums II'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Subarray Sums II"
weight: 1
---

{{< problem "cses-subarray-sums-ii" >}}

## Solution

We iterate through the array while maintaining a prefix sum. A map stores how many times each prefix sum has appeared so far. At each position, if a previous prefix sum equals `currentSum` − `targetSum`, a subarray with sum `targetSum` exists(The subarray start 1 place after the previous prefix sum). We add the frequency of how often that previous prefix sum value occurred to the answer and then increase the frequency of the value of the current prefix sum.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;

int main() {
    ll n, targetSum;
    cin >> n >> targetSum;

    vector<ll> array(n);
    for (int i = 0; i < n; i++) {
        cin >> array[i];
    }

    // prefixSumCount[s] = how many times prefix sum with value s has occurred
    map<ll, ll> prefixSumCount;
    prefixSumCount[0] = 1;   // empty prefix

    ll currentSum = 0;
    ll subarrayCount = 0;

    for (int i = 0; i < n; i++) {
        currentSum += array[i];

        // count subarrays ending here with sum = targetSum
        subarrayCount += prefixSumCount[currentSum - targetSum];

        // increase the frequency of the value of the current prefix sum
        prefixSumCount[currentSum]++;
    }

    cout << subarrayCount << "\n";
    return 0;
}
```

---
draft: false
title: 'Maximum Subarray Sum II'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Maximum Subarray Sum II"
weight: 1
---

{{< problem "cses-maximum-subarray-sum-ii" >}}

## Solution

We can first build a prefix sum array so any subarray sum can be computed in O(1). A multiset stores prefix sums such that if you subtract the prefix sums in the multiset from the prefix sum at the current index `right`, you get the sums of subarrays with lengths `minLen` to `maxLen` ending at `right`. As `right` moves forward, new valid prefixes are added to the set. Prefixes that would make the subarray longer than b ending a `right` are removed. The smallest prefix in the `multiset`(`valid.begin()`) gives the maximum possible subarray ending at `right`. The answer is updated by comparing all such valid subarrays.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, minLen, maxLen;
    cin >> n >> minLen >> maxLen;

    vector<long long> values(n);
    for (int i = 0; i < n; i++) {
        cin >> values[i];
    }

    // Prefix sums: prefix[i] = sum of first i elements
    vector<long long> prefix(n + 1, 0);
    for (int i = 0; i < n; i++) {
        prefix[i + 1] = prefix[i] + values[i];
    }

    multiset<long long> valid;
    long long bestSum = LLONG_MIN;

    for (int right = minLen; right <= n; right++) {
        // Add the prefix sum corresponding to a subarray of length >= minLen
        valid.insert(prefix[right - minLen]);

        // Remove the prefix sum that would make subarray length > maxLen
        if (right > maxLen) {
            valid.erase(valid.find(prefix[right - maxLen - 1]));
        }

        // Best subarray ending at 'right'
        bestSum = max(bestSum, prefix[right] - *valid.begin());
    }

    cout << bestSum << "\n";
    return 0;
}

```

= Dynamic Programming (WIP)
= Graph Algorithms (WIP)
= Range Queries (WIP)

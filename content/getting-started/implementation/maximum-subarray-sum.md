---
draft: false
title: 'Maximum Subarray Sum'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Maximum Subarray Sum"
weight: 1
---

{{< problem "cses-maximum-subarray-sum" >}}

## Solution

The algorithm finds the maximum possible sum of a continuous sequence in an array. It begins by assuming the first element is the best sum. Then, as it moves through the array, it decides whether to add the current element to the current sum or start the sum fresh from the current number. At each step, it updates the overall best sum found so far, ensuring the final answer is the largest contiguous total.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<long long> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }

    // max_current = maximum subarray sum ending at the current index
    // max_global = overall maximum subarray sum found so far
    long long max_current = arr[0];
    long long max_global = arr[0];

    // Iterate through the array
    for (int i = 1; i < n; i++) {
        // Either start a new subarray at arr[i] or extend the existing one
        max_current = max(arr[i], max_current + arr[i]);

        // Update the global maximum if needed
        max_global = max(max_global, max_current);
    }

    // Output the maximum subarray sum
    cout << max_global << endl;
    return 0;
}
```

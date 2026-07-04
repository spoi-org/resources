---
draft: false
title: 'Sum of Two Values'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Sum of Two Values"
weight: 1
---

{{< problem "cses-sum-of-two-values" >}}

## Solution

The algorithm sorts all numbers, then uses two pointers, one starting at the smallest and one at the largest value, to find a pair that sums to the target. If the sum is too small, the left pointer moves right to a larger number; if too large, the right pointer moves left to a smaller number.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, target;
    cin >> n >> target;

    // Store each number along with its original index
    vector<pair<int, int>> nums(n);
    for (int i = 0; i < n; i++) {
        // number value
        cin >> nums[i].first;

        // original position (1-based index)
        nums[i].second = i + 1;
    }

    // Sort numbers by value to apply two-pointer technique
    sort(nums.begin(), nums.end());

    int left = 0, right = n - 1;
    while (left < right) {
        int sum = nums[left].first + nums[right].first;

        // If target sum found, print their original indices
        if (sum == target) {
            cout << nums[left].second << " " << nums[right].second;
            return 0;
        }
        // Move pointers based on comparison with target
        else if (sum < target) 
          left++;// make the sum larger
        else 
          right--;// make the sum smaller
    }

    // If no valid pair found
    cout << "IMPOSSIBLE";
    return 0;
}
```

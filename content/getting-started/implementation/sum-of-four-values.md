---
draft: false
title: 'Sum of Four Values'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Sum of Four Values"
weight: 1
---

{{< problem "cses-sum-of-four-values" >}}

## Solution

This question is an extension of Sum of Three Values. We can pick two numbers `v[i]`,  `v[j]` then seeing if there are any remainder 2 values `v[l]`, `v[r]` in the range `i+1` to `n-1` such that `v[i] + v[l] + v[r] = target`. The remainder 2 values are found by using the same 2 pointer approach as shown in the previous question.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, target;
    cin >> n >> target;

    // Store (value, original_index)
    vector<pair<int, int>> nums(n);

    for (int i = 0; i < n; i++) {
        cin >> nums[i].first;
        nums[i].second = i + 1;   // 1-based indexing as required by CSES
    }

    // Sort by value to enable two-pointer scanning later
    sort(nums.begin(), nums.end());

    // Fix first two values using indices i and j
    for (int i = 0; i < n - 3; i++) {
        for (int j = i + 1; j < n - 2; j++) {

            int left = j + 1;
            int right = n - 1;

            // Two-pointer search for remaining pair
            while (left < right) {
                long long sum = nums[i].first
                + nums[j].first
                + nums[left].first
                + nums[right].first;

                if (sum == target) {
                    // Output original positions
                    cout << nums[i].second << " "
                    << nums[j].second << " "
                    << nums[left].second << " "
                    << nums[right].second;
                    return 0;
                }

                // Adjust pointers based on sum size
                if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }
    }

    cout << "IMPOSSIBLE";
    return 0;
}

```

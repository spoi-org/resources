---
draft: false
title: 'Sum of Three Values'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Sum of Three Values"
weight: 1
---

{{< problem "cses-sum-of-three-values" >}}

## Solution

This question is an extension of Sum of Two Values. We can pick one number `v[i]` and then seeing if there are any remainder 2 values `v[l]`, `v[r]` in the range `i+1` to `n-1` such that `v[i] + v[l] + v[r] = target`. The remainder 2 values are found by using the same 2 pointer approach as shown in the previous question.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, target;
    cin >> n >> target;

    // Store {value, original_index}
    vector<pair<int, int>> v(n);

    for (int i = 0; i < n; i++) {
        cin >> v[i].first;
        v[i].second = i + 1;  // keep original 1-based index
    }

    // Sort by value so we can use the two-pointer trick
    sort(v.begin(), v.end());

    // Fix one element v[i], then find two others using two pointers
    for (int i = 0; i < n - 2; i++) {
        int l = i + 1;       // left pointer
        int r = n - 1;       // right pointer

        while (l < r) {
            int sum = v[i].first + v[l].first + v[r].first;

            if (sum == target) {
                // Output original positions (not sorted ones)
                cout << v[i].second << " " << v[l].second << " " << v[r].second;
                return 0;
            }
            else if (sum < target) {
                l++;         // need a larger sum -> move left pointer right
            }
            else {
                r--;         // need a smaller sum -> move right pointer left
            }
        }
    }

    // If no triple found
    cout << "IMPOSSIBLE";
    return 0;
}

```

---
draft: false
title: 'Array Division'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Array Division"
weight: 1
---

{{< problem "cses-array-division" >}}

## Hint

If you were given a test sum `s`, you could find out how many subarrays you would need such that the sum of each subarray would be at most `s`. Could you use this information to find the largest `s` such that you only divide the array `k` times?

## Solution

We can use binary search on answers to solve this question. We assume the answer lies in the range `left` to `right`. We then guess the sum `mid = (l + r)/2`, we then check if we can split the array into `k` subarrays such that the sum of each subarray is at most `mid`. If it's possible, you can then attempt a lower sum in half the range `left` to `mid-1`. If it's not possible, you can then attempt a higher sum in half the range `mid+1` to `right`. Repeat till you find the smallest sum.   

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n);

    ll left = 0, right = 0;
    // 'left' = minimum possible max sum (largest single element)
    // 'right' = maximum possible max sum (sum of all elements)

    for (int i = 0; i < n; i++) {
        cin >> a[i];
        left = max(left, (ll)a[i]);
        right += a[i];
    }

    ll ans = right;   // Best answer found via binary search

    while (left <= right) {
        ll mid = (left + right) / 2;

        // Try to split into <= k subarrays where no subarray sum exceeds 'mid'
        int subarrays = 1;
        ll current_sum = 0;
        bool possible = true;

        for (int i = 0; i < n; i++) {
            // If adding this element exceeds 'mid', start a new subarray
            if (current_sum + a[i] > mid) {
                subarrays++;
                current_sum = a[i];

                // Too many subarrays means that mid is too small
                if (subarrays > k) {
                    possible = false;
                    break;
                }
            } 
            else //Add the current element to this subarray's sum
                current_sum += a[i];
        }
        //you could split the array into k subarray with their sum being at most mid
        //so try values that are smaller than mid to get a better answer
        if (possible) {
            ans = mid;        
            right = mid - 1;
        } 
        else //you couldn't split the array into k subarray with their sum being at most mid
            left = mid + 1; // so try values that are larger than mid.
    }

    cout << ans << "\n";
    return 0;
}

```

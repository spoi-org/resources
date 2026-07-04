---
draft: false
title: 'Distinct Values Subarrays II'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Distinct Values Subarrays II"
weight: 1
---

{{< problem "cses-distinct-values-subarrays-ii" >}}

## Hint

If you've solved Distinct Values Subarrays, try using the same method but modifying it for the needs of this question.

## Solution

We can use a sliding window while storing the frequencies of every element in a `map`. When you expand the right end of the window, check to see if the number of distinct elements has gone up. If that number exceeds `k`, shrink the window from the left until the distinct elements are at most `k`. Increase the answer by the length of the window because that's how many subarrays have at most `k` distinct values ending at the right endpoint.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    long long n, k;
    cin >> n >> k;

    vector<long long> v(n);
    for (int i = 0; i < n; i++) {
        cin >> v[i];
    }

    map<long long, int> freq; // frequency of each element in current window
    long long left = 0, ans = 0, distinct = 0;

    for (int right = 0; right < n; right++) {
        // add right element to window
        if (++freq[v[right]] == 1) {//If it's a new element, increase the number of distinct elements.
            distinct++;
        }

        // shrink window until we have at most k distinct elements
        while (distinct > k) {
            freq[v[left]]--;
            if (freq[v[left]] == 0) {//if an element is no longer present in the window, decrease the number od distinct elements
                distinct--;
            }
            left++;
        }

        // all subarrays ending at right with start in [left, right] are valid
        ans += (right - left + 1);
    }

    cout << ans << "\n";
}
```

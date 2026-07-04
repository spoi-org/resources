---
draft: false
title: 'Increasing Array'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Increasing Array"
weight: 1
---

{{< problem "cses-increasing-array" >}}

## Solution

We need to make the given array non-decreasing: that is, every element must be at least as large as the one before it. Whenever a number is smaller than the previous one, we must increase it until the condition `a[i]` ≥ `a[i−1]` holds. The problem asks for the total number of increments required to achieve this.

Here's the approach step by step:

+ Read the first element and store it as prev.
+ Iterate through the rest of the array.
+ If current $>=$ prev, move on because the order is fine.
+ If current $<$ prev, we need to increase it by (prev − current) so that the order is ascending.
+ Add this difference to the total count and update prev and current.
+ Continue until all elements are processed.
+ Output the total count of increments required.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, prev;
    long long operations = 0; // total number of increments needed
    cin >> n >> prev;

    // process the rest of the array
    for (int i = 1; i < n; ++i) {
        int current;
        cin >> current;  // read the current element

        // if the current element is smaller than the previous,
        // we need to increment it to match 'prev' (to keep array non-decreasing)
        if (current < prev) {
            // count how many increments are required
            operations += prev - current;
            // simulate the increment (virtually update current)
            current = prev;
        }

        prev = current; // update 'prev' for the next iteration
    }

    cout << operations << "\n";
    return 0;
}
```

---
draft: false
title: 'Ferris Wheel'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Ferris Wheel"
weight: 1
---

{{< problem "cses-ferris-wheel" >}}

## Solution

The algorithm sorts all weights, then uses two pointer, one at the lightest and one at the heaviest person, to form pairs without exceeding the limit. If they can share a gondola, both of them ride it; otherwise, the heavier one goes alone. This greedy pairing minimizes the total number of gondolas.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, x;
    cin >> n >> x;

    vector<int> weights(n);
    for (int i = 0; i < n; i++) {
        cin >> weights[i];
    }

    // Sort the weights
    sort(weights.begin(), weights.end());

    int gondolas = 0;
    int left = 0, right = n - 1;

    while (left <= right) {
        // If heaviest and lightest can share a gondola
        if (weights[left] + weights[right] <= x) {
            left++;//go to the next lightest
            right--;//go to the next heaviest
        }
        // Otherwise, heaviest gets their own gondola
        else {
            right--;
        }
        gondolas++;
    }

    cout << gondolas << endl;

    return 0;
}
```

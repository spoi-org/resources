---
draft: false
title: 'Concert Tickets'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Concert Tickets"
weight: 1
---

{{< problem "cses-concert-tickets" >}}

## Solution

Store all ticket prices in a `multiset` to keep them sorted and allow duplicates. Each customer gives an offer, and you use `upper_bound()` to find the first price strictly greater than that offer, then step one step back to get the best affordable ticket. If such a ticket exists, print it and remove it; otherwise print –1. This algorithm neatly handles each request without iterating through the whole list.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m, input;
    cin >> n >> m;

    // Multiset to store ticket prices in sorted order
    multiset<int> prices;

    // Store the m offers from customers
    vector<int> offers(m);

    // Insert the n ticket prices into the multiset
    for (int i = 0; i < n; i++) {
        cin >> input;
        prices.insert(input);
    }

    // Read all offers
    for (int i = 0; i < m; i++)
        cin >> offers[i];

    // For each offer, try to find the best possible ticket
    for (int i = 0; i < m; i++) {

        // Find the first price strictly greater than the offer
        auto it = prices.upper_bound(offers[i]);//auto gets set to multiset<int>::iterator

        // If upper_bound points to begin(), no ticket <= offer exists
        if (it == prices.begin())
            cout << "-1" << endl;
        else {
            // Move iterator to the largest price <= offer
            --it;

            // Output that price
            cout << *it << endl;

            // Remove that ticket so it can't be reused
            prices.erase(it);
        }
    }

    return 0;
}
```

---
draft: false
title: 'Traffic Lights'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Traffic Lights"
weight: 1
---

{{< problem "cses-traffic-lights" >}}

## Solution

The program simulates cutting a stick of length `a` at `b` given positions. It uses a `set` to store all the positions of the traffic lights and a `multiset` to track road segment lengths without a traffic light. After each cut that a traffic light makes, it removes the old segment and adds two new ones. Finally, it prints the length of the largest segment remaining after each cut.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int a, b, x;
    cin >> a >> b;
    
    set<int> trafficLights; // trafficLights stores the positions of the traffic Lights
    multiset<int> lengths; //  lengths stores all the road segment lengths without a traffic light
    trafficLights.insert(a); // rightmost boundary
    trafficLights.insert(0); // leftmost boundary
    lengths.insert(a); // initially one segment of length 'a'

    for (int i = 0; i < b; i++) {
        cin >> x;

        // Insert the new cut position and find its neighbors
        auto mid = trafficLights.insert(x);//auto is set<int>::iterator
        auto first = prev(mid);//auto is multiset<int>::iterator
        auto last = next(mid);//auto is multiset<int>::iterator

        // Remove the old segment and add the two new smaller segments
        lengths.erase(lengths.find(**last - **first));
        lengths.insert(**last - **mid);
        lengths.insert(**mid - **first);

        // Output the largest segment length after each cut
        cout << *lengths.rbegin() << " ";//rbegin() is a reverse iterator at the last element.
    }
}
```

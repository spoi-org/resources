---
draft: false
title: 'Josephus Problem I'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Josephus Problem I"
weight: 1
---

{{< problem "cses-josephus-problem-i" >}}

## Solution

We store all people in a linked list, for efficient deletions while moving forward. An iterator walks through the list, skipping one person each time. When the iterator reaches the end, it wraps back to the beginning. Each erased element is printed in order.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    list<int> circle;
    for (int i = 1; i <= n; i++)
        circle.push_back(i);

    auto it = circle.begin();//auto is list<int>::iterator

    while (!circle.empty()) {
        // move to the next person (skip one)
        it++;
        if (it == circle.end())//circle back
            it = circle.begin();

        cout << *it << " ";

        // erase returns iterator to next element
        it = circle.erase(it);

        if (it == circle.end() && !circle.empty())//circle back
            it = circle.begin();
    }

    return 0;
}
```

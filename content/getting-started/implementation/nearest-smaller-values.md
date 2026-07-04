---
draft: false
title: 'Nearest Smaller Values'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Nearest Smaller Values"
weight: 1
---

{{< problem "cses-nearest-smaller-values" >}}

## Solution

We can use a stack of pairs (value, index) that stores the previously seen values that are less than the current value `x` in ascending order. This is achieved by first popping all elements greater than `x` and then either outputting the index at the top of the stack (`s.top.second`) or if the stack is empty, outputting zero. Finally push `x` into the stack.

The index at the top of the stack is guaranteed to be the closest value less than than `x` because any other values that are less than `x` which occurred earlier than the answer were pushed in first and hence will be lower in the stack than the answer.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ll n, a;
  cin >> n;
  stack<pair<int,int>> s;  // Stores pairs of (value, index) in sorted order by value

  for (int idx = 1; idx <= n; idx++) {
    cin >> x;

    while(!s.empty() && s.top().first >= x)//remove all elements >= x
      s.pop(); 

    if(s.empty())//if there are no elements < x
      cout << 0 << " ";
    else //output the element with the idx closest to x i.e the top of the stack
      cout << s.top().second << " ";

    s.push({x,idx});
  }

  return 0;
}
```

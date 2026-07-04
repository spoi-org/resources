---
draft: false
title: 'Nested Ranges Check'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Nested Ranges Check"
weight: 1
---

{{< problem "cses-nested-ranges-check" >}}

## Hint

Try sorting the intervals in ascending order of left index and descending order of right index. What algorithm could you come up with where you only have to iterate once through the list to get the answer? Think about why this sorting method was mentioned and what advantages it has. 

## Solution

For detecting if a range is "contained" by another range, iterate in the forward direction (left to right) tracking the maximum right endpoint of a range seen so far. If the current range's right endpoint is less than or equal to the maximum, it means this range is contained by a previous one because the previous ranges will also have a left endpoint less than the current.

For detecting if a range "contains" another range, iterate backwards (right to left) tracking the minimum right endpoint. If the current range's right endpoint is greater than or equal to the minimum, it contains at least one subsequent range because the current ranges left endpoint will be lesser than the subsequent one.

The time complexity is $O(n log n)$.

For a deeper explanation to the problem, see the solution to the next question.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Range{
	int l, r, idx; 

  bool operator<(const Range& ran){//Operator overloading so we can get the correct sorting order.
    return l < ran.l || l == ran.l && r > ran.r;
  }
};

int main() {

    int n;
    cin >> n;
    vector<Range> ranges(n);
    for (int i = 0; i < n; i++) {
        cin >> ranges[i].l >> ranges[i].r; 
        ranges[i].idx = i;
    }

    sort(ranges.begin(), ranges.end());
    vector<int> contains(n, 0), contained(n, 0);

    int maxRight = INT_MIN;
    for (int i = 0; i < n; i++) {
        if (ranges[i].r <= maxRight)
            contained[ranges[i].idx] = 1;
        maxRight = max(maxRight, ranges[i].r);
    }
    int minRight = INT_MAX;
    for (int i = n - 1; i >= 0; i--) {
        if (ranges[i].r >= minRight)
            contains[ranges[i].idx] = 1;
        minRight = min(minRight, ranges[i].r);
    }
    for (int x : contains) 
      cout << x << " ";
    cout << "\n";

    for (int x : contained) 
      cout << x << " ";
    cout << "\n";
}
```

---
draft: false
title: 'Stick Lengths'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Stick Lengths"
weight: 1
---

{{< problem "cses-stick-lengths" >}}

## Hint

Think about what value minimizes the sum of absolute distances from all points. Consider a number line with all stick lengths plotted on it. If you need to pick one point that minimizes the total distance to all other points, what mathematical property does that point have?

## Solution

The key insight is that the median minimizes the sum of absolute deviations. When we need to make all sticks equal length, we're essentially finding a target value that minimizes the total cost, where cost is the absolute difference between each stick's current length and the target.

Why the median? Consider what happens when we move the target value slightly:

If we move the target to the right by 1 (increase it), all sticks shorter than the target need to be lengthened by 1, while sticks longer than the target need to be shortened by 1.

Example: Consider sticks with lengths [2, 3, 5, 8, 9] (already sorted). The median is 5.
- At median (target = 5): Cost = $|2-5| + |3-5| + |5-5| + |8-5| + |9-5|$ \ $ = 3 + 2 + 0 + 3 + 4 =$ **12**
- Move right (target = 6): Cost = $|2-6| + |3-6| + |5-6| + |8-6| + |9-6| = 4 + 3 + 1 + 2 + 3 =$ **13**
  - The 3 sticks below the median (2, 3, 5) each cost +1 more = +3 total
  - The 2 sticks above the median (8, 9) each cost -1 less = -2 total
  - Net change: +3 - 2 = +1 (cost increased!)
- Move left (target = 4): Cost = $|2-4| + |3-4| + |5-4| + |8-4| + |9-4|$ \ $ = 2 + 1 + 1 + 4 + 5 =$ **13**
  - The 2 sticks below the median (2, 3) each cost -1 less = -2 total
  - The 3 sticks above the median (5, 8, 9) each cost +1 more = +3 total
  - Net change: -2 + 3 = +1 (cost increased!)

Notice that moving away from the median in either direction increases the total cost because there are more sticks on one side that get penalized than sticks on the other side that benefit.

The algorithm works as follows:
- Sort the array to easily access the median
- Choose the middle element (for odd $n$) or either middle element (for even $n$) as the target
- Calculate the sum of absolute differences between each stick and the target

For even-length arrays, any value between the two middle elements works equally well, but using one of the middle elements is simplest.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> v(n);
    for (int i = 0; i < n; i++)
        cin >> v[i];

    // Sort the array in ascending order
    sort(v.begin(), v.end());

    // Choose the middle element (median) as the central value
    int c = v[v.size() / 2];

    long long ans = 0;
    // Calculate the total distance of all elements from the median
    for (int i = 0; i < n; i++)
        ans += abs(v[i] - c);

    // Output the minimum total distance
    cout << ans;
}
```

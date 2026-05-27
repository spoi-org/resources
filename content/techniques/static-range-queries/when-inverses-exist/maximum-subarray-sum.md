---
draft: false
title: 'Maximum subarray sum'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Maximum Subarray Sum"
weight: 6
---

{{< problem "cses-maximum-subarray-sum" >}}

## Abridged problem statement

Given an array $a$ of length $n$, find the maximum value of $\sum_{i=l}^{r} a[i]$ over all pairs $(l,r)$ with $l\le r$. In other words, find the maximum subarray sum.

## Solution

Define $p[i] = \sum_{j\le i} a[j]$, with $p[-1]=0$. In other words, $p$ is the prefix sum array of $a$. Then, for a given $(l, r)$, the subarray sum is $p[r] - p[l-1]$.

Now, let us fix $r$ (equivalently, we could have also fixed $l$, but fixing $r$ is more intuitive). To maximize $p[r]-p[l-1]$ for a fixed $r$, we must minimize $p[l-1]$. So we must find $\min_{-1\lt i<l}p[i]$, and we can just maintain this as we iterate $r$ from left to right. 

## Code

```cpp
#include <bits/stdc++.h>

using namespace std;

using int64 = long long;

int main() {
  int n;
  cin >> n;
  vector<int> a(n);
  vector<int64> p(n);
  for (int i = 0; i < n; ++i) {
    cin >> a[i];
    p[i] = a[i];
    if (i != 0) {
      p[i] += p[i - 1];
    }
  }

  int64 min_prefix = 0;
  int64 ans = int64(-1e15);
  for (int r = 0; r < n; ++r) {
    ans = max(ans, p[r] - min_prefix);
    min_prefix = min(min_prefix, p[r]);
  }

  cout << ans << '\n';
}
```
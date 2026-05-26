---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'CSES: Array Division'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Array Division"
weight: 2
---

{{< problem "cses-array-division" >}}

## Abridged problem statement
> Given an array $A$ containing $n$ positive integers, divide it into $k$ subarrays minimizing the maximum subarray sum.

Again, let's fix the maximum subarray sum. Let $f(x)$ be $1$ if there exists a valid division of $A$ into $k$ subarrays with each one of them having a sum $\le x$, and $0$ otherwise.

Notice that a division that is valid for $x$ will also be valid for $x+1$. Thus, in this case, $f(x)$ will look like this:

$\begin{array}{c|cccccccccccc}
  x & 0 & 1 & 2 & \cdots & t-1 & t & t+1 & t+2 & \cdots \\
  \hline
  f(x) & 0 & 0 & 0 & \cdots & 0 & 1 & 1 & 1 & \cdots \\
\end{array}$

To check whether a division for $x$ exists, we'd iterate over the elements of the array. Ideally, we want to use the least number of subarrays as possible, since if we end up using $<k$, we can always just split up one of the existing ones into two different subarrays and this will only ever improve our answer. Notice that this means that we want each subarray to be as big as possible.

To begin with, if there exists an element in our array which is larger than $x$, the answer is immediately $0$.

If this is not the case, we can keep expanding our current subarray until its sum exceeds $x$. At the end, we count how many 'splits' we've made, which will correspond to one less than the number of subarrays.

The lower bound on the answer is $0$, and the upper bound is $\sum a_i$.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

typedef long long ll;

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  ll n, k;
  std::cin >> n >> k;
  std::vector<ll> a(n);
  for (auto &i : a) {
    std::cin >> i;
  }

  ll max_e = *std::max_element(a.begin(), a.end());
  ll sum = std::accumulate(a.begin(), a.end(), 0LL);

  auto f = [&](ll x) {
    if (max_e > x) {
      return false;
    }
    ll splits_made = 0;
    for (ll i = 0, current_sum = 0; i < n; ++i) {
      current_sum += a[i];
      if (current_sum > x) {
        splits_made++, current_sum = a[i];
      }
    }
    return splits_made <= k - 1;
  };

  ll l = 0, r = sum;
  ll ans = sum;
  while (l <= r) {
    ll m = std::midpoint(l, r);
    if (f(m)) {
      ans = m;
      r = m - 1;
    } else {
      l = m + 1;
    }
  }
  std::cout << ans << '\n';
}
```
</details>
---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'CSES: Multiplication Table'
editorial:
  platform: "CSES"
  category: "Additional Problems I"
  name: "Multiplication Table"
weight: 3
---

{{< problem "cses-multiplication-table" >}}

## Abridged problem statement
> Find the median element of the multiset $S=\{ab; 1 \le a,b \le n\}$.

The condition for an element $x$ being the median (assuming it exists in the multiplication table) is that there should be exactly $\lfloor\frac{n^2}{2}\rfloor$ numbers to the left (and right, but one of these conditions alone is sufficient) of it.

How do we count how many smaller numbers lie to the left? Each number will be able to be represented as a product of two numbers $\le n$, so let's fix one of these. Now the other number can lie between $[1, \frac{x}{i})$. All we need to do is sum up the cardinalities of the ranges of the second number over all possibilities for the first number.

Now the problem reduces to finding the largest number with $\le \lfloor\frac{n^2}{2}\rfloor$ numbers to its left. We can, as you could've guessed, use binary search to do this.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

typedef long long ll;

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  ll n;
  std::cin >> n;

  auto elements_to_left = [&](ll x) {
    ll ans = 0;
    for (ll i = 1; i <= n; ++i) {
      ans += std::min(x / i - (x % i == 0), n);
    }
    return ans;
  };

  ll l = 1, r = n * n;
  ll ans = 1;
  while (l <= r) {
    ll m = std::midpoint(l, r);
    if (elements_to_left(m) <= n * n / 2) {
      ans = m;
      l = m + 1;
    } else {
      r = m - 1;
    }
  }
  std::cout << ans << '\n';
}
```
</details>
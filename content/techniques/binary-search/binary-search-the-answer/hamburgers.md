---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Codeforces: Hamburgers'
editorial:
  platform: "Codeforces"
  name: "Hamburgers"
weight: 1
---

{{< problem "codeforces-hamburgers" >}}

## Abridged problem statement
> Find the maximum number of hamburgers you can make given that one hamburger requires $b$ bread, $s$ sausage, and $c$ cheese pieces. You initially have $n_b, n_s, n_c$ pieces of bread, sausage, and cheese respectively, and can buy more at $p_b, p_s, p_c$ rubles per piece. You have $r$ rubles in total to spend.

Let us define $f(x)$ as:

$$
f(x)=\begin{cases} 
1 & \text{if we can make } x \text{ burgers} \\
0 & \text{otherwise}
\end{cases}
$$

Here's how the return values of $f(x)$ will look:

$$
\begin{array}{c|ccccccccc}
x & 0 & 1 & 2 & 3 & \dots & t & t+1 & t+2 & \dots \\
\hline
f(x) & 1 & 1 & 1 & 1 & \dots & 1 & 0 & 0 & \dots \\
\end{array}
$$

Now our task simply reduces to finding $t$, that is, finding the largest value of $x$ such that we can make $x$ burgers (such that $f(x)=1$), and we can use binary search for this.

The only two things left to do are to bound the answer from both directions, and to code up $f(x)$. Let us tackle them sequentially.

The lower bound is obviously $0$. For an upper bound, consider $\max\{b,s,c\}=1$ with $b+s+c=1$, $n_b=n_s=n_c=100,p_b=p_s=p_c=1,r=10^{12}$. In this case, the maximum number of burgers we can buy is $10^{12}+100$, which is our upper bound.

Next, to code $f(x)$, we'll need to have $x \cdot b,x \cdot s,x \cdot c$ pieces of ingredients in total. If we have less than this, buy the rest and keep track of how much money you use in doing so. If this amount turns out to be $\le r$, return $1$, otherwise, return $0$.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

typedef long long ll;

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  std::string recipe;
  std::cin >> recipe;
  ll b = 0, s = 0, c = 0;
  for (auto &i : recipe) {
    b += i == 'B';
    s += i == 'S';
    c += i == 'C';
  }
  ll nb, ns, nc, pb, ps, pc, rubles;
  std::cin >> nb >> ns >> nc >> pb >> ps >> pc >> rubles;

  auto f = [&](ll x) {
    return std::max(x * b - nb, 0LL) * pb + 
           std::max(x * s - ns, 0LL) * ps +
           std::max(x * c - nc, 0LL) * pc <= rubles;
  };

  ll l = 0, r = 1e12 + 100;
  ll ans = 0;
  while (l <= r) {
    ll m = std::midpoint(l, r);
    if (f(m)) {
      ans = m, l = m + 1;
    } else {
      r = m - 1;
    }
  }
  std::cout << ans << '\n';
}
```
</details>
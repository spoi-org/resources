---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Codeforces: Invertible Bracket Sequences'
editorial:
  platform: "Codeforces"
  name: "Invertible Bracket Sequences"
weight: 2
---

{{< problem "codeforces-invertible-bracket-sequences" >}}

## Abridged problem statement
> Given a regular bracket sequence $s$, find the number of pairs $(i, j)$ where $i \le j$, such that flipping the parentheses (i.e., changing '(' to ')' and vice versa) in the substring from $i$ to $j$ results in a valid regular bracket sequence.

We can define a regular bracket sequence as follows:
- The empty string is a regular bracket sequence.
- If $s$ is a regular bracket sequence, then so is $(s)$.
- If $s$ and $t$ are two regular bracket sequences, then $st$ is also a regular bracket sequence.

Here's a more intuitive way to think about this. Each time you encounter an opening bracket ‘(’, a matching closing bracket ‘)’ will follow later. At any point in the sequence, the number of closing brackets should never exceed the number of opening brackets. By the end, all opening brackets should be matched with corresponding closing ones.

Notice that the two conditions outlined in the paragraph are necessary and sufficient to ensure that a sequence of parentheses is a regular bracket sequence.

Let's fix the starting point ($i$) of the portion we are going to invert. To begin with, we need to satisfy the first condition: that is, the number of closed parentheses shouldn't exceed the number of open ones. Assume that we end our substring at index $j$. Then our condition becomes:

$\max\{\text{pref}[i...j]\} - \text{pref}[i-1] \le \text{pref}[i-1]$

Here, we've defined $\text{pref}[i]$ as the sum from indices $0$ to $i$, considering '(' as $1$ and ')' as $-1$.

Notice that this condition is monotonic: that is, if it's true for $j$, it's also guranteed that it's true for $j-1$ and if it's false for $j$ then it'll also be false for $j+1$. This means that we can apply binary search to find $j$!

But we aren't done yet. Now we have a range of indices $[i,j]$ that satisfy the first condition. The second condition is that the number of opening and closed parentheses should remain balanced. A pair $(u,v)$ will only be valid if:

$\text{pref}[v] - \text{pref}[u-1]=0$

So now we have to tackle the task of finding the number of indices in $v\in[i,j]$ satisfying $\text{pref}[v]=\text{pref}[i-1]$. We can yet again use binary search to do this by storing the elements of $\text{pref}$ in an `std::vector` of `std::pair<int, int>`'s, storing the sum in the first element of the pair and the index in the second one.

We sort this, prioritizing the first pair element, and only sorting by the second one in case of equality of the first. This means that all equal sums will be grouped together, and among these groups, indices will be sorted. So we can just apply binary search on this again.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

typedef long long ll;

void solve() {
  std::string s;
  std::cin >> s;
  ll n = s.length();
  s = " " + s;

  std::vector<std::pair<ll, ll>> elements = {{0, 0}};
  std::vector<ll> sum(n + 1);
  for (ll i = 1; i <= n; ++i) {
    sum[i] = sum[i - 1] + (s[i] == '(' ? 1 : -1);
    elements.push_back({sum[i], i});
  }
  std::sort(elements.begin(), elements.end());

  auto num_equal = [&](ll s, ll e, ll x) {
    std::pair<ll, ll> p1 = {x, s};
    std::pair<ll, ll> p2 = {x, e};
    auto it1 = std::lower_bound(elements.begin(), elements.end(), p1);
    auto it2 = std::upper_bound(elements.begin(), elements.end(), p2);
    if (it1 == elements.end() || it2 == elements.begin()) {
      return 0LL;
    }
    return ll(it2 - it1);
  };

  std::vector<std::array<ll, 20>> g(n + 1);
  for (ll i = 1; i <= n; ++i) {
    g[i][0] = sum[i];
  }
  for (ll j = 1; j < 20; ++j) {
    for (ll i = 1; i <= n; ++i) {
      g[i][j] = std::max(g[i][j - 1], g[std::min(i + (1 << (j - 1)), n)][j - 1]);
    }
  }

  ll ans = 0;
  for (ll i = 1; i <= n; ++i) {
    ll idx = i, cur = sum[i];
    for (ll j = 19; j >= 0; --j) {
      if (idx + (1 << j) > n) {
        continue;
      }
      ll upd = std::max(cur, g[idx][j]);
      if (upd <= 2 * sum[i - 1]) {
        idx += 1 << j, cur = upd;
      }
    }
    ans += num_equal(i, idx, sum[i - 1]);
  }
  std::cout << ans << "\n";
}

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  ll t;
  std::cin >> t;
  while (t--) {
    solve();
  }
}
```
</details>

Time complexity: $\mathcal{O}(n \log{n})$

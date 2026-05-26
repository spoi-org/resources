---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Algorithm'
weight: 1
---

**Note**: [binary search](/techniques/binary_search/basics) is a prerequisite.

Suppose we know that $f(x)=1$ for $x\le t$ and $f(x)=0$ otherwise. We can use binary lifting to find the value of $t$. The advantage of this method over normal binary search is that is always gurantees a fixed number of iterations. It is, for example, used to find the [lowest common ancestor](https://cp-algorithms.com/graph/lca_binary_lifting.html) of two nodes in a tree, and it is useful to keep in mind.

```cpp
int t = 0;
for (int i = 1 << 30; i >= 1; i >>= 1) {
  t += i * f(i + t);
}
std::cout << t << '\n';
```

The reason this works relies on the binary representation of a number. Let's say $f(i+t)$ was $0$, that is, we'd have jumped too far if we took the $i^\text{th}$ bit. It is still possible for us to check every jump length from $[0,2^i-1]$, since

$011111_2 = 100000_2 - 1_2$,

that is, the bits lower than $i$ can still exhaustively cover every possible smaller answer.

Another way to think of this is to think of the binary representation of $t$. We're trying to maximize it, and just like how for numbers of equal lengths, any number starting with a $9$ has to be bigger than a number starting with an $8$ in base $10$, a number starting with $1$ must be greater than a number starting with $0$ in binary. On every iteration, we're checking whether the $i^\text{th}$ bit can be $1$. 

Also, if we knew the value of $t$ beforehand, we could visualize how this algorithm would always find the correct series of jumps, and would eventually converge to $t$.

## Minimum/maximum with binary lifting

> Given an array $A$, answer $Q$ queries where, given an index $i$ and a number $x$, you need to output the largest index $j$ such that $\max\{A[i]...A[j]\} \le x\cdot A[i]$.

We can use binary lifting to solve this problem. The only tricky part is coming up with a valid $f(x)$.

**Key observation**: Refer to the earlier code. At every step, we only ever jump ahead by a power of $2$.

Assume we had some function $g(i,j)$ that would return $\max\{A[i]...A[i+2^j-1]\}$ in constant time. Notice that this function only has $\mathcal{O}(n \log{n})$ possible inputs ($i \in [0,n)$ and $j \in [0, \lfloor \log_2{n} \rfloor]$.

If we did have such a function, then we would be able to apply binary lifting similar to how we've done above:

```cpp
int ans = i, cur = A[i];
for (int j = 19; j >= 0; --j) {
  int upd = std::max(cur, g(ans, j));
  if (upd <= x * A[i]) {
    ans += 1 << j, cur = upd;
  }
}
std::cout << ans << '\n';
```

(Note: $2^{19} \approx 5\cdot 10^5$, which is larger than $10^5$, the standard constraint on $n$ in most problems)

Now, how do we compute $g(i,j)$ efficiently? 

**Base case**: If $j=0$, then $g(i,0)$ is just $A[i]$.

**Recursive case**: Otherwise, for $j>0$, notice that an interval of size $2^j$ can be broken down into two sub-intervals of sizes $2^{j-1}$. $g(i,j)=\max\{g(i,j-1), g(i+2^{j-1},j-1)\}$. Don't forget to account for the fact that $i+2^{j-1}$ may exceed $n-1$.

This computes $g(i,j)$ in $\mathcal{O}(n \log{n})$ time and uses $\mathcal{O}(n \log{n})$ space.
```cpp
std::vector<std::array<int, 20>> g(n);
for (int i = 0; i < n; ++i) {
  g[i][0] = A[i];
}
for (int j = 1; j < 20; ++j) {
  for (int i = 0; i < n; ++i) {
    g[i][j] = std::max(g[i][j - 1], g[std::min(i + (1 << (j - 1)), n - 1)][j - 1]);
  }
}
```


<br>
<problem>inv-brac-seq</problem>

<br>

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

## Practice problems
<ptable>
<prow>ruler-easy</prow>
<prow>AGGRCOW</prow>
<prow>guess-the-tree</prow>
<prow>hungry-games</prow>
<prow>cake-division</prow>
<prow>solar-storm</prow>
<prow>aesthetic</prow>
</ptable>

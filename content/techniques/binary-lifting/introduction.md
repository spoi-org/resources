---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Introduction'
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

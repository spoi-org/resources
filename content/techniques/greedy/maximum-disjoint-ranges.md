---
draft: false
title: 'Maximimum disjoint ranges'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Movie Festival"
weight: 1
---

{{< problem "cses-movie-festival" >}}

## Abridged problem statement

Given $n$ ranges, pick a subset $S$ of ranges $[s_i, e_i)$, maximizing $|S|$, such that, for any two ranges $a,b \in S$, $a \cap b = \varnothing$.

In other words, pick as many ranges as you can while ensuring that no two of them intersect.

## Solution

There is no natural next step we can perform in order to solve this problem. Perhaps a greedy strategy will work? What if we simply pick the first occuring interval every single time? Does this maximize the number of intervals we pick?

Turns out, no. Seeing why is pretty easy too. Consider this case:

<div style="margin: 2rem auto; width: 32rem; height: 8rem; position: relative;">
  <span style="position: absolute; left: 2rem; top: 4.2rem; width: 18rem; border-top: 7px solid currentColor;"></span>
  <span style="position: absolute; left: 20rem; top: 4.2rem; width: 6rem; border-top: 7px solid orange;"></span>
  <span style="position: absolute; left: 5rem; top: 2.3rem; width: 6rem; border-top: 7px solid #1976d2;"></span>
  <span style="position: absolute; left: 17rem; top: 2.3rem; width: 3rem; border-top: 7px solid #1976d2;"></span>
  <span style="position: absolute; left: 2rem; top: 6.4rem; width: 24rem; border-top: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 2rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 1.85rem; top: 6.75rem; font-size: 0.8rem;">0</span>
  <span style="position: absolute; left: 5rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 4.85rem; top: 6.75rem; font-size: 0.8rem;">1</span>
  <span style="position: absolute; left: 8rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 7.85rem; top: 6.75rem; font-size: 0.8rem;">2</span>
  <span style="position: absolute; left: 11rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 10.85rem; top: 6.75rem; font-size: 0.8rem;">3</span>
  <span style="position: absolute; left: 14rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 13.85rem; top: 6.75rem; font-size: 0.8rem;">4</span>
  <span style="position: absolute; left: 17rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 16.85rem; top: 6.75rem; font-size: 0.8rem;">5</span>
  <span style="position: absolute; left: 20rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 19.85rem; top: 6.75rem; font-size: 0.8rem;">6</span>
  <span style="position: absolute; left: 23rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 22.85rem; top: 6.75rem; font-size: 0.8rem;">7</span>
  <span style="position: absolute; left: 26rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 25.85rem; top: 6.75rem; font-size: 0.8rem;">8</span>
</div>

Our strategy would pick the interval $[0, 6)$, and then $[6, 8)$, achieving $|S|=2$. It is clear that we can achieve $|S|=3$, however, by picking the blue intervals instead.

So... what do we do?

The right move in most greedy problems is to 'fix' the right variable. With this in mind, let us try fixing the time we arrive at the festival, that is, the time after which we can start picking intervals. In fact, let us define a function $f(t)$ that denotes the maximum number of intervals we can pick if we can only pick intervals that start at or after $t$. 

The point of doing this is to notice only one thing: that $f$ is monotonic. Indeed, $f(t) \le f(t-1)$ for any value of $t$. This is because the set of intervals associated with $t-1$ is bigger than that affected with $t$, so, if $f(t-1)$ was currently worse, we could just pick the same set of intervals picked by $f(t)$ to get them to be equal.

Now, out of out the intervals $[s_i, e_i)$ available to us at $t=0$, let us try picking all of them. Then, we get

$$
f(0) = 1+\max_{0\le i < n} f(e_i)
$$

We could now proceed with some sort of dynamic programming solution; do note that we don't need to explicitly exclude the interval $i$ we selected as it will not be re-included in $f(e_i)$, because $s_i \le e_i$, and—but wait! We don't need to do any of that, because we already know that $f(t)$ is maximized for the smallest $t$. So, we can just pick $i$ such that $e_i$ is the smallest possible.

Then, we discard intervals with $s_j < e_i$ and proceed with calculating $f(e_i)$ in the exact same way!

## Exchange argument

While this solution is complete, with proof, I'll present another way to prove the same result. Formally, what we are saying is that there exists at least one optimal $S$ that contains $[s_i, e_i)$, where $e_i$ is minimized.

The way we prove this is by considering an arbitrary $S$. Now, we proceed to transform $S$ to another set $S'$, with the hope that $|S'| \ge |S|$. This method of proving greedy algorithms is popularly known as an exchange argument.

So let us do this. Consider an arbitrary set $S$.

<div style="margin: 2rem auto; width: 32rem; height: 8rem; position: relative;">
  <span style="position: absolute; left: 2rem; top: 4.2rem; width: 12rem; border-top: 7px solid #2e7d32;"></span>
  <span style="position: absolute; left: 23rem; top: 4.2rem; width: 3rem; border-top: 7px solid #2e7d32;"></span>
  <span style="position: absolute; left: 5rem; top: 1.6rem; width: 6rem; border-top: 7px solid currentColor;"></span>
  <span style="position: absolute; left: 17rem; top: 1.6rem; width: 3rem; border-top: 7px solid #2e7d32;"></span>
  <span style="position: absolute; left: 2rem; top: 6.4rem; width: 24rem; border-top: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 2rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 1.85rem; top: 6.75rem; font-size: 0.8rem;">0</span>
  <span style="position: absolute; left: 5rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 4.85rem; top: 6.75rem; font-size: 0.8rem;">1</span>
  <span style="position: absolute; left: 8rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 7.85rem; top: 6.75rem; font-size: 0.8rem;">2</span>
  <span style="position: absolute; left: 11rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 10.85rem; top: 6.75rem; font-size: 0.8rem;">3</span>
  <span style="position: absolute; left: 14rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 13.85rem; top: 6.75rem; font-size: 0.8rem;">4</span>
  <span style="position: absolute; left: 17rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 16.85rem; top: 6.75rem; font-size: 0.8rem;">5</span>
  <span style="position: absolute; left: 20rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 19.85rem; top: 6.75rem; font-size: 0.8rem;">6</span>
  <span style="position: absolute; left: 23rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 22.85rem; top: 6.75rem; font-size: 0.8rem;">7</span>
  <span style="position: absolute; left: 26rem; top: 6.15rem; height: 0.5rem; border-left: 1px solid currentColor;"></span>
  <span style="position: absolute; left: 25.85rem; top: 6.75rem; font-size: 0.8rem;">8</span>
</div>

Now, if $S$ contains $i$, we're done. Otherwise, sort the intervals contained in $S$ by their right endpoints, and consider the first one. Call this interval $j$.

We note that $e_j \ge e_i$. Let us make $S' = (S \setminus \{j\}) \cup \{i\}$. Now:

- $i$ is now the first interval in $S'$.
- Since $e_i \le e_j$, and $j$ did not intersect with any other intervals in $S$, $i$ will not either.

Therefore, we have managed to construct $S'$ with $|S'| \ge |S|$, proving that an optimal set containing $i$ must always exist.

For readers not convinced by the rigour of this proof, we are essentially imagining a huge set of sets of all the possible ways to pick intervals ($2^n$). In this set, we want to prove that our target set ($S'$) is the maximum element. We do this by showing, for every other element in the set ($S$), that $|S'| \ge |S|$.

## Code

Here's how we implement this in C++:

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n;
  cin >> n;
  vector<pair<int, int>> a(n);
  for (auto &[s, e] : a) {
    cin >> s >> e;
  }
  sort(a.begin(), a.end(), [&](const pair<int, int> &a, const pair<int, int> &b) {
    return a.second < b.second;
  });
  int ans = 0;
  int t = 0;
  for (auto &[s, e] : a) {
    if (s < t) {
      continue;
    }
    ans++;
    t = e;
  }
  cout << ans << '\n';
}
```
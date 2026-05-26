---
draft: false
title: 'Cake 3'
editorial:
  platform: "JOISC"
  name: "Cake 3"
---

{{< problem "joisc-cake3" >}}

## Abridged problem statement

You're given two arrays $a$ and $b$ of size $n$ ($3 \le n \le 10^5$), and a number $m$ ($m \le n$). Choose any permutation of $\{0,2,...,m-1\}=p$ to maximize the cyclic sum:

$$
\sum_{i=0}^{m} a_{p[i]} - \sum_{i=0}^{m} |b_{p[i]} - b_{p[i+1]}|
$$
where $p[m]$ is treated as $p[0]$.

## Solution

The permutation in the problem statement can be a bit confusing at first glance. To make things simpler, think of each index $i$ as representing a pair $(a_i, b_i)$.

Now, instead of directly working with the original arrays, imagine we can reorder these indices in any way we want. Then, we choose a subset of size $m$ of these reordered indices and maximize the cyclic sum over that subset.

Why is this potentially helpful? We had a permutation before, which was not ordered, but if we did this, we'd be able to just choose a subset of $\{0,1,...,n-1\}$ that *is* ordered.

Fair enough, but this doesn't help much right now. In fact, it won't help at all until we make this observation:

> For any arbitrary array $\text{arr}$ of length $n$
>
> $$
\sum_{i=0}^{n} |\text{arr}_i-\text{arr}_{i+1}|
$$
> is minimized when $\text{arr}$ is sorted ($\text{arr}_i \le \text{arr}_{i+1}$)

<details>
<summary>How would I think of this?</summary>

Write a program that generates a random array, then goes through all permutations of it.

Keep track of the best sum and the permutation that caused it. Print this at the end of your program. You'll notice that the array is always sorted.

Run this a few times (maybe $5$ to $6$ times) to convince yourself that it's true.
</details>

Of course, I won't just leave it at that. Here's how you prove this is true.

Let's first look at this example: $a = [1, \underline{3}, \underline{2}, 4, 5] \rightarrow [1, \underline{2}, \underline{3}, 4, 5]$. How does the sum change? Well, $\text{old}=|1-3|+|3-2|+|2-4|$ changes to $\text{new}=|1-2|+|2-3|+|3-4|$. All other terms remain the same. And we notice that $\text{new}<\text{old}$. Can we formalize this?

Okay, so maybe let's try running the [bubble sort algorithm](https://en.wikipedia.org/wiki/Bubble_sort) on this array to sort it. Each time we swap an adjacent unsorted pair, we go from: $[\dots a_1, a_3, a_2, a_4 \dots]$ to $[\dots a_1, a_2, a_3, a_4 \dots]$. Since:

$|a_1-a_3|+|a_3-a_2|+|a_2-a_4| \ge |a_1-a_2|+|a_2-a_3|+|a_3-a_4|$

is always true when $a_1 \le a_2 > a_3 \le a_4$, we're done?

Not quite. Bubble sort might also fix inversions where $a_1 > a_4$. And in this case, the sum actually increases after the swap!

<details>
<summary>Note</summary>
You should expect this. You could see this as you 'breaking' an existing descending sort.
</details>

Okay, so that didn't work. No worries, at least we have more intuition now, and we can get to something that does work: induction on the array size.

Assume the result is true for all arrays of size $n-1$. Can we prove that it works for $n$?

So I have a sorted array of length $n-1$ consisting of the first $n-1$ elements of $a$. I now want to insert $a_i$ to this array in order to minimize the sum given above. Where would I insert it?

<details>
<summary>Spoiler</summary>

Think about it: we want to insert $a_i$ in a way that minimizes the total added difference with its neighbors. So what position does that?

Well, if we put $a_i$ in a place where it's already in order with its surroundings, then the jumps to its left and right are as small as possible.

That’s right: we should insert it in the position that keeps the array sorted!

</details>

### Subtask 1, 2 ($n \le 2000$)

Sort the arrays as previously described, ensuring that $b_i \le b_{i+1}$ after you're done.

It is pretty easy to see that for a sorted array, $\sum b_i=(b_2-b_1)+(b_3-b_2)+\dots+(b_n-b_{n+1})+(b_n-b_1)=2(b_n-b_1)$.

Let's go through each subarray $[i\dots j]$ that is at least as big as $m$. We'll 'fix' the two endpoints of the subset we're gonna pick as $i$ and $j$, so now we need to pick $m-2$ elements from the middle.

However nothing but the endpoints matter for $b$, so we just need to pick the $m-2$ largest elements in $a$ from $(i, j)$. You can use, say, a multiset to achieve this, solving the problem in $\mathcal{O}(n^2 \log{n})$ time.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  const int64_t inf = 1e15;

  int n, m;
  std::cin >> n >> m;
  std::vector<std::pair<int64_t, int64_t>> a(n);
  for (auto &[b, a] : a) {
    std::cin >> a >> b;
  }
  std::sort(a.begin(), a.end());

  int64_t ans = -inf;
  for (int i = 0; i < n; ++i) {
    std::multiset<int64_t> st;
    int64_t sum = 0;
    auto add = [&](int64_t x) {
      if (st.size() + 1 <= m - 2) {
        st.insert(x);
        sum += x;
        return;
      }
      if (x > *st.begin()) {
        sum += -*st.begin() + x;
        st.erase(st.begin());
        st.insert(x);
      }
    };
    for (int j = i + 1; j < i + m - 1; ++j) {
      add(a[j].second);
    }
    for (int j = i + m - 1; j < n; ++j) {
      ans = std::max(ans, a[i].second + sum + a[j].second - 2 * (a[j].first - a[i].first));
      add(a[j].second);
    }
  }
  std::cout << ans << '\n';
}
```
</details>

### Full solution

Let's start with an observation.

**Observation**: Define $f(i)$ as the value of $j > i$ such that choosing $i$ and $j$ as endpoints will maximize the summation as stated in the problem, with ties broken arbitrarily, but consistently. Then $f(i) \le f(i+1)$.

**Proof**: Assume this was not true. In that case, we'd have $f(i+1) = j' < f(i) = j$. However, if this was the case, we could immediately improve our solution for $f(i)$ to $j'$ by picking the exact same values as $f(i+1)$ other than the first element, which would, of course, be $i$.

To understand this better, try doing the reverse: forcing $i$'s solution on $i+1$: this would be *worse* by the fact that $f(i+1)$ is already optimal.

Once we know that $f(i)$ is [monotonic](https://en.wikipedia.org/wiki/Monotonic_function), we can use divide and conquer to massively speed up our solution!

**Strategy**: Calcualate $f(0), f(1), \dots, f(n-m)$ together using divide and conquer. At each divide and conquer step, we'll store the *range of $i$ values* we want to calculate $f(i)$ for, **and** the possible range $f(i)$ can take on.

We'll first compute $f(m=\lfloor\frac{l+r}{2}\rfloor)=x$. Now we know that $f$ values to the left of $m$ will have a smaller range ($[\text{cur\_left}, m]$). Correspondingly, $f$ values to the right will also have a smaller range ($[m, \text{cur\_right}]$).

If we can figure out what $f(m)$ is in $\mathcal{O}(T(r))$ time, where $r$ is the *current active value range*, then our whole solution would have a time complexity of $\mathcal{O}(T(n) \log(n-m))$.

**Exercise**: Prove this time complexity. It's not that hard and will ensure that you've truly understood this. If you don't attempt doing this, the next parts of the editorial might be confusing to you.

The next natural question is: how do we efficiently find $f(m)$? The main bottleneck seems to be translating this code:

```cpp
ans = std::max(ans, a[i].second + sum + a[j].second - 2 * (a[j].first - a[i].first));
```

to something more efficient.

{{< problem "yosupo-range-kth-smallest" >}}

How do we solve this problem? There are multiple ways: the most obvious one you're probably thinking of right now is using a merge-sort tree (if you don't know what this is, I strongly recommend you check out [CSES'](https://cses.fi/problemset/list/) range queries section, they have really fun problems!). This is $\mathcal{O}(\log^2 n)$ per query, and if we used this for our solution, we'd have ~$\mathcal{O}(n \log^3{n})$. Too slow.

But there's a way to solve this problem in $\mathcal{O}(\log{n})$ per query as well using either [wavelet trees](/data-structures/wavelet-tree) or a persistent segment tree.

However, our purposes require the *sum* of the $m-2$ maximum values in a given range. This is an easy modification to make once you learn how wavelet trees/persistent segment trees answer order statistic queries.

Either way works and brings your final complexity down to $\mathcal{O}(n \log(n) \log(n-m))$, which comfortably passes.
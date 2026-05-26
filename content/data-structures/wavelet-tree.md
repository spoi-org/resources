---
date: '2026-05-25T21:47:38Z'
draft: true
title: 'Wavelet tree'
editorial:
  platform: "Yosupo"
  name: "Range Kth Smallest"
---

{{< problem "yosupo-range-kth-smallest" >}}

Wavelet trees are (yet another) divide and conquer data structure that answer static range order statistic queries. As an example, consider:


$$A=[4,3,2,1,5]$$

and assume 0-based indexing. Using a wavelet tree, you can answer queries such as 'print the second smallest element from $a_1,a_2,\dots,a_4$' (=$2$) in $\mathcal{O}(\log{n})$ time.

Again— you might think— $\mathcal{O}(\log{n})$? Looks like we'd need to perform some sort of smart divide and conquer on array indices. However, wavelet trees actually perform divide and conquer on the array's elements' values!

You may— again, rightfully so— be confused. But wait, where's $M$ in the time complexity then? And the answer is: we can actually treat the original value range as a subrange of $[0, n)$ without losing any information due to coordinate compression.

However, this is an optimization. You don't need this to make a functional wavelet tree. As usual, we start at the root node, which stores values in some range $[l, r]$. Now we set $m=\lfloor\frac{l+r}{2}\rfloor$, and make it so all elements $\le m$ are placed to the left in the array, and all elements $>m$ are placed to the right. 

For example, 
$$A=[4,\textcolor{green}{3,2,1},5] \rightarrow [\textcolor{green}{3,2,1},4,5]$$. Notice how I've not changed the *relative order* of the elements besides the fact that now all elements $\le 3$ are to the left of all elements $> 3$ (by the way, this operation is called a [stable_partition](https://en.cppreference.com/w/cpp/algorithm/stable_partition.html)). The entire idea of **mutating the array** while you construct a wavelet tree is hard to comprehend and makes you question what a wavelet tree node even stores. Let's try to understand that now.

```cpp
class wavelet_tree {
public:
  std::vector<int> a, map_left;
  int min, max;
};
```

Okay... some of this makes sense. `a`: sure, we can suppose that's the mutated subarray (left/right part, or the full array for the root node). `min` and `max` could store the minimum and maximum element in `a`: that also makes sense. But what's `map_left`?

`map_left` is the novel idea that makes wavelet trees ridiculously fast. `map_left[i]` is just equal the number of elements in `a[0...i]` that will go to the *left half* of the tree after partitioning. We can build this with prefix sums.

Remember, the query we're trying to answer is "find the $k$-th element in a range $[l, r]$".

The picture/high-level idea you should have in mind is:

> Let's try binary searching for the *value* the $k$-th element takes on by counting how many elements are $\le$ it.

However, just like a segment tree walk, we do this indirectly. In fact, consider a slightly easier problem:

> Given a static array $A$, answer $q$ queries where you find the $k$-th element.

Of course, we can just sort and index, but let's try using the aforementioned idea here. If we split this array up into two halves, we now just need to count how many values have gone to the left half ($\le m$) and how many have gone to the right half. Let these two numbers be $L$ and $R$— then if $k \le L$ we go to the left subtree, otherwise we go to the right subtree. Really simple.

The question is: what changes between this problem and the previous one? The most glaring difference is that now you also have to consider a range. So now when we move to the left half, we don't just care about there being enough $\le m$ values in **total**: they should specifically be in the range $[l, r]$ specified along with the query. 

> But wait. We have a prefix sum, don't we? Can't we just do `map_left[r] - map_left[l - 1]`?

And that's the core insight! It really was that straightforward— yes, we just shrink our range down to match the range of the query. This is why we need to store a full prefix sum instead of just a single number (in the previous example).

Divide and conquering to the left and right is also easy: but wait, our array mutates as we go down. Looks tricky, until we realise we can use `map_left` for this too. Think about it: `map_left[i]` would literally mean "the 1-based index of `i` in the left half of the array". 

If this is your first time reading this article, you should **not** read the code below— instead, you should code try coding wavelet trees yourself before continuing.

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

template <typename T> class WaveletTree {
public:
  std::vector<int> a;
  std::vector<T> o;
  std::vector<std::vector<int>> wv;
  int n;

  WaveletTree(const std::vector<T> &_a) : a(_a.size()), o(_a) {
    std::sort(o.begin(), o.end());
    o.erase(std::unique(o.begin(), o.end()), o.end());
    for (int i = 0; i < a.size(); ++i) {
      a[i] = std::lower_bound(o.begin(), o.end(), _a[i]) - o.begin();
    }
    wv.resize(2 * (n = std::bit_ceil(o.size())));
    wv[1].resize(a.size() + 1);
    for (int j = std::countr_zero(uint32_t(n)), v = 1; j > 0; --j) {
      for (int i = 0, size = 1 << j, tl = (v << j) - n, tr = tl + size - 1, tm = tl + ((1 << (j - 1)) - 1); i < a.size();
        ++v, tr += size, tm += size) {
        int p = i;
        for (int j = 1; i < a.size() and a[i] <= tr; ++i, ++j) {
          wv[v][j] = wv[v][j - 1] + (a[i] <= tm);
        }
        int pt = std::stable_partition(a.begin() + p, a.begin() + i, [&](int x) {
          return x <= tm;
        }) - a.begin();
        if (v < n) {
          wv[2 * v].resize(pt - p + 1);
          wv[2 * v + 1].resize(i - pt + 1);
        }
      }
    }
  }

  T kth(int l, int r, int k) {
    r++;
    int i = 1;
    while (i < n) {
      int a = wv[i][l], b = wv[i][r], c = b - a;
      i <<= 1;
      if (k <= c) {
        l = a, r = b;
      } else {
        l -= a, r -= b, k -= c, i++;
      }
    }
    return o[i - n];
  }
};

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  int n, q;
  std::cin >> n >> q;
  std::vector<int> b(n);
  for (auto &i : b) {
    std::cin >> i;
  }
  WaveletTree<int> wv(b);
  while (q--) {
    int l, r, k;
    std::cin >> l >> r >> k;
    std::cout << wv.kth(l, r - 1, k + 1) << '\n';
  }
}
```
</details>


External references: [this](https://ioinformatics.org/journal/v10_2016_19_37.pdf) amazing IOI journal paper that explains wavelet trees.
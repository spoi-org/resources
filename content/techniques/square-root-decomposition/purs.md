---
draft: false
title: 'Point update range sum'
weight: 1
editorial:
  platform: "CSES"
  category: "Range Queries"
  name: "Dynamic Range Sum Queries"
---

{{< problem "cses-dynamic-range-sum-queries" >}}

The problem is simple: we must answer range sum queries while performing point updates. That is, we must support the following two types of operations on an array $a$ of length $n$:

* $a_i \leftarrow x$
* Find $\sum_{i=l}^r a_i$

Here, $n \le 2\cdot 10^5$.

## What if $n$ was smaller?

In that case, we could literally just simulate:

```cpp
while (q--) {
  int type;
  cin >> type;
  if (type == 1) {
    int i, x;
    cin >> i >> x;
    a[i] = x;
  } else {
    int l, r;
    cin >> l >> r;
    int64_t sum = 0;
    for (int i = l; i <= r; ++i) {
      sum += a[i];
    }
    cout << sum << '\n';
  }
}
```

Here, each query of type $1$ takes $\mathcal{O}(1)$ time, and each query of type $2$ takes $\mathcal{O}(n)$ time.

If you've read about [prefix sums](< ref "techniques/static-range-queries/when-inverses-exist/prefix-sums.md" >), you may think of instead writing it this way, making queries of type $2$ $\mathcal{O}(1)$:

```cpp
while (q--) {
  int type;
  cin >> type;
  if (type == 1) {
    int i, x;
    cin >> i >> x;
    a[i] = x;
    for (int j = i; j <= n; ++j) {
      pref[j] = pref[j - 1] + a[j];
    }
  } else {
    int l, r;
    cin >> l >> r;
    cout << pref[r] - pref[l - 1] << '\n';
  }
}
```

Of course, we must now spend $\mathcal{O}(n)$ time for updates!

## What is square-root decomposition?

The whole idea is this—we have two methods ($1$ and $2$)—one that can perform query $1$ fast, and one that can perform query $2$ fast. We will divide the array into $\sqrt{n}$ blocks, each of length $\sqrt{n}$. Each block will use method $x$. We will then merge the blocks with method $3-x$.

Was that confusing? 

Well, let's actually fill in the gaps and see how this works!

<figure>
  <img src="/sqrt-decomposition-blocks.svg"
       alt="An array divided into blocks numbered 1, 2, …, √n; each block contains elements numbered 1, 2, …, √n."
       style="display:block; width:100%; height:auto; margin:0 auto;">
  <figcaption>
    Square-root decomposition!
  </figcaption>
</figure>
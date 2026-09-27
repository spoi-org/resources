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

If you've read about [prefix sums](< ref "techniques/static-range-queries/when-inverses-exist/prefix-sums.md" >), you may instead write it this way, making queries of type $2$ $\mathcal{O}(1)$:

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

Well, let's actually fill in the gaps and see how this works.

<figure>
  <img src="/sqrt-decomposition-blocks.svg"
       alt="An array divided into blocks numbered 1, 2, …, √n; each block contains elements numbered 1, 2, …, √n."
       style="display:block; width:100%; height:auto; margin:0 auto;">
  <figcaption>
    Square-root decomposition!
  </figcaption>
</figure>

## Solution #1

For each block, store a prefix sum of length $\sqrt{n}$ (method 2). To connect the blocks, we store no additional information (method 1).

Notice that a sum query spans part of a left block, several whole middle blocks, and part of a right block:

<figure>
  <img src="/sqrt-decomposition-range-query.svg"
       style="display:block; width:100%; height:auto; margin:0 auto;">
</figure>

For each of the left and right blocks, we use the prefix sum arrays stored in them to obtain their sum contributions in $\mathcal{O}(1)$ time. Then, we sum up the last element of the prefix sum array for each of the blocks in the middle with a simple for loop: this takes $\mathcal{O}(\sqrt{n})$ time, which is also our overall time complexity.

It is also possible for a query to be entirely contained in a single block: if this happens, we simply use that block's prefix sum.

When we want to update an element, we can simply rebuild the block it's a part of, which will take $\mathcal{O}(\sqrt{n})$ time. 

### Implementation

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n, q;
  cin >> n >> q;
  vector<int> a(n);
  for (int &i : a) {
    cin >> i;
  }

  // size of each block
  const int B = sqrt(n);

  // shared prefix array across all blocks
  // block i uses pref[B*i...B*(i+1))
  vector<int64_t> pref(n);

  // populates the pref array for block `i`
  // the first block is block 0
  auto make_block = [&](int i) {
    // first copy the original values
    copy(a.begin() + B * i, a.begin() + min(B * (i + 1), n), pref.begin() + B * i);
    // then prefix sum them
    partial_sum(pref.begin() + B * i, pref.begin() + min(B * (i + 1), n), pref.begin() + B * i);
  };

  // make each initial block
  for (int i = 0; i < (n + B - 1) / B; ++i) {
    make_block(i);
  }

  // now start processing queries
  while (q--) {
    int type;
    cin >> type;
    if (type == 1) {
      // rebuild the block after an update
      int k, u;
      cin >> k >> u;
      a[k - 1] = u;
      make_block((k - 1) / B);
    } else {
      int l, r;
      cin >> l >> r;
      --l, --r;
      if (l / B == r / B) { // same block, just use prefix sum
        int64_t sum = pref[r];
        if (l % B != 0) {
          sum -= pref[l - 1];
        }
        cout << sum << '\n';
        continue;
      }
      // sum the left and right partials
      int64_t sum = 0;
      if (l % B != 0) { // left partial exists, add it
        sum += pref[B * (l / B) + B - 1] - pref[l - 1];
      }
      if ((r + 1) % B != 0) { // right partial exists, add it
        sum += pref[r];
      }
      // now sum up all middle blocks
      for (int i = (l / B) + (l % B != 0); i <= (r / B) - ((r + 1) % B != 0); ++i) {
        sum += pref[i * B + B - 1];
      }
      cout << sum << '\n';
    }
  }
}
```

## Solution #2

Could we have done it the other way around? What if we maintained no information for each block and only stored a prefix sum over all the blocks instead?

To answer sum queries, we again deal with the same cases. If a query's left and right endpoints both lie in the same block, this time we manually iterate over the elements in that block and sum them up in $\mathcal{O}(\sqrt{n})$ time.

Otherwise, we have a partial left and right intersection with blocks in the middle. We sum up the contributions of the left and right blocks simiarly to the single block case ($\mathcal{O}(\sqrt{n})$). For the blocks in the middle, we use the existing prefix sum and obtain their sum in $\mathcal{O}(1)$.

When we update a block, we must update all prefix sums including and ahead of that block. Since there are $\mathcal{O}(\sqrt{n})$ blocks, this takes $\mathcal{O}(\sqrt{n})$ time.

### Implementation

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n, q;
  cin >> n >> q;
  vector<int> a(n);
  for (int &i : a) {
    cin >> i;
  }

  // the size of each block
  const int B = sqrt(n);

  // returns the sum of the elements in block `i`
  // the first block is block 0
  auto sum_block = [&](int i) {
    return accumulate(a.begin() + B * i, a.begin() + min(B * (i + 1), n), int64_t(0));
  };

  // each block's sum
  vector<int64_t> sum((n + B - 1) / B);
  // prefix sum over blocks
  vector<int64_t> pref((n + B - 1) / B);

  // build initial prefix sum
  for (int i = 0; i < (n + B - 1) / B; ++i) {
    sum[i] = sum_block(i);
  }
  partial_sum(sum.begin(), sum.end(), pref.begin());
  
  while (q--) {
    int type;
    cin >> type;
    if (type == 1) {
      // update the element and re-compute prefix sum
      int k, u;
      cin >> k >> u;
      a[k - 1] = u;
      sum[(k - 1) / B] = sum_block((k - 1) / B);
      partial_sum(sum.begin(), sum.end(), pref.begin());
    } else {
      int l, r;
      cin >> l >> r;
      --l, --r;
      if (l / B == r / B) { // sum manually 
        cout << accumulate(a.begin() + l, a.begin() + r + 1, int64_t(0)) << '\n';
        continue;
      }
      // sum the left and right partials
      int64_t sum = 0;
      if (l % B != 0) { // left partial exists, add it
        sum += accumulate(a.begin() + l, a.begin() + min(n, B * (l / B + 1)), int64_t(0));
      }
      if ((r + 1) % B != 0) { // right partial exists, add it
        sum += accumulate(a.begin() + B * (r / B), a.begin() + r + 1, int64_t(0));
      }
      // now use the prefix sum for the middle blocks
      sum += pref[(r / B) - ((r + 1) % B != 0)];
      if ((l / B) + (l % B != 0) > 0) {
        sum -= pref[(l / B) + (l % B != 0) - 1];
      }
      cout << sum << '\n';
    }
  }
}
```

## Why square-root?

We chose $B$ to be $\sqrt{n}$ in both solutions, which resulted in a time complexity of $\mathcal{O}(\sqrt{n})$ per update. However, what if we let $B$ just refer to the block size? What would the time complexity of each of these solutions look like in terms of $q_1$, $q_2$, $n$, and $B$ (where $q_1+q_2=q$)?

For the sake of brevity, let's only analyse solution #1 (the analysis for solution #2 is very similar). Building the initial prefix sums takes $\mathcal{O}(n)$ time in total. Then, for each query of type $1$, we call `make_block`, which is $\mathcal{O}(B)$ ($\mathcal{O}(q_1B)$ in total). For each query of type $2$, we potentially loop over all blocks: there are $\frac{n}{B}$ many of them, so the total time complexity is $\mathcal{O}(q_2\frac{n}{B})$.

Summing this up, we obtain a complexity of

$$
\mathcal{O}(n+q_1B+q_2\frac{n}{B})
$$

We see that the time complexity is a function of $B$. The question is: what value of $B$ makes the overall time complexity the smallest?

Let us use differential calculus to answer this question.

Set $f(B) = n + q_1 B + q_2 \frac{n}{B}$, and compute $f'(B) = q_1 - q_2 \frac{n}{B^2}$. Setting this to $0$ gives us $B = \sqrt{n\frac{q_2}{q_1}}$.

which gives us a final time complexity of:

$$
\mathcal{O}(n + \sqrt{nq_1q_2})
$$

Usually, $B$ is chosen as $\sqrt{n}$: this is optimal when both query types occur around the same number of times.

When $q_1=q_2$, the time complexity simplifies to $\mathcal{O}(n + q \sqrt{n})$, which is what we had calculated before as well.

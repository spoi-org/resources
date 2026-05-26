---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Basics'
weight: 1
---

You are given a **sorted** array $A$ of length $n$. Your goal is to find an element $x$ in this array. Assume for now that $A$ consists of distinct elements, and that $x$ is guranteed to exist in $A$.

Here's how binary search solves this problem. We maintain two variables $l$ and $r$ that are initially $0$ and $n-1$ respectively. $[l, r]$ represents the range of indices in $A$ where $x$ could lie.

Next, we compute $m=\lfloor\frac{l+r}{2}\rfloor$. Notice that $[l,m]$ and $[m+1,r]$ are both roughly half the size of $[l,r]$. The key observation to make here is that we can quickly determine if it is impossible for $x$ to be part of a range. Specifically, if $[u,v]$ is a range and $A_u>x$ or $A_v<x$, then we know that it's impossible for $x$ to be in this range.

We check whether $A_m$ is $\ge x$. If this is true, we know that $x$ cannot lie beyond $m$, and so must lie in $[l,m]$. On the other hand, if this wasn't true, i.e., $A_m < x$, $x$ would have to lie in $[m+1,r]$.

We can keep dividing our search space in half until it only consists of one index, at which point we will know that $A_l=A_r=x$.

## Implementation
Here's how this is implemented:
```cpp
int l = 0, r = n - 1;
while (l < r) {
  int m = std::midpoint(l, r);
  if (A[m] >= x) {
    r = m;
  } else {
    l = m + 1;
  }
}
std::cout << x << " is located at index " << l << '\n';
```

Time complexity: $\mathcal{O}(\log{n})$.

There exists a very popular alternative implementation of binary search that is prefered by some people. Here, the question in hand is a rephrased version of the original one: find the smallest index $i$ such that $A_i \ge x$ (notice that both questions are indeed equivalent).

<details>
<summary>Alternative implementation</summary>

```cpp
int l = 0, r = n - 1;
int ans = n - 1;
while (l <= r) {
  int m = std::midpoint(l, r);
  if (A[m] >= x) {
    ans = m;
    r = m - 1;
  } else {
    l = m + 1;
  }
}
std::cout << x << " is located at index " << l << '\n';
```
</details>
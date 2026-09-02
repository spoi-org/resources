---
draft: true
title: 'Dynamic programming'
weight: 6
---

THIS IS A DRAFT.

There are various different techniques to solve problems with in competitive programming. However, most of the time, coming up with a *correct* solution is a lot easier than coming up with a *fast* and correct solution.

Usually, for any given problem, it is easy to come up with a solution with exponential time complexity. 

A small subset of these problems have remarkable properties that allow us to use *dynamic programming* to solve them fast.

## What is dynamic programming?

Dynamic programming is optimised brute force. Of course, not all brute force solutions can be optimised. Generally, a couple of things need to be true for dynamic programming to be considered a viable optimisation:

- There is a theoretical solution which solves the problem recursively. Do note that, currently, this solution does **not** have to be fast.
- The number of unique states that can be obtained is sufficiently small (usually smaller than $\approx 10^8$)

Before we proceed further, it may be useful to understand what 'being able to solve a problem recursively' means with an example, and you will find that almost all problems satisfy this first requirement.

## What is recursion?

Consider the following problem:

> In an array $a$ of length $n$, with $0 \le a[i] \le 5000$, does there exist a subset $S \in [0,1,2,\cdots,,n)$ such that $\sum_{i\in S} a[i] = w$, for a given $w$? It is given that $n,w \le 5000$.

We solve this problem with brute force by iterating over all possibilites for $S$ in $\mathcal{O}(2^n)$ time, like so:

```cpp
int possible = 0;
for (int mask = 0; mask < 1 << n; ++mask) {
  int sum = 0;
  for (int i = 0; i < n; ++i) {
    sum += a[i] * !!(mask & 1 << i);
  }
  possible |= sum == w;
}
```

This is the standard solution. Now, can we also solve this, in any possible way, recursively?

Yes, we can. We make a recursive function `possible()` that takes in a vector of integers, the sum so far, and the value of $w$ itself, and, at each step; we try every single possible remaining element.

```cpp
bool possible(vector<int> a, int cur_sum, int w) {
  if (w == cur_sum) {
    return true;
  }
  if (a.empty()) { // nothing more to try!
    return false;
  }
  for (int &x : a) {
    auto new_a = a;
    new_a.erase(find(new_a.begin(), new_a.end(), x));
    if (possible(new_a, cur_sum + x, w)) {
      return true;
    }
  }
  return false;
}
```

So we satisfy the first requirement: we have a recursive solution. However, one immediate observation we can make is that the variable `cur_sum` is unnecessary; we can simply subtract $x$ from $w$ itself. Although this doesn't change anything asymptotically, simpler code is better:

```cpp
bool possible(vector<int> a, int w) {
  if (w == 0) { // check that w is 0
    return true;
  }
  if (a.empty()) { 
    return false;
  }
  for (int &x : a) {
    auto new_a = a;
    new_a.erase(find(new_a.begin(), new_a.end(), x));
    if (possible(new_a, w - x)) { // subtract x from w instead
      return true;
    }
  }
  return false;
}
```

In addition, since $a[i] \ge 0$, if $w$ were to ever become negative, we would never be able to obtain $w=0$, so we can prevent this too:

```cpp
bool possible(vector<int> a, int w) {
  if (w == 0) {
    return true;
  }
  if (a.empty()) {
    return false;
  }
  for (int &x : a) {
    if (w - x < 0) { // just preventing an obvious failure case
      continue;
    }
    auto new_a = a;
    new_a.erase(find(new_a.begin(), new_a.end(), x));
    if (possible(new_a, w - x)) {
      return true;
    }
  }
  return false;
}
```

## Analysis

At this point, it is important to start analysing what our recursive code is doing. At each step of recursion, we try each element, so we obtain a time complexity of $\mathcal{O}(n!)$ assuming the vector copying is free. But we know that, asymptotically, $\mathcal{O}(n!) \gg \mathcal{O}(2^n)$. What's happening? Well, let's have a closer look:

Suppose $n=2$, $a=[1,2]$, and $w=4$. Then, we try both $[1,2]$ and $[2,1]$ individually: $[1,2]$ if our code chooses the first element both times, and $[2,1]$ if it chooses the second element first, and then the only remaining one. However, by the commutativity of addition, $1+2=2+1$, so both branches end up at the exact same state (this means that the values of $a$ and $w$ are equal, so they behave identically), and we're actually doing more work than what is needed. 

We can fix this unnecessary exploration by maintaining one canonical representative for all the permutations of a given subset. Any arbitrary choice is fine, here I enforce that the permutation (of indices chosen) must be sorted. If we make this change, our code is simplified dramatically, although we do need to remember one additional paramter: the previous index we chose.

```cpp
bool possible(vector<int> a, int i, int w) {
  if (w == 0) {
    return true;
  }
  if (i == n - 1) {
    return false;
  }
  int x = a[i + 1];
  if (w - x >= 0) { // choice where we pick `x`
    if (possible(a, i + 1, w - x)) {
      return true;
    }
  }
  if (possible(a, i + 1, w)) { // or we could choose to not pick `x`
    return true;
  }
  return false;
}
```

In fact, *this* code is the one that's actually equivalent to our iterative bitmask solution. The previous ones were significantly worse.

Now we count the number of distinct possible states. 

- $w$ does not drop below $0$, and has a maximum value of $W=5000$
- $i$ can only take on $n$ distinct values
- $a$ is always the original array, since it is no longer being modified

The number of states is $\mathcal{O}(nW)$. Does that mean we satisfy the second requirement? Yes!

## So then... dynamic programming?

Is that it? Well, not quite. Our code still has a time complexity of $\mathcal{O}(2^n)$ because, physically, it still explores two cases per recursion level, and there are $n$ recursion levels. However, we can fully fix this by either caching results or manually constructing a topological sort of state dependendies and writing the whole thing iteratively.

Doing the latter gives us this:

```cpp
map<pair<int, int>, bool> ans;
for (int i = n - 1; i >= -1; --i) {
  for (int w = 0; w <= W; ++w) {
    if (w == 0) {
      ans[{i, w}] = true;
      continue;
    }
    if (i == n - 1) {
      ans[{i, w}] = false;
      continue;
    }
    int x = a[i + 1];
    if (w - x >= 0) {
      if (ans[{i + 1, w - x}]) {
        ans[{i, w}] = true;
      }
    }
    if (ans[{i + 1, w}]) {
      ans[{i, w}] = true;
    } else {
      ans[{i, w}] = false;
    }
  }
}
```

and we're fully done!
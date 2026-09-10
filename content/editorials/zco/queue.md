---
draft: false
title: 'Queue'
editorial:
  platform: "ZCO"
  name: "Queue"
---

For a video editorial, [go here](https://youtu.be/hYAB5oXZruc?t=278&si=-oLpANo9FyAHd5hN)!

{{< problem "zco-queue" >}}

## No updates

(We number the people from $1$ to $n$, while the corresponding arrays are zero-indexed.)

The main idea here is that the number of valid configurations we can reach is very small (not exponential). 

In particular, there are only $n+1$ configurations—we can bribe any suffix of people of length from $0$ to $n$. 

<figure>
  <img
    src="/queue-example.webp"
    alt="Animation demonstrating the above property"
    style="display:block; width:100%; height:auto; margin:0 auto;"
  >
  <figcaption>
    The squares in blue denote people we wait for, the green square is us, and the orange squares are the people we bribe.
  </figcaption>
</figure>

Think about why this is true. The problem states that we can only bribe the person immediately in front of us, and we start off behind person $n$. Once we bribe person $n$, we're then behind person $n-1$. We also can't bribe person $n-2$ *before* bribing person $n-1$!

With that in mind, let us compute the total cost when we bribe $x$ people:

$$
\sum_{i=0}^{n-x-1} A_i + \sum_{i=n-x}^{n-1} B_i
$$

We can rewrite this as:

$$
\sum{A_i} + \sum_{i=n-x}^{n-1} B_i - A_i
$$

And then the answer to the question will be the minimum value of this over all choices for $x$:

$$
\sum{A_i} + \min_{0 \le x \le n} \sum_{i=n-x}^{n-1} B_i - A_i
$$

We can compute this in $\mathcal{O}(n)$ time with a running suffix minimum.

## Updates?

We require a data structure that can query the minimum suffix sum and perform point updates, and the segment tree is a perfect fit. 

The solution to [CSES: Prefix Sum Queries](https://cses.fi/problemset/task/2166) uses a *harder* version of the technique we'll need to use here, and you should be somewhat familiar with segment trees to understand what's next.

Each node will store two values: the minimum suffix sum of the node's range (`best`) and the sum of the whole range (`sum`). We can then combine nodes like this:

```cpp
struct node {
  int sum, best;
  // min(0, v) allows for an empty suffix
  // may be better than picking a +ve element
  node(int v) : sum(v), best(min(0, v)) {}
}

node operator+(const node &l, const node &r) {
  node ans;
  ans.sum = l.sum + r.sum;
  // we either pick the best right suffix, or the whole
  // right side along with the best left suffix
  ans.best = min(r.best, l.best + r.sum);
}
```

To find the answer, read `best` from the root. **I have intentionally omitted many details** because the problem is almost identical to the CSES problem linked above, so you should solve that to better understand this!

The time complexity of this solution is $\mathcal{O}(n \log{n})$.
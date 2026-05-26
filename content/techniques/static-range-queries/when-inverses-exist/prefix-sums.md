---
draft: false
title: 'Prefix sums'
weight: 1
---

In this article, we are going to focus on the following problem (or variations of it):
 
You are given an array $A$ of length $N$, along with $Q$ queries. In each query, you are given a range $[l, r]$ $(1 \leq l, r \leq N)$ and want to compute the sum $A_l + A_{l+1} \ldots + A_N$ (referred to as `sum(l, r)` from now on). How do we accomplish this in $O(1)$ time per query? 

You are encouraged to try tackling this problem on your own! It's a subproblem that will come up again and again while you're doing other problems. 

<problem>cses-static-range-sum-queries</problem> 

The naive solution is to iterate over all indices from $l$ to $r$, and add each value one by one. However, as each query takes a maximum of $O(N)$ time, our total time complexity will be $O(NQ)$. In the problem provided above, this means $4 \cdot 10^{10}$ operations &mdash; way too slow!

Instead, let's approach the problem from another angle. You might have noticed how there's a lot of redundant information that we compute over and over again. Assuming we already know `sum(l, a)` and `sum(l, b)` with $a < b$, we can already figure out that

$
 \text{sum(a+1, b)} =  \text{sum(l, b)} - \text{sum(l, a)} 
$
`sum(l, a)` is included in both sums, so they cancel out &mdash; leaving behind `sum(a+1, b)`. But $a$ and $b$ could be any number, so if we could choose $l = 1$ and precompute `sum(1, r)` for each value of `r`, we would be able to answer each query in $O(1)$ time!

That might have been a bit too fast, so let's go over an example together. 

Assume the array $A$ is as the following, with $N = 5$:

$
A = [5, 42, 3, 7, 1]
$

Then, the prefix sum array $P$ will be as follows: 

$
P = [5, 5+42, \ldots, 5+42+3+7+1] = [5, 47, 50, 57, 58]
$
Now, if we want to calculate `sum(2, 4)`$\ldots$

$
[5, \textcolor{green}{42}, \textcolor{green}{3}, \textcolor{green}{7}, 1]
$

We can just use $P[4] - P[1] = 57 - 5 = 52$!

$
[\textcolor{red}{5}, \textcolor{green}{42}, \textcolor{green}{3}, \textcolor{green}{7}, 1]
$


## Implementation

Notice how you can't build the prefix sum array naively by summing each prefix over and over again (that would take $O(n^2)$ time). Instead, observe how $P[i+1] = P[i] + A[i+1]$; thanks to this, we can build our prefix sum array in $O(n)$ total. 

In addition, using 1-based indexing with $P[0] = 0$ simplifies building and querying the prefix sum array. 

Here's an example of how to implement this in C++:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int N, Q;
    cin >> N >> Q;
    vector<int> A(N + 1);
    for(int i = 1; i <= N; ++i) {
        cin >> A[i];
    }
    vector<long long> P(N + 1); // beware of overflow!
    for(int i = 1; i <= N; ++i) {
        P[i] = P[i - 1] + A[i]; // build the prefix sum array
    }
    for(int query_num = 0; query_num < Q; ++query_num) {
        int l, r;
        cin >> l >> r;
        cout << P[r] - P[l - 1] << "\n"; // queries are very simple!
    }
}
```

You can also use [std::partial_sum](https://en.cppreference.com/w/cpp/algorithm/partial_sum) to build the prefix sum array. Be careful about overflow though &mdash; it uses the value type pointed to by the input iterator, so `A` being a vector of integers might present issues.
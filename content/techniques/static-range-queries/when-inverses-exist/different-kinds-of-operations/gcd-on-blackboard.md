---
draft: false
title: 'AtCoder: GCD on Blackboard'
editorial:
  platform: "AtCoder"
  name: "GCD on Blackboard"
weight: 1
---

{{< problem "atcoder-gcd-on-blackboard" >}}

As always, try to solve the problem yourself before continuing!

The key point here is to think of each operation as outright _deleting_ a number, instead of replacing it. Indeed, note how 
$$
\gcd(a,b) \leq \min(a, b) \implies \gcd(A_1, A_2, \ldots A_N) \geq \gcd(A_1, A_2, \ldots A_{i-1}, A_{i+1}, \ldots A_N)
$$

Then, replacing $A_i$ with $\gcd(A_1, A_2, \ldots A_{i-1}, A_{i+1}, \ldots A_N)$ would be optimal, which is the same thing as deleting it altogether.

Now, we are faced with a new challenge: How do we compute the GCD of the remaining numbers efficiently? Your intuition might guide you towards computing the GCD of the entire array first and removing elements one by one, but it is very hard to reverse GCD operations. Instead, let's phrase the problem differently:

$$
\gcd(A_1, A_2, \ldots A_{i-1}, A_{i+1}, \ldots A_N) = \gcd(\gcd(A_1, \ldots A_{i-1}), \gcd(A_{i+1}, \ldots A_N))
$$

In other words, the remaining GCD after deleting element $i$ is the same thing as combining the GCDs of the prefix at $i-1$ and the suffix at $i+1$, respectively. Wait... we can calculate each of these separately via prefix/suffix GCD arrays! After that, all that remains is to calculate the answer for each index separately and output the maximum. 

### Implementation in C++
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> a(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
    }
    vector<int> suffix(n + 2), prefix(n + 1); // extra padding at the end of suffix gcd array
    for(int i = 1; i <= n; ++i) {
        prefix[i] = gcd(prefix[i - 1], a[i]);
    }
    for(int i = n; i >= 1; --i) {
        suffix[i] = gcd(suffix[i + 1], a[i]); // without padding, this is undefined for i = n
    }
    int ans = -1;
    for(int i = 1; i <= n; ++i) {
        ans = max(ans, gcd(prefix[i - 1], suffix[i + 1]));
    }
    cout << ans << "\n";
}
```
<br>
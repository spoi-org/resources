---
draft: false
title: 'CSES: Subarray Sums II'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Subarray Sums II"
weight: 5
---

{{< problem "cses-subarray-sums-ii" >}}

Again, tackle the problem yourself first!

The naive solution is to iterate over all possible ranges one by one, summing everything up in the process. However, there are $\Theta(N^2)$ distinct ranges, and each range takes up to $O(N)$ time to process. With a final complexity of $O(N^3)$, this is too slow. Even if we used $O(1)$ time per range using prefix sums, it would still take $O(N^2)$ in total.

You might have noticed how, in some sense, this problem is the first problem we focused on &mdash; just reversed. Instead of trying to calculate the sum of a given range, we want to count ranges with a given sum. 

Remember how in our prefix sum array, we had $P[r] - P[l - 1] = \text{sum(l, r)}$. Put another way, $P[l - 1] = P[r] - \text{sum(l, r)}$. This implies that if we could efficiently query the number of previous prefixes with sum $P[r] - x$ at each point $r$, we could easily solve the problem. There are different ways to do this &mdash; the simplest is to use the built-in STL data structure [std::map](https://en.cppreference.com/w/cpp/container/map). You might want to consult documentation in your respective language if you don't use C++. Alternatively, it is also possible to use [binary search](/techniques/binary-search/basics) to do this, though the code is somewhat longer and not as clean. 

### Implementation in C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, x;
    cin >> n >> x;
    vector<int> a(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
    }
    long long sum = 0;
    map<long long, int> cnt;
    cnt[0] = 1; // for subarrays with l = 1
    long long ans = 0;
    for(int i = 1; i <= n; ++i) {
        sum += a[i]; // current prefix sum
        ans += cnt[sum - x];
        cnt[sum]++;
    }
    cout << ans << "\n";
}
```
<br>

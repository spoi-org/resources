---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Two-sum'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Sum of Two Values"
weight: 1
---

{{< problem "cses-sum-of-two-values" >}}

## Abridged problem statement

Given a list ($a$) of $n$ numbers and a number $x$, find distinct indices $i$ and $j$ such that $a[i]+a[j]=x$, if they exist.

## Solution

A naive solution would iterate over all $\mathcal{O}(n^2)$ pairs of $(i,j)$ values. However, after fixing one of the indices ($i$), it is straightforward to realise that we must find $j$ such that $j \neq i$ and $a[j]=x-a[i]$.

Then, we can store pairs $(a[i], i)$ in a sorted vector and use `std::lower_bound` to find where a given value $v$ appears.

<details>
<summary>Please elaborate!</summary>

Pairs are sorted lexicographically. So, for example, if our array was $[2,2,0,1,2]$, then our array of pairs would be $[(2,0),(2,1),(0,2),(1,3),(2,4)]$. This, sorted, is $[(0,2),(1,3),(2,0),(2,1),(2,4)]$.

Notice that all occurences of equal values are grouped together. In addition to this, in a group of equal values, indices are sorted ascendingly. This is why we can use binary search to find the first occurence of a given value $v$.

For further context, read [this](/techniques/binary-search/stl).
</details>

## Code
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n, x;
  cin >> n >> x;
  vector<int> a(n);
  for (int &i : a) {
    cin >> i;
  }

  vector<pair<int, int>> buf;
  for (int i = 0; i < n; ++i) {
    buf.push_back({a[i], i});
  }
  sort(buf.begin(), buf.end());

  for (int i = 0; i < n; ++i) {
    int v = x - a[i];
    pair<int, int> p = {v, -1};
    auto it = lower_bound(buf.begin(), buf.end(), p);
    if (it == buf.end() || it->first != v || it->second == i) {
      continue;
    }
    cout << it->second + 1 << ' '  << i + 1  << '\n';
    return 0;
  }
  cout << "IMPOSSIBLE\n";
}
```

We use $(v, -1)$ because we wish to find the first occurence of $v$ in $a$. Setting the second element in the pair as $-1$ ensures that we are smaller than any other pair $(v, i)$ that actually exists in `buf`, which makes `std::lower_bound` work correctly.

The time complexity of this code is $\mathcal{O}(n \log{n})$, and its space complexity is $\mathcal{O}(n)$.

## Notes

- It is also possible to solve this problem using `std::map`; this solution involves mapping a value to index map and also checking if $s-a[i]$ exists among $j<i$, same as our solution.
- You can also use an incremental pointer to move along `buf` if you process values in ascending order, but this is slightly more complicated and does not change the asymptotic complexity, as sorting still incurs $\mathcal{O}(n \log{n})$.
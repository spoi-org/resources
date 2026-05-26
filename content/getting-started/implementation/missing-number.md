---
draft: false
title: 'Use some math'
editorial:
  platform: "CSES"
  category: "Introductory Problems"
  name: "Missing Number"
weight: 1
---

{{< problem "cses-missing-number" >}}

## Abridged problem statement

Given all numbers from $1,2,\dots ,n$ except one, find the missing number.

## Solution

There are multiple ways to solve this problem. One of the simplest is to recall the identity

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$

so we sum up all input numbers and subtract this from the sum of the first $n$ numbers.

```cpp
#include <bits/stdc++.h>

using namespace std;

using int64 = long long;

int main() {
  int n;
  cin >> n;
  int64 sum = 0;
  for (int i = 0, x; i < n - 1; ++i) {
    cin >> x;
    sum += x;
  }
  cout << int64(n) * (n + 1) / 2 - sum << '\n';
}
```

There are two noteworthy implementation details here:

- First, we use `int64` for the variable `sum`. This is because, for $n = 2 \cdot 10^5$, the value of the sum is on the order of $10^{10}$, which exceeds the limit of a regular `int`. As a general rule of thumb, sums of many numbers should usually be stored in wider integer types.
- Secondly, while printing the answer, $n$ needed to be casted to an `int64`. Again, this is to prevent overflow: otherwise, the multiplication `n * (n + 1)` would be evaluated using regular `int` arithmetic.
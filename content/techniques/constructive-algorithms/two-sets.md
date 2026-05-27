---
draft: false
title: 'Two sets'
editorial:
  platform: "CSES"
  category: "Introductory Problems"
  name: "Two Sets"
weight: 1
---

{{< problem "cses-two-sets" >}}

## Abridged problem statement

Divide the numbers $1,2,3,\cdots,n$ into two sets of equal sum. If this is possible, print `YES` followed by a valid division. If it is not, print `NO`.

## Solution

To begin with, notice that if

$$
\frac{n(n+1)}{2}
$$

that is, the total sum of the numbers, is odd, the answer is always `NO`, because we cannot divide an odd number into two parts.

On the other hand, if we can produce a valid construction whenever the sum is even, we would be done.

Let us find the first triangular number $x$ such that

$$\frac{x(x+1)}{2} \ge \frac{n(n+1)}{4}$$

so

$$x=\left\lceil \frac{\sqrt{1+2n(n+1)}-1}{2}\right\rceil$$

Now, we claim that

$$\frac{x(x+1)}{2} - \frac{n(n+1)}{4} \le x$$

Indeed, on solving the above inequality, we get 

$$x\le \left\lfloor \frac{\sqrt{1+2n(n+1)}+1}{2}\right\rfloor$$

and since

$$
\left\lceil \frac{c-1}{2} \right\rceil
\le
\left\lfloor \frac{c+1}{2} \right\rfloor$$

for all real $c$, the statement is true. Therefore, we sum up the first $x$ numbers, and then subtract whatever the difference is between our target half-sum and our actual current sum, and add the remaining numbers to the second set.

## A more intuitive explanation

The same idea can be proved without any complicated maths if we approach it from the perspective of constructing the other set directly.

That is, we iterate a variable $i$ from $n$ down to $1$, desiring a total sum of $\frac{n(n+1)}{4}$. Call the currently needed sum $T$, initially $\frac{n(n+1)}{4}$.

- If $i \le T$, we pick $i$ and subtract it from $T$.
- Otherwise, $T < i$. Since we are iterating downward, the number $T$ itself is still unused, so we can simply pick $T$ and immediately achieve our target sum.

In fact, we can use this exact same approach to construct any sum from $1$ to $\frac{n(n+1)}{2}$.

## Code
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int64_t n;
  cin >> n;

  if ((n * (n + 1) / 2) % 2 == 1) {
    cout << "NO\n";
    return 0;
  }

  int x = ceil((sqrt(1 + 2 * n * (n + 1)) - 1) / 2);
  int exclude = x * (x + 1) / 2 - n * (n + 1) / 4;
  
  vector<int> a, b;
  for (int i = 1; i <= n; ++i) {
    (i != exclude && i <= x ? a : b).push_back(i);
  }

  cout << "YES\n" << a.size() << '\n';
  for (int &i : a) {
    cout << i << ' ';
  }
  cout << '\n' << b.size() << '\n';
  for (int &i : b) {
    cout << i << ' ';
  }
  cout << '\n';
}
```
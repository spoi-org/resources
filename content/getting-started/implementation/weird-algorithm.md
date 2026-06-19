---
draft: false
title: 'Weird Algorithm'
editorial:
  platform: "CSES"
  category: "Introductory Problems"
  name: "Weird Algorithm"
weight: 1
---

{{< problem "cses-weird-algorithm" >}}

## Abridged problem statement

Implement the following procedure: given a number $n$, repeatedly set it to $\frac{n}{2}$ if it is even, otherwise to $3n+1$, while is not equal to $1$. Also print the value of $n$ at each intermediate step.

## Solution

For those interested, this question is directly inspired by the [Collatz conjecture](https://en.wikipedia.org/wiki/Collatz_conjecture), an extremely popular conjecture in mathematics that predicts that the process described above always ends with $n=1$, no matter the starting value of $n$. While most believe this is true, it hasn't yet been proven, only verified for up to $n \le 10^{21}$.

We implement what the question asks us to do:
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n;
  cin >> n;
  while (n != 1) {
    cout << n << ' ';
    if (n % 2 == 0) {
      n /= 2;
    } else {
      n = 3 * n + 1;
    }
  }
  cout << "1\n";
}
```

But wait, when we submit this, we get a wrong answer?!

Indeed, this is because we have used the wrong data type. Our code uses `int`, which can only fit a 32-bit integer, but during the execution of our code, it is possible for $n$ to become larger than $\approx 2\cdot 10^9$, the limit for an `int`. To fix this, we must use a 64-bit data type, like so:

```cpp
#include <bits/stdc++.h>

using namespace std;

using int64 = long long;

int main() {
  int64 n;
  cin >> n;
  while (n != 1) {
    cout << n << ' ';
    if (n % 2 == 0) {
      n /= 2;
    } else {
      n = 3 * n + 1;
    }
  }
  cout << "1\n";
}
```

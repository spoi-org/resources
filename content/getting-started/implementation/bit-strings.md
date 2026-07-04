---
draft: false
title: 'Bit Strings'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Bit Strings"
weight: 1
---

{{< problem "cses-bit-strings" >}}

## Solution

Each of the n positions has 2 values it can be, either 0 or 1. The answer is going to be $\underbrace{2 \\times 2 \\times 2 \\times ... \\times 2}_{n "times"}$ because its 2 values for each character. We compute the answer iteratively while taking remainders modulo $10^9+7$ to avoid overflow.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    const long long MOD = 1e9 + 7;
    long long n;
    cin >> n;

    long long answer = 1;
    for (long long i = 0; i < n; ++i) {
        answer = (answer * 2) % MOD;
    }

    cout << answer << "\n";
    return 0;
}
```

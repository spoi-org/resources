---
draft: false
title: 'Coin Piles'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Coin Piles"
weight: 1
---

{{< problem "cses-coin-piles" >}}

## Solution

The first observation is that each time you remove 2 coins from pile A and 1 coin from pile B or 1 coin from pile A and 2 coins from pile B, the total number of coins in both the towers always gets reduced by 3 so to empty both piles. 
**Therefore the sum of coins in the two piles must be divisible by 3.**

The second observation is that if the number of coins in one pile is more than twice the number of coins in the other pile, you cannot empty the bigger pile even if you remove 2 coins from the bigger pile for each time you remove 1 coin from the smaller pile.
**Therefore the number of coins in the larger pile must be less than or equal to twice the number of coins in the smaller pile.**

We check if the above two conditions are met and accordingly output the result.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;

    while (t--) {
        int a, b;
        cin >> a >> b;

        bool condition1 = (a + b) % 3 == 0;//sum of coins in pile is a multiple of 3.
        bool condition2 = max(a, b) <= 2 * min(a, b);//number of coins in bigger mile must be less than or equal to twice the number of coins in the smaller pile.

        if(condition1 && condition2) 
          cout<< "YES" << "\n";
        else
          cout<< "NO" << "\n";
    }

    return 0;
}
```

---
draft: false
title: 'Collecting Numbers'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Collecting Numbers"
weight: 1
---

{{< problem "cses-collecting-numbers" >}}

## Solution

The program stores the index of each number in the order it appears. It then scans numbers from 1 to n and checks whether a number appears before its predecessor. Whenever this happens, a new round is required. The final count represents the total number of rounds needed.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {

    int n;
    cin >> n;

    vector<int> position(n + 1);
    for (int i = 0; i < n; i++) {
        int value;
        cin >> value;
        position[value] = i;
    }

    int rounds = 1;
    for (int i = 2; i <= n; i++) {
        if (position[i] < position[i - 1]) {//if the larger number
            rounds++;
        }
    }

    cout << rounds;

```

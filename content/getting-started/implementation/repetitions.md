---
draft: false
title: 'Repetitions'
editorial:
platform: "CSES"
category: "Introductory Problems"
name: "Repetitions"
weight: 1
---

{{< problem "cses-repetitions" >}}

## Solution

To find the longest repetition, we can go through each character of the string. At each character, we need to keep track of:

1. The previous character
2. The length of the current repetition.

If the current character is the same as the previous character, we can **increase** the length of the current repetition by 1. If it's different, we **reset** the length of the current repetition to 1.

We also keep track of the maximum repetition by updating it whenever the current repetition is greater than the maximum repetition.

### Code:

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    string s;
    cin >> s;
    int maxRep = 1, cur = 1;

    for (int i = 1; i < s.length(); i++) {
        // Check if the current character matches the previous one.
        // Increment the current repetition of consecutive characters.
        if (s[i] == s[i - 1])
            cur++;
        // Reset cur to 1 if characters differ.
        else
            cur = 1;

        // Updates maxRep if cur is larger
        maxRep = max(maxRep, cur);
    }

    cout << maxRep << "\n";
    return 0;
}
```

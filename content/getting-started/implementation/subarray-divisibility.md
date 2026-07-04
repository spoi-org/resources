---
draft: false
title: 'Subarray Divisibility'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Subarray Divisibility"
weight: 1
---

{{< problem "cses-subarray-divisibility" >}}

## Hint

If you solved the question above(Subarray Sums II), try using the same technique of storing frequencies, but figure out what you should be storing the frequency of so that you can figure out a subarrays divisibility.

## Solution

We use prefix sums modulo `n` to count subarrays whose sum is divisible by `n`. Each element is first reduced modulo `n` to keep values small.

As we iterate, we maintain the current prefix sum modulo `n`. A map stores the frequency of each modulo value of the prefix sums. If the same modulo appears again, there exists a  subarray that has it's sum divisible by `n`(If the current remainders is `n` and some previous remainder is also `n`, then their difference is 0 which means it's divisible). We add the frequency of the current modulo to the answer and increase the frequency of that modulo in the map.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    long long n;
    cin >> n;

    vector<long long> arr(n);
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        arr[i] %= n;                 // keep values within modulo n
    }

    unordered_map<long long, long long> frequency;
    frequency[0] = 1;               // empty prefix sum

    long long prefixSum = 0;
    long long result = 0;

    for (int i = 0; i < n; i++) {
        prefixSum = (prefixSum + arr[i]) % n;
        if (prefixSum < 0) prefixSum += n;

        result += frequency[prefixSum];
        frequency[prefixSum]++;
    }

    cout << result << "\n";
    return 0;
}
```

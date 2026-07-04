---
draft: false
title: 'Digit Queries'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Digit Queries"
weight: 1
---

{{< problem "cses-digit-queries" >}}

## Solution

The trick is to notice that numbers form blocks by digit-length: 1–9 (1-digit), 10–99 (2-digit), 100–999 (3-digit), and so on. Each block has a predictable number of digits (1-9 has 9 digits, 10-99 has 90×2 = 180 digits and so on), so the code keeps subtracting whole blocks until the target digit falls inside one specific block. 

Once the block is located, it directly computes which exact number contains the digit. This is achieved because the correct block has numbers of length $l$ . The number of places you have to jump ahead from the start is $(n-1)/l$. $n-1$ is used instead of $n$ because $n$ is one indexed but the jump amount is 0-indexed.

Once the correct number has been identified, the correct digit in that number assuming the digits are 0-indexed from left right right is $n-1 mod l$. Once again we use $n-1$ because of 1-indexing.

**Code **

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {

    int q;
    cin >> q;

    while (q--) {
        ll n;
        cin >> n;

        ll count = 9;     // how many numbers exist with current digit-length
        ll len = 1;       // current digit-length
        ll start = 1;     // first number of current digit-length

        // skip full digit-blocks (e.g., 1–9, then 10–99, then 100–999...)
        while (n > count * len) {
            n -= count * len;  // remove whole block worth of digits
            start *= 10;       // move to next block's starting number
            count *= 10;       // next block has 10× more numbers
            len++;             // numbers now have one more digit
        }

        // find the exact number containing the nth digit
        ll num = start + (n - 1) / len;

        // pick the specific digit inside that number
        string s = to_string(num);
        cout << s[(n - 1) % len] << "\n";
    }
}
```

Here's an example to make it clearing as to how the code and approach work:

The sequence is 123456789101112131415...

Initial values: `n = 15`, `count = 9`, `len = 1`, `start = 1`

After skipping 1-digit block:
- Block 1-9 contributes $9 \\times 1 = 9$ digits
- Update: `n = 15 - 9 = 6`, `start = 10`, `len = 2`

Find the number:
- `num = 10 + (6-1)/2 = 10 + 2 = 12`

Find the digit:
- `s = "12"`, `index = (6-1) % 2 = 1`
- Answer: `s[1] = '2'`

Verification: 
The sequence is 123456789|10|11|12|13... $->$ position 15 is the 2nd digit of 12 = 2

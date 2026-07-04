---
draft: false
title: 'Gray Code'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Gray Code"
weight: 1
---

{{< problem "cses-gray-code" >}}

## Hint

Trying listing out the solution for $n = 1$, then for $n = 2$ and $n = 3$. Try to see if there is any pattern from the previous smaller sequences to the larger ones. You might even find a pattern just by looking at any one value of $n$. 

## Solution 1

The first pattern you may have spotted when you attempt to solve the question was the way the sequences of a longer Gray code build up on the sequence of smaller Gray code. 

Take the Gray code for $n = 2$:

$
00
01
11
10
$

If you now look at the Gray code for $n = 3$, you'll notice that the gray code for $n = 2$ appears in the list:

<!-- Diagram: The Gray code for n = 3 listed vertically. The first four entries (000, 001, 011, 010) are highlighted in red — the n = 2 code with 0 prepended. The last four entries (110, 111, 101, 100) are highlighted in blue — the n = 2 code reversed with 1 prepended. -->

$
000
001
011
010
110
111
101
100
$

The first 4 strings in the Gray code for $n = 3$ is just the Gray code for $n = 2$ (shown in red) with 0 prepended to it. The last 4 string in the Gray code for $n = 3$ is just the gray code for $n = 2$ but backwards (shown in blue). This pattern applies to all gray codes. 

Here's the code for this approach:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n;
    cin >> n;

    vector<string> gray;
    gray.push_back("");

    for (int i = 0; i < n; i++) {
        vector<string> next;

        // Prefix "0" to all existing strings
        for (int j = 0; j < gray.size(); j++)
            next.push_back("0" + gray[j]);

        // Prefix "1" to all existing strings in reverse order
        for (int j = gray.size() - 1; j >= 0; j--)
            next.push_back("1" + gray[j]);

        gray = next;
    }

    for(int i = 0; i < gray.size(); i++)
        cout << gray[i] << "\n";
}
```

From the code, we start by saying `gray = {""}`. Then to generate the next gray code which we store in `next`, we prepend a `0` to every string in `gray` and store that in `next` and then we prepend a `1` to every element in `gray` while going backwards and sore that in `next`. Finally we update `gray` to next. 

This process is repeated until you get the $n$th Gray Code which you then output.

The time complexity of this solution is $O(n \\cdot 2^n)$ and the space complexity is $O(n)$. While this is pretty good and will pass all the test cases within the time limit, we can do a bit better.

## Solution 2

Let's look again at the Gray code for $n = 3$ and highlight where the bit flips occur going from 000 to the end:

<!-- Diagram: The Gray code for n = 3 with bit indices 2, 1, 0 shown above the strings. Each flipped bit position between consecutive strings is highlighted in red. -->

$
210
000
001
011
010
110
111
101
100
$

The numbers in gray at the top, represent the index of each bit. If we list the index of which bit was flipped from on string to the other, we get: ${0, 1, 0, 2, 0, 1, 0}$. In this list, you can see that we flipped the 0th bit every 2nd bit flip, the 1st bit every 4th bit flip, and if we were to look at Gray code of $n = 4$, we would also notice that the 2th bit was flipped every 8th time.

This sequence has a pattern that can be expressed with the following expression:

$
  t_n = \log_2(\text{lsb}(n)), \quad \text{where } t_n \text{ is the } n\text{th term.}
$

We can use this formula to calculate the position of which bit we need to flip to generate the next string. The advantage of this method is that we don't need to compute the Gray codes for smaller values and we can just directly output the answer for the given $n$ value. 

Here's the code for this approach:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    int n;
    cin >> n;
    string s(n, '0');// Start with all zeros

    cout << s << endl;
    for (int i = 1; i < (1 << n); i++) {
        // Find position of lowest set bit and flip the corresponding bit
        int pos = n - 1 - __builtin_ctz(i & -i);
        s[pos] = (s[pos] == '0' ? '1' : '0');// Flip the bit.
        cout << s << "\n";
    }
    return 0;
}
```

`i & -i` computes $"LSB"(n)$ and `__builtin_ctz()` computes the number of trailing zeros which is the same as `log2()` for a power of 2 which the LSB($n$) is. Finally we must subtract this from $n - 1$ to convert it to the correct index in the string because strings are indexed left to right but our formula indexes the bit's from right to left.

The time complexity of this code is $O(2^n)$ which is a bit faster than the first approach. For the last testcase of the problem on the website, the first code takes 0.03s whereas this one runs in 0.01s.

---
draft: false
title: 'Reading Books'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Reading Books"
weight: 1
---

{{< problem "cses-reading-books" >}}

## Solution

If one book takes more than half the total time, one child will be forced to wait while the other finishes that long book. Otherwise, they can optimally interleave their reading with no idle time. Thus, the answer is $max("total_time", 2 * "longest_book")$.

The rigorous reason for why it's possible to optimally interleave their reading with no idle time is as follows:

Let $b_1, b_2, ... b_n$ be the $n$ books in order of smallest to largest.
Let person one read the books in order of largest to smallest : $b_n, b_(n-1), b_(n-2), ... b_1$
Let person two read the books in the same order except the read the longest book $b_n$ at the end : $b_(n-1), b_(n-2), ... b_1, b_n$

Person one will never catchup to the same book as person two because they are always going to be reading a book that is at least as long and the book person two is reading. The only time a conflict arises is when person 2 goes to read $b_n$. 

If $b_n <= "sum" - b_n$, then person one would've moved on before person two comes around to reading $b_n$; otherwise person 2 will have to wait for person one to finish. The inequality can be rearranged as $2 \\cdot b_n <= "sum"$ which when true results in an answer of sum, else the answer is $2 \\cdot b_n$.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<long long> books(n);
    long long total = 0, max_book = 0;

    // Read book times, track:
    // 1) total time of all books
    // 2) the longest single book
    for (int i = 0; i < n; i++) {
        cin >> books[i];
        total += books[i];
        max_book = max(max_book, books[i]);
    }

    // Minimum total time is governed by:
    // - total (one person reading sequentially)
    // - OR twice the largest book (two-person parallel reading constraint)
    // The answer is the max of these two.
    cout << max(total, 2 * max_book) << "\n";

    return 0;
}

```

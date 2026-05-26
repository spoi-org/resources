---
draft: false
title: 'Difference arrays'
weight: 2
---

## Updates?

In the problem above, we didn't have to worry about updating the array between queries. What if there were also queries that required us to update an element? What if we had to update a _range_ of elements?

Using our current approach, updates are rather expensive. A single change to the an element affects the entire suffix of the prefix sum array starting at that element &mdash; requiring us to build it from scratch. But wait... a single change affects the entire suffix? That would be incredibly convenient for updates, _if we were trying to update the prefix sum array_. Then, what if we could build an array $D$ such that the prefix sum array of $D$ was $A$?

Indeed, notice how $P[i] - P[i - 1] = A[i]$. Then, we could build the array $D$ such that $A[i] - A[i - 1] = D[i]$. Now we have the magic power of updating suffixes with a single touch! (to update a range, first update the suffix $[l, n]$; then undo the update on the suffix $[r+1,n]$.)

Though obviously, this approach also has drawbacks. Although we can perform updates very quickly, if we want to actually know the value of an element at index $i$, we have to start from $D[1]$ and sum everything up until $D[i]$. So we have achieved the "reverse" of what a prefix sum array does, in some sense: we can do range updates on the array in $O(1)$, but queries take $O(n)$. This is called a "difference array" &mdash; not as common as prefix sums, but a cool trick nonetheless. 

### Difference Array Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int N, Q;
    cin >> N >> Q;
    vector<int> A(N + 1), D(N + 1);
    for(int i = 1; i <= N; ++i) {
        cin >> A[i];
        D[i] = A[i] - A[i - 1]; // build the difference array
    }
    for(int query_no = 0; query_no < Q; ++query_no) {
        int l, r, delta; // delta is how much we should increase the range by
        cin >> l >> r >> delta;
        D[l] += delta; // update the suffix at l
        if(r < N) {
            D[r+1] -= delta; // undo the update for the suffix at r
        }
    }
    // output the final array
    int sum = 0;
    for(int i = 1; i <= N; ++i) {
        sum += D[i];
        cout << sum << " \n"[i == N];
    }
}
```
<br><br>

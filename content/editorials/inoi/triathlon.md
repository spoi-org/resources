---
draft: false
title: 'Triathlon'
editorial:
  platform: "INOI"
  name: "Triathlon"
---

*Editorial written by Roumak Das.*

{{< problem "inoi-triathlon" >}}

Every citizen goes through three tracks in the same order: programming, then pole vault, then doughnut eating. Citizen $i$ needs $a_i$, $b_i$ and $c_i$ time on them. There is only one computer, so at most one person can be programming at any moment, and we get to decide the queue. The other two tracks have room for everybody at once. We want the last person to finish as early as possible.

## Where the time actually goes

Let's fix an order $p_1, p_2, \ldots, p_N$ and follow whoever is $k$-th in the queue. They cannot touch the computer until everyone ahead of them is finished with it, so they start at $a_{p_1} + \cdots + a_{p_{k-1}}$ and hold it for $a_{p_k}$. After that nothing delays them, since the last two tracks have no capacity limit, so they finish at

$$
\left( \sum_{j=1}^{k} a_{p_j} \right) + b_{p_k} + c_{p_k}
$$

The event ends when the slowest of them is done, so the answer is the maximum of this over all $k$.

Try to see what you can do with this formula. The solution is mostly hidden in it.

<details>
<summary>Hint 1</summary>

Look at how $b_{p_k}$ and $c_{p_k}$ appear. They never show up apart from each other, only ever as
a sum. So a citizen is really just two numbers: the time $a_i$ they block the computer for, and
$w_i = b_i + c_i$ they spend on their own afterwards.

</details>

<details>
<summary>Hint 2</summary>

Whatever order you choose, the prefix sums of $a$ are built from the same $N$ values, so the
multiset of prefix totals never changes. Reordering does not change which prefixes exist. All it
changes is which $w$ gets paired with which prefix.

So the real question is how to pair them.

</details>

<details>
<summary>Key idea</summary>

<Aside type="tip" title="Key idea">
Sort by $w_i = b_i + c_i$ in descending order.
</Aside>

Someone with a long tail after the computer should get to it early, so their tail runs while
everyone else is still queueing. Notice that $a$ plays no part in the rule at all.
</details>


## Why the sorted order is optimal

As in the key idea above, write $w_i = b_i + c_i$ for the time citizen $i$ spends after leaving
the computer.

**Claim.** If $w_i \ge w_j$, then scheduling $i$ immediately before $j$ is never worse than the
other way round.

**Proof.** Take any order, and pick two citizens $i$ and $j$ standing next to each other. Let $P$
be the total $a$ of everyone ahead of both of them, and let $S = a_i + a_j$.

Swapping the two disturbs nobody else. People ahead are untouched, and people behind still get the
computer at $P + S$, since the pair occupies it for $S$ either way. So we only have to compare the
two finish times inside the pair.

With $i$ first, the two finish at $P + a_i + w_i$ and $P + S + w_j$. With $j$ first, they finish at
$P + a_j + w_j$ and $P + S + w_i$.

Assume $w_i \ge w_j$, and compare everything against $P + S + w_i$, which the second arrangement
always pays:

- $P + a_i + w_i \le P + S + w_i$, because $a_j \ge 0$
- $P + S + w_j \le P + S + w_i$, because $w_i \ge w_j$

Both finish times of the first arrangement are at most something the second arrangement pays
anyway, so putting the bigger $w$ first is never worse.

This only talks about neighbours, but that is enough. If an order is not sorted by descending $w$,
then some adjacent pair in it is in the wrong relative order, and swapping that pair cannot
increase the answer. Repeat the swap until the order is fully sorted. Nothing got worse along the
way, so the sorted order is optimal. Ties in $w$ can go either way.

## Implementation

Sort with the rule above, then walk through once carrying a running prefix sum.

```cpp title="triathlon.cpp"
#include <bits/stdc++.h>
using namespace std;

bool compare(const vector<long long> &p, const vector<long long> &q) {
    return p[1] + p[2] > q[1] + q[2];
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;

    vector<vector<long long>> v(n, vector<long long>(3));
    for (int i = 0; i < n; i++) {
        cin >> v[i][0] >> v[i][1] >> v[i][2];
    }

    sort(v.begin(), v.end(), compare);

    long long prefix = 0;
    long long ans = 0;
    for (int i = 0; i < n; i++) {
        prefix += v[i][0];
        long long finish = prefix + v[i][1] + v[i][2];
        if (finish > ans) {
            ans = finish;
        }
    }

    cout << ans << '\n';
}
```

{{< caution "Watch the accumulator" >}}
The worst case is $200000 \times 10000 + 20000 = 2000020000$, which does fit in a 32-bit `int`,
but with under 7% of room left before `INT_MAX`. Using 64-bit costs nothing and saves you from
redoing that bound during a contest.
{{< /caution >}}

Time complexity: $O(N \log N)$ for the sort and $O(N)$ for the sweep. Memory is $O(N)$. This fits
well inside the 2 second and 32 MB limits, and the same code clears both subtasks.

## Worked example

The sample gives $w = 13, 37, 23$ for citizens $1, 2, 3$, so sorting by descending $w$ puts them in
the order $2, 3, 1$:

| Position | Citizen | $a$ | prefix $a$ | $w$ | finish |
| :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | 2 | 23 | 23 | 37 | 60 |
| 2 | 3 | 20 | 43 | 23 | 66 |
| 3 | 1 | 18 | 61 | 13 | **74** |

The event ends at $74$, which matches the expected output.

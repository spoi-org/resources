---
draft: false
title: "Vacation"
editorial:
  platform: "ZCO"
  name: "Vacation"
---

{{< problem "zco-vacation" >}}

# ZCO 2022 - Vacation
## Subtask 4 (Q <= 5)

Notice that each cell from (A, B) to (C, D) has at least one path passing through it.
If any cell from (A, B) to (C, D) has a cost of 0, then we can use the path that passes through the cell with a cost of 0 to get a path of 0 cost.

![Grid](https://pub-e92ad040f113456098b524bb71804b18.r2.dev/editor-images/d8e0d9ec-d05d-4792-9fe5-b1fdf06d28a5-1789194441085-oFWGC5w8y4p7FKyzGWX7PiJnq4p31dyC.png)

If none of the cells from (A, B) to (C, D) is 0, then no matter which path we choose, we will get a cost of 1.

Since we check each cell between (A, B) and (C, D), the time complexity of this solution is O($N \times M$) per day/query. Since there are $Q$ queries/days, the time complexity of the solution is O($Q \times N \times M$) which passes this subtask.

## Subtask 5 (At most 10 cells in the grid have a cost of 0)

We need to check whether any of the cells between (A, B) and (C, D) has a cost of 0 with a time complexity faster than O($N \times M$) to improve the last solution and make it pass this subtask.

Since there are at most 10 cells with a cost of 0, we can store the positions of all cells with a cost of 0 and check whether any of these cells lies between (A, B) and (C, D) (Let the position be (x, y). Then, we check whether $A \le x \le C$ and $B \le y \le D$)

This will only take 10 checks since there are at most 10 cells with a cost of 0 and hence the time complexity of this solution is O($Q \times Z$) where Z is the number of zeros, which passes this subtask.

## Subtask 6 (N = 1)

Prerequisite: Prefix Sums [If you do not know prefix sums, [read this](https://resources.spoi.org.in/techniques/static-range-queries/when-inverses-exist/prefix-sums/)]

Since N = 1, this is basically a 1D array. Let's call this array $C$.

We need to check whether the cost of any of the cells from $C_a$ to $C_b$ is 0 for some a and b in O(1), using preprocessing if needed.

This can be done using prefix sums.
Let $\text{pref}_{i}$ be the number of 0s in the first $i$ elements.

The code will look something like this:
```cpp
if (C[i] == 0) pref[i] = pref[i-1]+1;
else pref[i] = pref[i-1];
```

We can do ${}\text{pref}\_{b} - \text{pref}\_{a-1}$ to find the number of 0s between a and b. If $\text{pref}\_{b} - \text{pref}\_{a-1}$ is greater than 0, we know that there is at least one 0 between a and b and the cost of the path will be 0, otherwise the cost of the path will be 1.

The time complexity of this solution is O($M$) for preprocessing and O($Q$) for processing the queries. Therefore, the total time complexity of this solution is O(M + Q), which passes this subtask.

## Subtask 8 (No additional constraints)

Prerequisite: 2D Prefix Sums

We can extend the solution of Subtask 6 to use 2D prefix sums, which allows us to query the number of 0s between (A, B) and (C, D) in O(1).

The time complexity of this solution is O($N \times M$) for preprocessing and O(Q) for the queries. Therefore, the total time complexity of this solution is O($N \times M$ + Q), which passes this subtask.

My implementation of the above solution:
```cpp
#include <bits/stdc++.h>
#define endl '\n'
#define flash ios_base::sync_with_stdio(false); cout.tie(NULL); cin.tie(NULL);
#define ll long long
#define ull unsigned ll

const ll INF = 1e18;
const ll MOD = 1e9+7;

using namespace std;

void solve()
{
    ll n, m;
    cin >> n >> m;
    vector<vector<ll>> cost(n+1, vector<ll>(m+1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++) cin >> cost[i][j];
    }

    vector<vector<ll>> pref(n+1, vector<ll>(m+1));
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++) pref[i][j] = pref[i-1][j]+pref[i][j-1]-pref[i-1][j-1]+(cost[i][j] == 0);
    }

    ll q;
    cin >> q;
    while (q--)
    {
        ll a, b, c, d;
        cin >> a >> b >> c >> d;

        ll zeros = pref[c][d]-pref[a-1][d]-pref[c][b-1]+pref[a-1][b-1];

        if (zeros >= 1) cout << 0 << endl;
        else cout << 1 << endl;
    }
}

int main()
{
	flash;
    #ifdef LOCAL
        freopen("inout/burger.in", "r", stdin);
        freopen("inout/burger.out", "w", stdout);
    #endif

    solve();

    return 0;
}
```

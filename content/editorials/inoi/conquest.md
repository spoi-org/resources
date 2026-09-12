---
draft: false
title: 'Conquest'
editorial:
  platform: "INOI"
  name: "Conquest"
---

*Editorial written by Harish Rusum.*

{{< problem "inoi-conquest" >}}

Tutaria has $N$ cities joined by $N - 1$ two-way roads, and every city is reachable from every
other, so the road network is a tree. City $i$ holds $v_i$ gold ingots. Chef then makes $Q$
independent expeditions; in the $i$-th of them he walks the unique simple route from city $a_i$ to
city $b_i$, and may loot any subset of the cities on that route, as long as he never loots two
cities joined by a road. For each expedition, report the largest total gold he can take.

Constraints: $N, Q \le 5 \cdot 10^5$ and $0 \le v_i \le 10^9$, with a time limit of 3 seconds.
Note that $a_i = b_i$ is allowed, in which case the route is a single city.

Each query is just the House Robber problem, run on the route between $a_i$ and $b_i$.

{{< problem "leetcode-house-robber" >}}

The solution to the house robber problem in brief goes as follows.

Let $\mathrm{dp}[i]$ be the maximum money obtainable from the
first $i$ houses. Looking at house $i$ alone, there are two options:

- loot it, which forbids house $i-1$ and leaves the first $i-2$ houses free: $\mathrm{dp}[i-2] + money[i]$
- skip it, which lifts every restriction house $i$ would have imposed: $\mathrm{dp}[i-1]$

$$
\mathrm{dp}[i] = \max\left(\mathrm{dp}[i-1],\ \mathrm{dp}[i-2] + money[i]\right)
$$

## Solution

The naive solution would be to just calculate this dp in $O(N)$ for every path. That would be worst case $O(N \cdot Q)$.

What we can do, is instead of walking the path for every query, we could precompute the answer for the paths between every node
and its $2^k$-th ancestor, for every $0 \le k \le \lfloor \log_2 N \rfloor$, and then do binary lifting.

In order to do this, we would have to make some change to $\mathrm{dp}[i]$, so that
$\mathrm{dp}[i] \to \mathrm{dp}[i][k][a][b]$ = maximum money obtainable on the path from node $i$ up to its
$2^k$-th ancestor, where $a, b \in \{0, 1\}$ record whether the lower and upper endpoints are barred from
being looted: $a = 1$ means node $i$ itself may not be looted, $b = 1$ means the ancestor may not be.

|  | ancestor free | ancestor barred |
| :--- | :---: | :---: |
| **$i$ free** | $\mathrm{dp}[i][k][0][0]$ | $\mathrm{dp}[i][k][0][1]$ |
| **$i$ barred** | $\mathrm{dp}[i][k][1][0]$ | $\mathrm{dp}[i][k][1][1]$ |

### Computing the DP table

A jump of $2^k$ is two jumps of $2^{k-1}$ stacked on each other. Let $m$ be the $2^{k-1}$-th ancestor of $i$:
the lower half runs from $i$ to just below $m$, the upper half from $m$ upwards. The halves are disjoint,
so the total money just adds together cleanly.

Let $x$ be the child of $m$ on the path, i.e. the top node of the lower half. The edge joining the two
halves is then just $(x, m)$.

<div style="display:flex;justify-content:center;margin:1.5rem 0;">
<svg viewBox="0 0 460 320" width="100%" style="max-width:420px;height:auto;" role="img"
     aria-label="A vertical chain from node i up to its 2^k-th ancestor, split into a lower half ending at x and an upper half starting at m, joined by the seam edge between x and m.">
  <g stroke="currentColor" fill="none" stroke-width="1.5">
    <line x1="230" y1="53"  x2="230" y2="70"/>
    <line x1="230" y1="110" x2="230" y2="117"/>
    <line x1="230" y1="143" x2="230" y2="167" stroke-dasharray="5 4"/>
    <line x1="230" y1="193" x2="230" y2="205"/>
    <line x1="230" y1="245" x2="230" y2="257"/>
    <circle cx="230" cy="40"  r="13"/>
    <circle cx="230" cy="130" r="13"/>
    <circle cx="230" cy="180" r="13"/>
    <circle cx="230" cy="270" r="13"/>
    <g opacity="0.55">
      <path d="M175 283 H163 V167 H175"/>
      <path d="M175 143 H163 V27 H175"/>
    </g>
    <path d="M243 155 H280" opacity="0.55"/>
  </g>
  <g fill="currentColor" stroke="none" font-size="13">
    <circle cx="230" cy="82"  r="2"/><circle cx="230" cy="90"  r="2"/><circle cx="230" cy="98"  r="2"/>
    <circle cx="230" cy="218" r="2"/><circle cx="230" cy="226" r="2"/><circle cx="230" cy="234" r="2"/>
    <text x="230" y="45"  text-anchor="middle" font-style="italic">a</text>
    <text x="230" y="135" text-anchor="middle" font-style="italic">m</text>
    <text x="230" y="185" text-anchor="middle" font-style="italic">x</text>
    <text x="230" y="275" text-anchor="middle" font-style="italic">i</text>
    <text x="155" y="229" text-anchor="end" opacity="0.75">lower half</text>
    <text x="155" y="89"  text-anchor="end" opacity="0.75">upper half</text>
    <text x="286" y="159" opacity="0.75">seam edge</text>
    <text x="252" y="45"  font-style="italic">a = 2<tspan baseline-shift="super" font-size="9">k</tspan>-th ancestor of i</text>
  </g>
</svg>
</div>

When combining the two halves, we must also make sure that we follow the rules of house robber. $x$ and $m$
are adjacent, so at most one of them is looted: either bar $x$ from being looted, or bar $m$. Barring both
is never better than barring one, so those two cases are all we need.

$$
\begin{aligned}
\mathrm{dp}[i][k][a][b] = \max\big(\;
& \mathrm{dp}[i][k-1][a][1] + \mathrm{dp}[m][k-1][0][b], \\
& \mathrm{dp}[i][k-1][a][0] + \mathrm{dp}[m][k-1][1][b] \;\big)
\end{aligned}
$$

The first line bars $x$, the second bars $m$. The outer flags $a$ and $b$ pass through untouched, since the
outer endpoints of the halves are the outer endpoints of the whole.

### Answering queries

A route is not a single upward chain, so it cannot be covered by lifts directly. Let
$l = \mathrm{lca}(a, b)$. The route splits at $l$ into two arms, $a \to l$ and $b \to l$, and each
of those *is* an upward chain, of lengths $d_a = \mathrm{depth}(a) - \mathrm{depth}(l)$ and
$d_b = \mathrm{depth}(b) - \mathrm{depth}(l)$.

An upward chain of any length is covered by writing its length in binary and lifting once per set bit
merging as we go. Each lift lands exactly where the next one starts, so the segments are
consecutive and disjoint, and since merging is associative the result is the same as if we had one
precomputed segment of the full length.

<style>
@keyframes conquest-step-one   { 0%, 7%  { opacity: 0 } 10%, 100% { opacity: 1 } }
@keyframes conquest-step-two   { 0%, 37% { opacity: 0 } 40%, 100% { opacity: 1 } }
@keyframes conquest-step-three { 0%, 67% { opacity: 0 } 70%, 100% { opacity: 1 } }
@keyframes conquest-label-one   { 0%, 7%  { opacity: 0 } 10%, 36% { opacity: 1 } 39%, 100% { opacity: 0 } }
@keyframes conquest-label-two   { 0%, 37% { opacity: 0 } 40%, 66% { opacity: 1 } 69%, 100% { opacity: 0 } }
@keyframes conquest-label-three { 0%, 67% { opacity: 0 } 70%, 100% { opacity: 1 } }
.conquest-lift g[class^="conquest-"] { animation-duration: 7.5s; animation-iteration-count: infinite; }
.conquest-lift .conquest-one   { animation-name: conquest-step-one }
.conquest-lift .conquest-two   { animation-name: conquest-step-two }
.conquest-lift .conquest-three { animation-name: conquest-step-three }
.conquest-lift .conquest-said-one   { animation-name: conquest-label-one }
.conquest-lift .conquest-said-two   { animation-name: conquest-label-two }
.conquest-lift .conquest-said-three { animation-name: conquest-label-three }
@media (prefers-reduced-motion: reduce) {
  .conquest-lift g[class^="conquest-"] { animation: none; opacity: 1 }
  .conquest-lift .conquest-said-one, .conquest-lift .conquest-said-two { display: none }
}
</style>

<div style="display:flex;justify-content:center;margin:1.5rem 0;">
<svg class="conquest-lift" viewBox="0 0 440 190" width="100%" style="max-width:440px;height:auto;" role="img"
     aria-label="An arm of thirteen cities being covered by three lifts, of sizes one, four and eight, applied in that order.">
  <g stroke="currentColor" fill="none" stroke-width="1.5">
    <line x1="29" y1="40" x2="401" y2="40"/>
    <circle cx="20"  cy="40" r="9" fill="currentColor" fill-opacity="0.15"/>
    <circle cx="50"  cy="40" r="9"/><circle cx="80"  cy="40" r="9"/>
    <circle cx="110" cy="40" r="9"/><circle cx="140" cy="40" r="9"/>
    <circle cx="170" cy="40" r="9"/><circle cx="200" cy="40" r="9"/>
    <circle cx="230" cy="40" r="9"/><circle cx="260" cy="40" r="9"/>
    <circle cx="290" cy="40" r="9"/><circle cx="320" cy="40" r="9"/>
    <circle cx="350" cy="40" r="9"/><circle cx="380" cy="40" r="9"/>
  </g>
  <g fill="currentColor" stroke="none" font-size="12">
    <text x="20"  y="22" text-anchor="middle" font-style="italic">a</text>
    <text x="380" y="22" text-anchor="middle" font-style="italic">l</text>
    <text x="220" y="176" text-anchor="middle" opacity="0.75">13 = 1101 in binary, so three lifts cover the arm</text>
  </g>
  <g class="conquest-one" stroke="currentColor" fill="currentColor" font-size="12">
    <path d="M11 62 V72 H29 V62" fill="none" stroke-width="1.5"/>
    <text x="20" y="90" text-anchor="middle" stroke="none">1</text>
  </g>
  <g class="conquest-two" stroke="currentColor" fill="currentColor" font-size="12">
    <path d="M41 62 V72 H149 V62" fill="none" stroke-width="1.5"/>
    <text x="95" y="90" text-anchor="middle" stroke="none">4</text>
    <line x1="35" y1="26" x2="35" y2="54" stroke-dasharray="4 3" opacity="0.6"/>
  </g>
  <g class="conquest-three" stroke="currentColor" fill="currentColor" font-size="12">
    <path d="M161 62 V72 H389 V62" fill="none" stroke-width="1.5"/>
    <text x="275" y="90" text-anchor="middle" stroke="none">8</text>
    <line x1="155" y1="26" x2="155" y2="54" stroke-dasharray="4 3" opacity="0.6"/>
  </g>
  <g fill="currentColor" stroke="none" font-size="13">
    <g class="conquest-said-one"><text x="220" y="125" text-anchor="middle">carrying a segment of 1 city</text></g>
    <g class="conquest-said-two"><text x="220" y="125" text-anchor="middle">glued at the first seam: 5 cities</text></g>
    <g class="conquest-said-three"><text x="220" y="125" text-anchor="middle">glued at the second seam: all 13 cities</text></g>
  </g>
</svg>
</div>

Note: $l$ lies on both arms, but must be counted once. We give it to the $a$ side: that arm is
lifted $d_a + 1$ steps, up to and including $l$, while the $b$ side is lifted only $d_b$ steps and
stops at the child of $l$.

Reading the route from $a$, we walk up to $l$ and then back *down* to $b$, so
the second arm appears reversed relative to how it was computed. Its two endpoint flags therefore
sit on the wrong ends, and swapping $[0][1]$ with $[1][0]$ puts them back. After the swap the two
pieces are adjacent at the seam between $l$ and its child on the $b$ side, so they merge exactly
like two halves of a lift.


If $d_b = 0$ then $b$ is an ancestor of $a$, or $b = a$, and the single $a$-side lift already covers
the whole route; there is nothing to merge.

The answer is the entry with neither outer endpoint barred, since the two ends of the route are
under no constraint from outside it.

Each query costs $O(\log N)$ for the LCA and $O(\log N)$ merges of four entries each.

## Implementation

```cpp title="conquest.cpp"
#include <bits/stdc++.h>
#define int long long
using namespace std;

struct Node {
    array<int,4> values{};
};

Node leaf(int gold) {
    Node node;
    node.values[0*2+0] = gold;
    return node;
}

Node merge(const Node &lower, const Node &upper) {
    Node result;
    for (int first = 0; first < 2; first++) {
        for (int second = 0; second < 2; second++) {
            result.values[first*2+second] = max(lower.values[first*2+1] + upper.values[0*2+second],
                                                lower.values[first*2+0] + upper.values[1*2+second]);
        }
    }

    return result;
}

Node reversed(Node node) {
    swap(node.values[0*2+1], node.values[1*2+0]);
    return node;
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int N, Q;
    cin >> N >> Q;

    vector<int> gold(N+1, 0);
    for (int i = 1; i <= N; i++) cin >> gold[i];

    vector<vector<int>> adj(N+1);
    for (int i = 0; i < N-1; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    vector<int> parent(N+1, 0), depth(N+1, 0);
    vector<char> seen(N+1, 0);
    vector<int> pending{1};
    seen[1] = 1;
    while (!pending.empty()) {
        int node = pending.back(); pending.pop_back();
        for (int next : adj[node]) {
            if (seen[next]) continue;
            seen[next] = 1;
            parent[next] = node;
            depth[next] = depth[node] + 1;
            pending.push_back(next);
        }
    }

    int LOG = 1;
    while ((1LL << LOG) <= N) LOG++;

    vector<vector<int>> ancestor(LOG, vector<int>(N+1, 0));
    vector<vector<Node>> table(LOG, vector<Node>(N+1));

    for (int node = 1; node <= N; node++) {
        ancestor[0][node] = parent[node];
        table[0][node] = leaf(gold[node]);
    }
    for (int level = 1; level < LOG; level++)
        for (int node = 1; node <= N; node++) {
            int middle = ancestor[level-1][node];
            ancestor[level][node] = ancestor[level-1][middle];
            table[level][node] = merge(table[level-1][node], table[level-1][middle]);
        }

    auto lca = [&](int u, int v) {
        if (depth[u] < depth[v]) swap(u, v);

        int difference = depth[u] - depth[v];
        for (int level = 0; level < LOG; level++) {
            if (difference >> level & 1) u = ancestor[level][u];
        }

        if (u == v) return u;
        for (int level = LOG-1; level >= 0; level--) {
            if (ancestor[level][u] != ancestor[level][v]) { u = ancestor[level][u]; v = ancestor[level][v]; }
        }

        return ancestor[0][u];
    };

    auto lift = [&](int node, int steps) {
        Node combined;
        bool started = false;
        for (int level = 0; level < LOG; level++)
            if (steps >> level & 1) {
                combined = started ? merge(combined, table[level][node]) : table[level][node];
                started = true;
                node = ancestor[level][node];
            }
        return combined;
    };

    while (Q--) {
        int u, v;
        cin >> u >> v;

        int meet = lca(u, v);
        int upperSteps = depth[u] - depth[meet];
        int lowerSteps = depth[v] - depth[meet];

        Node first = lift(u, upperSteps + 1);
        if (lowerSteps == 0) {
            cout << first.values[0*2+0] << "\n";
            continue;
        }

        Node second = lift(v, lowerSteps);
        cout << merge(first, reversed(second)).values[0*2+0] << "\n";
    }
}
```


Time complexity: $O\left((N + Q) \log N\right)$. Memory: $O(N \log N)$.

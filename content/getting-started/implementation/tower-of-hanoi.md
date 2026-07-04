---
draft: false
title: 'Tower of Hanoi'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Tower of Hanoi"
weight: 1
---

{{< problem "cses-tower-of-hanoi" >}}

## Hint

Think about a recurrence relation that relates the solution of $n$ and $n-1$. Then you can write the code as either recursion(easy) or as a loop(hard).

## Solution

The recurrence relation is as follows: to move a stack of n disks from a starting pillar to the ending pillar, you must first move n-1 disks from the starting pillar to the middle pillar, then move the $n$th disk from start to end, and then finally move the n-1 disks from the middle pillar to the end pillar. 

Here's the simple recursion code which solves the question.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);

  int n;
  cin >> n;

  vector<pair<int, int>> moves;

  // This can also be a normal function instead of a lambda expression.
  // Make sure that the moves vector is a global for this to work.
  function<void (int, int, int, int)> solve = [&] (int n, int start, int end, int middle){
    if (n == 0) return;
    solve(n - 1, start, middle, end);
    moves.push_back({start, end});
    solve(n - 1, middle, end, start);
  };

  //output:
  solve(n, 1, 3, 2);
  cout << moves.size() << endl;
  for(int i = 0; i < moves.size(); i++)
    cout << moves[i].first << " " << moves[i].second << endl;
  return 0;
}
```

As an extra challenge to the reader, try writing a solution with a loop instead of using recursion.

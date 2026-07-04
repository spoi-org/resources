---
draft: false
title: 'Tasks and Deadlines'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Tasks and Deadlines"
weight: 1
---

{{< problem "cses-tasks-and-deadlines" >}}

## Solution

The greedy approach is to always prioritize tasks with the shortest durations first, because choosing a long task early delays all subsequent tasks and reduces their rewards by a larger amount. Say we have 2 tasks $X$ and $Y$ arranged in the following 2 arrangements:

<!-- Diagram: Two timeline comparisons. Top row: task X (duration $a$) followed by task Y (duration $b$). Bottom row: task Y first, then task X. Each task is shown as a labelled rectangle whose width represents its duration. -->

**Ordering 1:** `[X (duration a)] [Y (duration b)]`

**Ordering 2:** `[Y (duration b)] [X (duration a)]`

If you compare the change in score from the first ordering to the second ordering: in the second ordering, $Y$ gets completed $a$ units of time earlier so the score of $Y$ increases by $a$. However, $X$ gets completed $b$ units of time later, so the score of $X$ decreases by $b$. The net change in score is $a - b < 0$ which is lesser than it was in the first ordering. Hence you must sort the tasks by during to ensure the maximum score.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main() {
    int n;
    cin >> n;

    // jobs[i] = {duration, deadline}
    vector<pair<int, int>> jobs(n);
    for (int i = 0; i < n; i++) {
        cin >> jobs[i].first >> jobs[i].second;
    }

    // Sort by duration (or by first element), ensures earliest finishing attempts first
    sort(jobs.begin(), jobs.end());

    ll time_elapsed = 0;   // running sum of durations
    ll total_reward = 0;   // accumulated reward

    for (int i = 0; i < n; i++) {
        time_elapsed += jobs[i].first;              // finish this job at this time
        total_reward += jobs[i].second - time_elapsed;  // reward = deadline - completion time
    }

    cout << total_reward << "\n";
    return 0;
}
```

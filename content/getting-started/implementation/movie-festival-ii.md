---
draft: false
title: 'Movie Festival II'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Movie Festival II"
weight: 1
---

{{< problem "cses-movie-festival-ii" >}}

## Solution

The movies are sorted by ending time so earlier-finishing movies are considered first. A `multiset` stores when each of the k watchers becomes free. For each movie, we find the watcher who become free as close as possible to when the current movie starts (i.e. the time when the watcher becomes free is the largest value less than or equal to the start time of the movie). If such a watcher exists, we assign the movie and update when the time when they become free. This greedy process maximizes the total number of movies watched.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, k;
    cin >> n >> k;

    // movies[i] = {end_time, start_time}
    vector<pair<int, int>> movies(n);
    for (int i = 0; i < n; i++) {
        cin >> movies[i].second >> movies[i].first;
    }

    // Sort movies by ending time (classic interval scheduling)
    sort(movies.begin(), movies.end());

    // Each element represents when a watcher becomes free
    multiset<int> freeAt;
    for (int i = 0; i < k; i++) {
        freeAt.insert(0);
    }

    int watched = 0;

    for (auto [endTime, startTime] : movies) {
        // Find the watcher who is free as close to the startTime (i.e largest values less than or equal to startTime)
        auto it = freeAt.upper_bound(startTime);//auto is multiset<int>::iterator

        if (it == freeAt.begin()) {
            // No watcher available
            continue;
        }

        // Assign this movie to the latest possible free watcher
        freeAt.erase(--it);//upperbound - 1 is the same as largest value less than or equal to startTime
        freeAt.insert(endTime);//updating when the watcher gets free
        watched++;
    }

    cout << watched << "\n";
    return 0;
}
```

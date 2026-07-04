---
draft: false
title: 'Knight Moves Grid'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Knight Moves Grid"
weight: 1
---

{{< problem "cses-knight-moves-grid" >}}

## Solution

The program calculates the minimum number of knight moves needed to reach every square on an `n x n` chessboard starting from the top-left corner.

It keeps a grid where each cell stores how many moves are required to reach it, marking unreachable cells as -1.
The knight starts at position (0, 0) with zero moves taken.
From each position, all eight legal knight moves are tried.
Whenever a new square is reached for the first time, its move count is recorded as one more than the current square.
Each newly reached position is added so its moves can be explored later.

This process continues until all reachable squares have been processed.
Finally, the grid of minimum move counts is printed.

A visual understanding of the algorithm can be found in the image below:

<!-- Diagram: An 8×8 chessboard with a knight on a8. Orange arrows show the knight's first BFS wave to reachable squares (b6, c7, etc.), with move counts displayed on each square, illustrating how the queue expands one knight-move at a time. -->

You move from the knight to all unvisited grid that are a knight move away. This approach guarantees the shortest path to any cell.

The code does use a data structure called a queue, which you may be unfamiliar with. See  for what a queue is.

### Code:

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int n;
    cin >> n;

    // dist[x][y] will store the minimum number of moves needed to reach cell (x, y)
    // a value of -1 means that cell has not been reached yet
    vector<vector<int>> dist(n, vector<int>(n, -1));

    // This queue stores board positions that still need to be explored
    queue<pair<int, int>> q;

    // These arrays describe how a knight moves on a chessboard
    // Each (dx[k], dy[k]) pair represents one possible knight move
    vector<int> dx = {-2, -1, 2, 1, 2, 1, -1, -2};
    vector<int> dy = {-1, -2, 1, 2, -1, -2, 2, 1};

    // This function checks whether a position is inside the board
    // and whether it has not been visited before
    auto isValid = [&](int x, int y) {
        return x >= 0 && y >= 0 && x < n && y < n && dist[x][y] == -1;
    };

    // We begin from the top-left cell (0, 0)
    // Reaching the starting cell takes 0 moves
    dist[0][0] = 0;
    q.push({0, 0});

    // As long as there are positions left to explore, keep processing them
    while (!q.empty()) {
        // Take the oldest unexplored position from the queue
        int x = q.front().first, y = q.front().second;
        q.pop();

        // Try moving the knight in all 8 possible ways from this position
        for (int k = 0; k < 8; k++) {
            int nx = x + dx[k];
            int ny = y + dy[k];

            // If the new position is valid and not visited yet
            if (isValid(nx, ny)) {
                // The distance to this cell is one more than the current cell
                dist[nx][ny] = dist[x][y] + 1;

                // Add the new position to the queue to explore later
                q.push({nx, ny});
            }
        }
    }

    // Print the minimum number of moves needed to reach each cell
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << dist[i][j] << " ";
        }
        cout << "\n";
    }

    return 0;
}
```

---
draft: false
title: 'Grid Path Description'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Grid Path Description"
weight: 1
---

{{< problem "cses-grid-path-description" >}}

## Hint

Try using backtracking but optimizing it for cases when you know it's impossible to reach the bottom-left while covering all other squares.

## Solution

You start at the top-left cell and must end at the bottom-left cell without visiting any cell twice. Each move must follow the given string, where `?` allows any direction and other characters force a specific move. The code tries all allowed moves step by step while marking visited cells and backtracking when either all cells are reached or it's impossible to continue the path.

While you could implement this easily, this will be too slow. Hence, you need to optimize it to catch impossible routes as soon as possible.

+ If a path reaches the bottom-left cell early (i.e. before covering other cells), backtrack right here because all future paths can't end up at the bottom-left. 
+ If a path gets stopped by a wall and has the option to move left or right along the wall, then the grid will get split into 2 regions, and you can't cover all cells in both regions. Hence you must backtrack 
+ The extension to the previous point is that if a path get's stopped by cells previously visited and have to option to move left or right along the visited cells, the grid will again get split into 2 regions. Hence you must backtrack.

<!-- Diagram: A 7×7 grid with a path drawn from the top-left cell. The path hits a wall and splits the remaining unvisited cells into two shaded regions (Region 1 in blue, Region 2 in red). Caption: "2 regions formed by getting stopped at a wall". -->

<!-- Diagram: A 7×7 grid with a longer path that loops along previously visited cells, again splitting the grid into two disconnected shaded regions. Caption: "2 regions formed by getting stopped by previously visited cells". -->

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

string path;
bool visited[7][7];          // Marks cells already used in the current path
int ans = 0;

const int dx[] = {1, -1, 0, 0};   // Change in row for each move
const int dy[] = {0, 0, 1, -1};   // Change in column for each move
const char dir[] = {'D', 'U', 'R', 'L'};  // Corresponding move letters

bool inside(int x, int y) {
    return x >= 0 && x < 7 && y >= 0 && y < 7;   // Checks grid boundaries
}

bool is_blocked(int x, int y) {
    if (!inside(x, y) || visited[x][y]) return true; // Outside grid or already used
    return false;
}

void gridPaths(int x, int y, int step) {
    // If we reached the target cell
    if (x == 6 && y == 0) {
        if (step == 48) ans++;  // Count only if all moves were used
        return;
    }
    
    //Backtrack if the current path has split the grid into 2 regions.
    //x+1 is right, x-1 is left, y+1 is up, y-1 is down.
    if ((is_blocked(x + 1, y) && is_blocked(x - 1, y) && 
         !is_blocked(x, y + 1) && !is_blocked(x, y - 1)) ||
        (!is_blocked(x + 1, y) && !is_blocked(x - 1, y) &&
         is_blocked(x, y + 1) && is_blocked(x, y - 1)))
        return;

    visited[x][y] = true;// Mark this cell as visited

    for (int d = 0; d < 4; ++d) {
        int nx = x + dx[d];
        int ny = y + dy[d];

        // Enforce the given path character if it is not '?'
        if (path[step] != '?' && path[step] != dir[d]) continue;

        // Move only to valid and unused cells
        if (!is_blocked(nx,ny))
            gridPaths(nx, ny, step + 1);
    }

    visited[x][y] = false;  // Undo the move before trying other possibilities
}

int main() {
    cin >> path;
    gridPaths(0, 0, 0);           // Start from top-left with zero moves taken
    cout << ans << endl;
    return 0;
}
```


---
draft: false
title: 'Chessboard and Queens'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Chessboard and Queens"
weight: 1
---

{{< problem "cses-chessboard-and-queens" >}}

## Hint

Please see  for an explain to a question very similar to this one. You should then be able to solve this question easily.

## Solution

This solution uses backtracking. Section  explains a problem very similar to this which was how do you place $n$ queens on an $n \\times n$ chess board such that no 2 queens attack each other. This question on the other hand has $n = 8$ but has an additional condition that any cell with `*` is blocked.

The code is almost the same with just one extra condition that if a cell is `*`, you can't place a queen.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int n = 8, ans = 0;
vector<bool> col, diag1, diag2;
vector<vector<bool>> blocked;

void findPositions(int i = 0){
	if(i == n){//If true, we successfully placed all the queens in an arrangement.
		ans++;
		return;
	}

	for(int j = 0; j < n; j++){
		if(blocked[i][j] || col[j] || diag1[i+j] || diag2[(n-1)-j+i]) //The new queen would be blocked or attacked
      continue;
		col[j] = diag1[i+j] = diag2[(n-1)-j+i] = true;//Placing the queen on the current spot
		findPositions(i+1);
		col[j] = diag1[i+j] = diag2[(n-1)-j+i] = false;//Removing queen for the current spot
	}
}

int main(){
  //n was defined globally as 8
  
  for(int i = 0; i < n; i++){
    for(int j = 0; j < n; j++){
      char ch;
      cin >> ch;
      blocked[i][j] = (ch == '*');
    }
  }
      
  col.resize(n);
  diag1.resize(2*n-1);
  diag2.resize(2*n-1);
  findPositions();

	cout << ans << endl;
	return 0;
}
```

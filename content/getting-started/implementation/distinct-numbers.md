---
draft: false
title: 'Distinct Numbers'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Distinct Numbers"
weight: 1
---

{{< problem "cses-distinct-numbers" >}}

## Solution

Accept all the numbers and insert them into a set. Then report the size of the set. This works due to the fact that a set only stores unique elements and removes duplicates.

### Code:

```cpp

#include <bits/stdc++.h>
using namespace std;

int main(){

	int n;
	cin >> n;

	set<int> s;

	for(int i = 0; i < n; i++){
		int x;
		cin >> x;
		s.insert(x);// Accepting and inserting the values into the set.
	}

	cout << s.size() << endl;// Outputs the number of unique elements.

	return 0;
}
```

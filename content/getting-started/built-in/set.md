---
draft: false
title: 'Set'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Distinct Numbers"
weight: 2
---

In brief, this data structure allows for $\mathcal{O}(\log{n})$ insertions, deletions, and membership checks. Here's how it's used:

```cpp
int main() {
  set<int> st;
  st.insert(2);

  cout << (st.contains(2) ? "true" : "false") << '\n'; // note that .contains() needs c++20, you can enable this by appending -std=c++20 to your compiler's command line parameters

  st.erase(2);

  st.insert(5);
  auto it = st.find(5);
  if (it != st.end()) { // this method, on the other hand, does not need c++20
    cout << "5 exists!\n");
  }

  cout << st.size() << '\n'; // 1

  st.insert(7);
  st.insert(6);

  st.insert(6); // this won't do anything
                // since a set will not add duplicates

  cout << st.size() << '\n'; // will print 3, not 4

  cout << *st.begin() << '\n'; // the smallest element: 5
  cout << *st.rbegin() << '\n'; // the largest element: 7

  for (int i : st) { // 5 6 7; an in order traversal gives the elements in sorted order
    cout << i << ' ';
  }
  cout << '\n';
}
```

In fact, equipped with this knowledge, we can solve the first problem in CSES's sorting and searching category:

{{< problem "cses-distinct-numbers" >}}

## Abridged problem statement

Given a list of $n$ numbers, find how many distinct values occur in the list.

## Solution

Notice that what this problem asks is exactly what `std::set` does— a perfect opportunity to use it!

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  int n;
  cin >> n;
  set<int> st;
  for (int i = 0, x; i < n; ++i) {
    cin >> x;
    st.insert(x);
  }
  cout << st.size() << '\n';
}
```
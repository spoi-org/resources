---
date: '2026-05-25T21:48:47Z'
draft: false
title: 'Using STL'
weight: 2
---

For elementary forms of binary search, like the one presented above, there's no need for us to manually code up a binary search routine since we can just use the C++ STL instead. 

- `std::lower_bound(iterator l, iterator r, T x)` - finds the first iterator in $[l, r)$ satisfying $*it \ge x$, where $*$ signifies dereferencing.
- `std::upper_bound(iterator l, iterator r, T x)` - finds the first iterator in $[l, r)$ satisfying $*it > x$, where $*$ signifies dereferencing.

`T` is a typename. In both cases, if no such iterator exists, $r$ is returned.

Here's how you'd use `std::lower_bound()` to solve the above problem:
```cpp
auto it = std::lower_bound(A.begin(), A.end(), x);
std::cout << x << " is located at index " << (it - A.begin()) << '\n';
```

These functions also exist for sets (and multisets), and can be used like this:
```cpp
std::set<int> st;
st.insert(1);
st.insert(2);
st.insert(4);
auto it1 = st.lower_bound(3);
auto it2 = st.upper_bound(1);
```

<ptable>
<prow>lc-704</prow>
<prow>lc-34</prow>
<prow>lc-278</prow>
<prow>lc-374</prow>
</ptable>

Note: use STL when you can!

Here's another problem:
> Given a sorted array $A$, answer $Q$ queries where you need to find the number of elements that lie between $l$ and $r$ ($l \le r$)\, inclusive.

And here's the solution! Note the use of `lower_bound` and `upper_bound`.
```cpp
while (Q--) {
  int l, r;
  std::cin >> l >> r;

  auto it1 = std::lower_bound(A.begin(), A.end(), l);
  auto it2 = std::upper_bound(A.begin(), A.end(), r);

  if (it1 == A.end()) {
    std::cout << "0\n";
  } else {
    std::cout << (it2 - it1) << '\n';
  }
}
```
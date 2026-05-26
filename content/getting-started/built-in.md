---
draft: false
title: 'Built-in'
weight: 2
---

(For the sake of brevity, this article leaves out details about how these data structures are implemented—for example, dynamic array doubling for `vector` or balanced binary search trees for `set`. Feel free to look these up yourself if you’re curious!)

As you may know already, most languages come with a standard library with helpful utilities. This includes C++. C++ without its standard library is actually a very minimal language. For example, we wouldn't have any way to take in input without `<iostream>`!

<details>
<summary>Is there really no way?</summary>

There is a way, but it requires invoking system calls manually, which is tedious and far beyond what you need for IOI. Low-level programming is outside the scope of the contest, so you **do not need to learn this** for anything practical.

</details>

Anyway, every C++ program you'll write will probably look somewhat like this:

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  
}
```

Let’s examine the line `#include <bits/stdc++.h>`. It uses an include directive. The header bits/stdc++.h itself consists of a list of all the standard C++ headers.

<details>
<summary>bits/stdc++.h</summary>

```cpp
// C++ includes used for precompiling -*- C++ -*-

// Copyright (C) 2003-2025 Free Software Foundation, Inc.
//
// This file is part of the GNU ISO C++ Library.  This library is free
// software; you can redistribute it and/or modify it under the
// terms of the GNU General Public License as published by the
// Free Software Foundation; either version 3, or (at your option)
// any later version.

// This library is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.

// Under Section 7 of GPL version 3, you are granted additional
// permissions described in the GCC Runtime Library Exception, version
// 3.1, as published by the Free Software Foundation.

// You should have received a copy of the GNU General Public License and
// a copy of the GCC Runtime Library Exception along with this program;
// see the files COPYING3 and COPYING.RUNTIME respectively.  If not, see
// <http://www.gnu.org/licenses/>.

/** @file stdc++.h
 *  This is an implementation file for a precompiled header.
 */

// 17.4.1.2 Headers

// C
#ifndef _GLIBCXX_NO_ASSERT
#include <cassert>
#endif
#include <cctype>
#include <cfloat>
#include <climits>
#include <csetjmp>
#include <cstdarg>
#include <cstddef>
#include <cstdlib>

#if __cplusplus >= 201103L
#include <cstdint>
#if __cplusplus < 201703L
#include <ciso646>
#endif
#endif

// C++
// #include <bitset>
// #include <complex>
#include <algorithm>
#include <bitset>
#include <functional>
#include <iterator>
#include <limits>
#include <memory>
#include <new>
#include <numeric>
#include <typeinfo>
#include <utility>

#if __cplusplus >= 201103L
#include <array>
#include <atomic>
#include <initializer_list>
#include <ratio>
#include <scoped_allocator>
#include <tuple>
#include <typeindex>
#include <type_traits>
#endif

#if __cplusplus >= 201402L
#endif

#if __cplusplus >= 201703L
#include <any>
// #include <execution>
#include <optional>
#include <variant>
#include <string_view>
#endif

#if __cplusplus >= 202002L
#include <bit>
#include <compare>
#include <concepts>
#include <numbers>
#include <ranges>
#include <span>
#include <source_location>
#include <version>
#if __cpp_impl_coroutine
# include <coroutine>
#endif
#endif

#if __cplusplus > 202002L
#include <expected>
#include <stdatomic.h>
#endif

#if _GLIBCXX_HOSTED
// C
#ifndef _GLIBCXX_NO_ASSERT
#include <cassert>
#endif
#include <cctype>
#include <cerrno>
#include <cfloat>
#include <climits>
#include <clocale>
#include <cmath>
#include <csetjmp>
#include <csignal>
#include <cstdarg>
#include <cstddef>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
#include <cwchar>
#include <cwctype>

#if __cplusplus >= 201103L
#include <cfenv>
#include <cinttypes>
#include <cstdint>
#include <cuchar>
#if __cplusplus < 201703L
#include <ccomplex>
#include <cstdalign>
#include <cstdbool>
#include <ctgmath>
#endif
#endif

// C++
#include <complex>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <set>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <typeinfo>
#include <utility>
#include <valarray>
#include <vector>

#if __cplusplus >= 201103L
#include <array>
#include <atomic>
#include <chrono>
#include <codecvt>
#include <condition_variable>
#include <forward_list>
#include <future>
#include <initializer_list>
#include <mutex>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <system_error>
#include <thread>
#include <tuple>
#include <typeindex>
#include <type_traits>
#include <unordered_map>
#include <unordered_set>
#endif

#if __cplusplus >= 201402L
#include <shared_mutex>
#endif

#if __cplusplus >= 201703L
#include <any>
#include <charconv>
// #include <execution>
#include <filesystem>
#include <optional>
#include <memory_resource>
#include <variant>
#endif

#if __cplusplus >= 202002L
#include <barrier>
#include <bit>
#include <compare>
#include <concepts>
#include <format>
#include <latch>
#include <numbers>
#include <ranges>
#include <span>
#include <stop_token>
#include <semaphore>
#include <source_location>
#include <syncstream>
#include <version>
#endif

#if __cplusplus > 202002L
#include <expected>
#include <flat_map>
#include <flat_set>
#include <generator>
#include <print>
#include <spanstream>
#include <stacktrace>
#include <stdatomic.h>
#include <stdfloat>
#endif

#if __cplusplus > 202302L
#include <text_encoding>
#include <stdbit.h>
#include <stdckdint.h>
#endif

#endif // HOSTED
```
</details>

The C++ standard library is large, but we only need a small portion of it. In this article, we will focus on the data structures it provides.

Let's begin with something everyone has used at least once: `std::vector`.

# `std::vector`

## Introduction

```cpp
int main() {
  vector<int> a;
  a.push_back(2);
  cout << a.back() << '\n'; // 2
  a.push_back(3);
  cout << a[0] + a[1] << '\n'; // 5

  cout << a.size() << '\n'; // 2
  
  swap(a[0], a[1]);
  a.pop_back();
  cout << a.back() << '\n'; // 3
}
```

The above code demonstrates some basic functionalities of a vector. As is evident, we insert elements to the back of a `vector` with the function `.push_back()`. Also, we remove elements with `.pop_back()`.

Vectors are useful when you need to store $n$ numbers but do not know $n$ until the program is running. For example, here is a program that reads a number $n$, then reads $n$ integers and prints their sum.

```cpp
int main() {
  int n;
  cin >> n;
  vector<int> a(n);
  for (int &i : a) {
    cin >> i;
  }

  int sum = 0;
  for (int &i : a) {
    sum += i;
  }
  cout << sum << '\n';
}
```

By now you have probably noticed the pattern for declaring a vector: a `<`, then a type such as `int`, and then a `>`. This is correct, and you can use a vector with any type. For example, here is a vector of `pair<int, int>`.

```cpp
int main() {
  int n;
  cin >> n;
  vector<pair<int, int>> a(n);
  for (auto &[a, b] : a) {
    cin >> a >> b;
  }
}
```

In fact, we can go one level deeper and make the type of a vector... a vector itself!

```cpp
int main() {
  int n, m;
  cin >> n >> m;
  vector<vector<int>> grid(n, vector<int>(m));
  for (auto &i : grid) {
    for (int &j : i) {
      cin >> j;
    }
  }
}
```

## Methods

Okay, now that we've seen a bunch of examples, here's a list of all the most important methods you'll need for competitive programming:

### CTORs
- `vector<T>(std::size_t n)` creates a vector of size `n` with all elements initialised to `T{}`. For `int`, `int{}` is `0`, for `pair<A, B>`, it is `{A{}, B{}}`, and for your own custom type, it is whatever the default constructor provides.
- `vector<T>(std::size_t n, T x)` creates a vector of size `n` with all elements initialised to `x`. This is what we use in the above code snippet.

### General purpose
- `T &operator[](int i)`, used as `a[i]`, returns the $i$-th element of the vector `a`. For example, `a[0]` returns the $0$-th element.
- `void push_back(T x)` appends `x` to the end of the vector.
- `void pop_back()` removes the last element of the vector.
- `std::size_t size()` returns the size of the vector.
- `void assign(std::size_t n, T x)` sets the first `n` elements in the vector to `x`.
- `void resize(std::size_t n)` changes the capacity of the vector to `n`. If `n` is less than the current capacity, it deletes the elements at the end, and if it's greater than the current capacity, it adds `T{}`s to the end.
- `void clear()` clears the vector, essentially setting its size to $0$ and therefore removing all elements.

**Note**: Throughout my code, you will see intermediate C++ features like structured bindings and range-based for loops. I believe in learning by example, so don’t worry if you don’t recognize every construct. You may have to rely on intuition and pick up some new concepts as you read.

There is also a container called `std::deque`, which allows insertions at both ends. There is also `std::stack`, which only allows access at the back, so it is strictly less powerful than a vector. I’m not covering `std::deque` because it is a bit advanced, but you can read about it on [cppreference](https://en.cppreference.com/w/cpp/container/deque.html). I’m also not covering `std::stack` because it is strictly worse (and slower, and uses more memory by default; ironic, given it can do less!) than `std::vector`.

With that being said, let's move on to the next major data structure; this time we'll look at something interesting!

# `std::set`

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

<problem>distinctnumbers</problem>

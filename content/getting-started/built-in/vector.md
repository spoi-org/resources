---
draft: false
title: 'Vector'
weight: 1
---

Let's begin with something everyone has used at least once: `std::vector`.

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
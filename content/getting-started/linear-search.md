---
date: '2026-05-25T21:47:38Z'
draft: true
title: 'Linear search'
weight: 3
---

Linear search is an extremely common technique in competitive programming, and you probably already know what it is, even if you think you don't.

Consider the following problem:
> Given an array $A$ of length $n$, find and print the index of the first element in $A$ that is equal to $x$. If $x$ is not present in the array, print $n$ instead.

## Solution 1

We can solve this with a simple for loop:
<details>

```cpp
int ans = n;
for (int i = 0; i < n; i++) {
  if (A[i] == x) {
    ans = i;
    break;
  }
}
std::cout << ans << '\n';
```
</details>

## Solution 2

We can leverage the [C++ STL](https://en.wikipedia.org/wiki/Standard_Template_Library) to solve this with way less code!

```cpp
int ans = std::find(A.begin(), A.end(), x) - A.begin();
std::cout << ans << '\n';
```

What if we wanted to find the first odd/even number?
> Find and print the index of the first even number in $A$. If an even number does not exist, print $n$ instead.

Again, we have two solutions:

## Solution 1
Again, we can just use a for loop:
<details>

```cpp
int ans = n;
for (int i = 0; i < n; i++) {
  if (A[i] % 2 == 0) {
    ans = i;
    break;
  }
}
std::cout << ans << '\n';
```
</details>

## Solution 2
Or we could use C++'s STL.
```cpp
auto is_even = [](int x) { return x % 2 == 0; };
int ans = std::find_if(A.begin(), A.end(), is_even) - A.begin();
std::cout << ans << '\n';
```

`is_even` is a [lambda function](https://en.cppreference.com/w/cpp/language/lambda). These are special inline functions that can be passed as parameters to various other functions, as done so above.

Let us now consider a slightly more interesting problem:
> Given an array $A$ of length $n$, answer $q$ queries where each query asks for the index of the first element $\le Q_i$. If no such element exists, print $n$.

## Solution
I hope that the non-STL solution is obvious. Here's the STL solution:
```cpp
vector<int> pref_min(n);

partial_sum(A.begin(), A.end(), pref_min.begin(), [](int a, int b) { return min(a, b); });

for_each(
    Q.begin(), Q.end(),
    [](int q) {
        cout << (find_if(pref_min.begin(), pref_min.end(), [q](int x) {
            return x <= q;
        }) - pref_min.begin() << '\n';
    }
);
```

The time complexity of the above code is $\mathcal{O}(nq)$.

## Explanation

Multiple STL functions were introduced in the above code examples. Here's a brief description of each one:

1. [`find()`](https://en.cppreference.com/w/cpp/algorithm/find) returns an iterator to the first element in a range equal to a specified value.
2. [`find_if()`](https://en.cppreference.com/w/cpp/algorithm/find) returns an iterator to the first element in a range for which the lambda function passed to it returns `true`.
3. [`partial_sum()`](https://en.cppreference.com/w/cpp/algorithm/partial_sum) essentially computes the prefix sum/max/min (or any binary operation) of a range. In the above example, the $i^{\text{th}}$ element of the output range (`pref_min`) will contain $\min\{A[0]...A[i]\}$.
4. [`for_each()`](https://en.cppreference.com/w/cpp/algorithm/for_each) runs the lambda function passed to it on every element in a range sequentially.
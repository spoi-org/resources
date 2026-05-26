---
draft: false
title: 'Lamps'
editorial:
  platform: "JOISC"
  name: "Lamps"
---

{{< problem "joisc-lamps" >}}

## Abridged problem statment

- You have two binary strings: $a$ and $b$.
- You can perform the following three operations:
  - Set a range $l$, $r$, with $1 \le l \le r \le n$ to $0$.
  - Set a range $l$, $r$, with $1 \le l \le r \le n$ to $1$.
  - Toggle a range $l$, $r$, with $1 \le l \le r \le n$: that is, perform addition modulo $2$.
- Find the minimum number of operations to convert $a$ to $b$.

How do we begin? Lucky for us, the problem gives us a good place to start.

## Subtask 1: $n \le 18$

$n \le 18$ should instantly remind you of one thing: bitmasks. And, sure enough, you can represent each state of string $a$ as a bitmask. We could try dynamic programming, but we'd quickly notice that there would be circular dependencies. But that's not an issue with something like a breadth-first-search. And actually, that's the solution.

Almost. Keep in mind that we can't implement our transitions in $\mathcal{O}(n^3)$ (naively). We need to be smart and use bitwise operations (which make our transitions $\mathcal{O}(n^2)$).

Let us iterate over each pair $(l, r)$ such that $1 \le l \le r \le n$. Let our starting state be $s$.

- Toggling seems to be the easiest, since it is equivalent to addition mod $2$, which is in turn equivalent to the exclusive or (XOR) bitwise operation. We can toggle the state of all bits from $l$ to $r$ by applying the XOR operation to $s$ with $2^{r+1} - 2^l$ (call the latter `mask`).
- For range setting to $0$, we extract the bits from $l$ to $r$ by doing `s & mask`. Then XORing this with $s$ again sets all of the bits in that region to $0$.
- The case for $1$ is similar, except we need to flip the bits we got with `s & mask`. We do this using the `~` operator, but then we need to re-AND it with `mask` to not flip the bits beyond our range. Then we simply XOR this with $s$.

In conclusion, the transitions are:

$$
s \to \{ \texttt{s \^{} mask, s \^{} (s \& mask), s \^{} ((\~{}(s \& mask)) \& mask)} \}
$$

## Subtask 3: Each character in $a$ is $0$

Imagine going from something like $0000000000$ to $0011001110$. There is one clear solution: simply flip all segments where there are $1$s in $b$ in $a$, and you're done. So that would be $00\underline{00}00\underline{000}0 \to 0011001110$. Is this optimal? Turns out, yes.

For the purposes of this short proof, I'll define 'fixing' a segment of $1$s as applying an operation on the corresponding $0$s in $a$ to convert them to a segment of $1$s. Also, let $k$ be the number of segments of $1$s in $b$.

We already know that our answer is at most $k$. But now imagine we tried to do this in less than $k$ operations: we'd have to fix two or more $1$ segments with a single operation. If you try to do this, you'd have to flip everything in between, including $0$s. But this introduces yet another $1$ segment that you'd have to fix, so even with just two $1$ segments, you'd be better off fixing them individually than trying to handle both in one operation.

## Full solution

Experimenting slightly with the operations might eventually lead you to noticing that two toggle operations that overlap can be replaced with two toggle operations that don't overlap.

<div style="margin: 2rem auto; display: flex; justify-content: center; align-items: center; gap: 4rem; font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace; font-size: 1.15rem; line-height: 1;">
  <span style="position: relative; display: inline-block; width: 14ch;">
    01010000100101
    <span style="position: absolute; left: 4ch; width: 6ch; top: 1.35em; border-top: 1px solid currentColor;"></span>
    <span style="position: absolute; left: 6ch; width: 5ch; top: 1.65em; border-top: 1px solid currentColor;"></span>
  </span>
  <span style="font-family: serif; font-size: 1.6rem; position: relative; top: -0.1em;">⇒</span>
  <span style="position: relative; display: inline-block; width: 14ch;">
    01010000100101
    <span style="position: absolute; left: 4ch; width: 2ch; top: 1.35em; border-top: 1px solid currentColor;"></span>
    <span style="position: absolute; left: 10ch; width: 1ch; top: 1.35em; border-top: 1px solid currentColor;"></span>
  </span>
</div>

Of course, similar logic can be used to reach the same conclusion for consecutive overlapping set operations. So if we have an optimal solution with consecutive overlapping and set operations, we can obtain one without. Next, notice that any set operations that come after toggles can be moved before them. Red lines depict set to $1$ operations, while blue lines are set to $0$ operations.

### Case #1: Intervals perfectly overlap

<div style="margin: 2rem auto; display: flex; justify-content: center; align-items: center; gap: 4rem; font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace; font-size: 1.15rem; line-height: 1;">
  <span style="position: relative; display: inline-block; width: 14ch;">
    01010000100101
    <span style="position: absolute; left: 0ch; width: 5ch; top: 1.35em; border-top: 1px solid currentColor;"></span>
    <span style="position: absolute; left: 2ch; width: 2ch; top: 1.65em; border-top: 1px solid red;"></span>
  </span>
  <span style="font-family: serif; font-size: 1.6rem; position: relative; top: -0.1em;">⇒</span>
  <span style="position: relative; display: inline-block; width: 14ch;">
    01010000100101
    <span style="position: absolute; left: 2ch; width: 2ch; top: 1.35em; border-top: 1px solid blue;"></span>
    <span style="position: absolute; left: 0ch; width: 5ch; top: 1.65em; border-top: 1px solid currentColor;"></span>
  </span>
</div>

### Case #2: Intervals don't perfectly overlap

With this, we've proven that there exists an optimal solution where all set operations occur before all toggle operations. How do we proceed?

Well, it's natural to divide the process into two steps:

* Go from $a$ to some ternary string consisting of (?, $0$, and $1$) using only set operations, where ? means no set operation has been applied to that character, $0$ means that the character has been forced to be a $0$, and $1$ means that it has been forced to be a $1$. This ternary string is just our way of representing what happens to the string after applying our set operations. Call this step $p$.
* Go from that same ternary string to $b$ using only toggle operations. Call this step $q$.

### Step $p$

Like before, we group the $1$s together. It is quite obvious that we can't perform operations that cross any ? characters, so we can deal with each part individually.

For one part, the optimal strategy is to either first zero out the entire range, then activate $1$s sequentially, or set the entire range to $1$ and then deactive ranges to $0$s sequentially.

<div style="margin: 2rem auto; display: flex; justify-content: center; align-items: center; font-family: 'Latin Modern Mono', 'CMU Typewriter Text', monospace; font-size: 1.15rem; font-weight: 500; line-height: 1;">
  <span style="position: relative; display: inline-block; width: 14ch;">
    a1 a2 a3 a4 a5
    <span style="position: absolute; left: 0ch; width: 14ch; top: 1.35em; border-top: 1px solid blue;"></span>
    <span style="position: absolute; left: 3ch; width: 2ch; top: 1.65em; border-top: 1px solid red;"></span>
    <span style="position: absolute; left: 9ch; width: 2ch; top: 1.65em; border-top: 1px solid red;"></span>
  </span>
</div>

The reason is: if we didn't do this, we'd need to deal with each $0$ and $1$ separately, spending one operation on each one of them. It is better to spend one operation in total for all $0$s or all $1$s at the beginning, and then spend the rest of your operations on the remaining ranges.

The answer for:

* 0 is 1
* 01 is 2
* 010 is 2
* 0101 is 3
* 01010 is 3
* 010101 is 4
* 0101010 is 4

Do you notice a pattern? Yes, in general, the answer is $\lfloor{\frac{k+2}{2}}\rfloor$, where $k$ is the number of segments!

### Step $q$

Form a new string, call it $s$, from the ternary string, replacing each ? at location $i$ with $a_i$. We need to go $s$ to $b$ using only toggles. Notice how this problem (as stated many times now!) is equivalent to converting $s$ to $b$ by performing range increment operations, modulo $2$.

So in other words, we want $s_i \equiv b_i \pmod{2} , \forall , i \in [1, n]$ in the fewest amount of increment operations. We can add $1$ to each side of this congruency without changing anything: we want $s_i + 1 \equiv b_i + 1 \pmod{2}$. The trick is: we'll perform this change for all indices where $s_i$ is $1$, thus making $s_i=0$.

This converts $s$ to a $0$ string, and the answer here (as we already determined in subtask $3$) is the number of $1$ segments in the modified $b$ (which is just $a \oplus b$, by the way).

### Putting it all together

So now we know how to find the answer for both steps $p$ and $q$. How do we use this to solve the problem?

Easy! Iterate over all $3^n$ possible ternary strings.

Okay, we obviously can't actually do that, but what we can do is use dynamic programming. We'll need to store the index we're at ($i$), the current character of the ternary string ($j$), and the number of segments (for step $p$), modulo $2$ ($k$). I'm sure you can figure the rest out yourself. If you can't, read ahead.

### Code

<details>
<summary>Expand</summary>

```cpp
#include <bits/stdc++.h>

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);

  int n;
  std::cin >> n;
  std::string a, b;
  std::cin >> a >> b;

  auto f = [&](int i, int j) {
    return i == n ? 0 : (j == 2 ? a[i] - '0' : j) ^ (b[i] - '0');
  };

  std::vector dp(n + 1, std::vector(3, std::vector<int>(2)));
  dp[n][0][0] = dp[n][1][1] = dp[n][0][1] = dp[n][1][0] = 1e8;
  for (int i = n - 1; i >= 0; --i) {
    for (int k = 0; k < 2; ++k) {
      dp[i][0][k] = std::min(
          {dp[i + 1][0][k] + (f(i, 0) and !f(i + 1, 0)),
           dp[i + 1][1][!k] + !k + (f(i, 0) and !f(i + 1, 1)),
           dp[i + 1][2][0] + !k + (f(i, 0)  and !f(i + 1, 2))});
      dp[i][1][k] = std::min(
          {dp[i + 1][0][!k] + !k + (f(i, 1) and !f(i + 1, 0)),
           dp[i + 1][1][k] + (f(i, 1) and !f(i + 1, 1)),
           dp[i + 1][2][0] + !k + (f(i, 1) and !f(i + 1, 2))});
      dp[i][2][k] = std::min(
          {dp[i + 1][0][1] + (f(i, 2)  and !f(i + 1, 0)) + 1,
           dp[i + 1][1][1] + (f(i, 2) and !f(i + 1, 1)) + 1,
           dp[i + 1][2][0] + (f(i, 2)  and !f(i + 1, 2))});
    }
  }

  std::cout << std::min({dp[0][0][1] + 1, dp[0][1][1] + 1, dp[0][2][0]}) << '\n';
}
```
</details>

This solution has a time complexity of $\mathcal{O}(n)$, and a space complexity of $\mathcal{O}(n)$.
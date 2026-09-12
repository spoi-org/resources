---
draft: false
title: 'Introduction'
weight: 1
---

Welcome to competitive programming!

For many of you, this marks your first step into the world of competitive programming. This resource covers some basic tips and principles to help you get started, depending on what you already know.

Most of what follows applies broadly, but some parts are written with the [ICO](https://www.iarcs.org.in/) series in mind: ZCO, INOI, and (optimistically) IOITC.

## Choosing a programming language

In its purest form, competitive programming has no language barrier. However, most contestants end up using one of the following three languages:

- Python
- Java
- C++

Most start out with Python, but as they get deeper into competitive programming, they usually switch to C++.

The reasons for this are as follows:
- Python is typically 2–3× slower than C++, and in certain cases, the gap can be much wider.
- At INOI (IOITC, and IOI), Python (and Java) are **not allowed**. The only allowed language is C++.
- C++ has more or less become the standard language for competitive programming, mostly because of the reasons mentioned earlier. Naturally, most resources you’ll find use C++.

This guide also uses C++. If you aren’t familiar with it yet, don’t worry—the basics will be covered soon. Still, it’s a good idea to start learning C++ early on.

## How do contests work

A contest is a set of competitive programming problems that you need to solve within a fixed time limit.

There are several types of contests.

The most common format is the IOI style, which is also used in ZCO and INOI. IOITC TSTs follow it exactly. In this format, you’re given three problems to solve in five hours. Each problem has multiple subtasks, and you earn partial points for each subtask you solve — up to 100 for a full solution.

ZCO is shorter, with two problems and three hours, while INOI has three problems but the same three-hour duration. In all of these contests, the time of submission doesn’t matter: a 30-point solution submitted five minutes after the contest starts is worth just as much as one submitted five minutes before it ends.

Another common format is the ICPC style, used in both Codeforces contests and ICPC itself. It differs quite a bit from the IOI format. ICPC-style contests usually have more problems — around 5–6 on Codeforces and 10–12 in ICPC — but run for a shorter duration: about 2–3 hours on Codeforces and 5 hours for ICPC. Unlike IOI contests, there are no subtasks, and ties are broken by time, so every minute counts.

Since these formats differ in rules and scoring, naturally, you should develop your strategy differently for each type.

## Time complexity

Time complexity is one of the most important ideas in competitive programming. Every problem you solve comes with a time limit, and your solution has to run within it.

At its core, time complexity describes how the number of operations in your program grows with the size of the input. The actual runtime then depends on more practical factors — the judge computer’s CPU speed, the specific instructions your code compiles to, cache locality, and even branch prediction.

In short, time complexity lets you estimate how long your program will take to run, and whether it can finish within the problem’s time limit — all just by analyzing your code.

### Formal definition

Let $T(n)$ denote the number of primitive computational steps an algorithm performs on an input of size $n$.

We usually express $T(n)$ asymptotically— much like an asymptote on a graph, it describes how the runtime function behaves as $n$ grows without bound— using Big O notation. We say that the time complexity of an algorithm is

$$T(n)=\mathcal{O}(f(n))$$

if there exist constants $c>0$ and $n_0 \ge 0$ such that for all $n \ge n_0$, 

$$T(n) \le c \cdot f(n)$$

Here, $c \cdot f(n)$ represents an upper bound on how the runtime grows relative to input size.

The graph below shows how some common time complexities grow as the input size $n$ increases. The curves are illustrative and are intended to show their relative growth rather than exact runtimes.

![Comparison of computational complexities](/images/time-complexity.svg)

*Image: Cmglee, CC BY-SA 4.0, via Wikimedia Commons.*

## Space complexity

Space Complexity

Space complexity is conceptually identical to time complexity, but it measures memory instead of time. Every problem you solve also comes with a memory limit— your variables, arrays, and data structures must all fit within it.

It’s often useful to estimate how much memory your program uses. For instance, an `int` typically takes $4$ bytes, so an array of $n$ integers would need about $4n$ bytes. These rough calculations help you gauge whether your program might exceed the limit.

That said, memory is rarely the main bottleneck in most Olympiad-style problems (of course, there are exceptions, such as [this JOI problem](https://oj.uz/problem/view/JOI18_snake_escaping)).

Just like with time, we express space complexity using Big O notation, which describes how your program’s memory usage grows with the input size.

## General constraints and tips

Every problem gives you limits on input size, for example $n \le 10^5$ or $n \le 10^9$. These **are not** random numbers; they hint at the expected time complexity of the solution.

A useful heuristic is that you can usually fit around $10^8$ simple operations into one second. (This varies between judges, and if your operations are heavy; e.g., lots of memory accesses or pointers moving; it's safer to assume closer to $10^7$.)

Here's a table listing some common constraints and their corresponding potential time complexities. Do not memorize this.

| Constraint | Feasible complexity                     |
| ---------- | --------------------------------------- |
| $n \le 10$ | $\mathcal{O}(n!)$ or better |
| $n \le 13$ | $\mathcal{O}(3^n)$ or better |
| $n \le 20$ | $\mathcal{O}(2^n)$ or better |
| $n \le 10^3$ | $\mathcal{O}(n^2)$ or better |
| $n \le 10^5$ | $\mathcal{O}(n \log{n})$ or better |
| $n \le 10^7$ | $\mathcal{O}(n)$ or better |
| $n \le 10^9$ | Usually needs $O(\log{n})$ or $O(1)$ |

### Be mindful of constants!

Even if two algorithms have the same Big O complexity, one can be several times faster because of smaller constant factors.

For example, `std::vector` is often faster than `std::deque`, even though both can theoretically handle similar input sizes within the same asymptotic limits.

Similarly, a Fenwick tree is typically faster than a recursive segment tree, even though both support $\mathcal{O}(\log n)$ updates and queries. *(If you don’t know what these are yet, don’t worry— they’ll be covered later!)*

Another common pitfall is `std::unordered_map` versus `std::map`: despite `unordered_map` having a theoretical complexity of $\mathcal{O}(1)$ compared to `map`’s $\mathcal{O}(\log n)$, `map` is *generally* faster in practice. This is due to better cache locality and lower constant factors. Moreover, `unordered_map` can degrade to $\mathcal{O}(n^2)$ on adversarial input because of hash collisions— for more on this, see [this well-known Codeforces blog](https://codeforces.com/blog/entry/62393).

### Hidden bottlenecks

- Using `cin`/`cout` without `ios::sync_with_stdio(false)`
- Using `endl` instead of `'\n'` (the former flushes the buffer, which is almost never what you need)
- Reallocating large containers repeatedly (very bad for cache locality)

### In contests

- Always test on edge cases: smallest and largest inputs, all identical values, empty structures, etc.
- If your program is slow, print timings for parts of your code using `chrono` or `clock()`.
- If you get RE (runtime error) or MLE (memory limit exceeded), check for array bounds and uninitialized memory.
- Don’t panic if your first idea seems inefficient: this is normal. Simplify the problem, find patterns, optimize step by step.
- Know when to move on. Spending 90 minutes on a 100-point problem you’re stuck on often costs more than solving an easier one for 50 points.
- For IOI style contests, **always read all problems first**, sometimes an “easy” one is hidden later.

## Past ZCO cutoffs; relevance to ICO

This section lists the past qualification cutoffs for the ZCO.

All information below is taken directly from verified participant communications and official announcements. Formatting has been standardized for clarity— no values have been altered.

If you’re new to ICO, you may want to start with [this excellent introduction by Samik Goyal](https://samikgoyal.com/posts/ico-starter/), which explains the structure of ZCO, INOI, and IOITC in detail.
The data here is also referenced from [his compiled dataset](https://samikgoyal.com/data/zco/).

---

### ZCO 2025

| Class       |     Male |  Female |
| :---------- | -------: | ------: |
| 12          | $28 / 200$ | $9 / 200$ |
| 11          | $25 / 200$ | $9 / 200$ |
| 10          | $25 / 200$ | $9 / 200$ |
| 9           | $16 / 200$ | $9 / 200$ |
| 8           | $12 / 200$ | $9 / 200$ |
| 7 and below | $10 / 200$ | $9 / 200$ |

---

### ZCO 2024

| Class       |     Male |   Female |
| :---------- | -------: | -------: |
| 12          | $55 / 200$ | $35 / 200$ |
| 11          | $51 / 200$ | $35 / 200$ |
| 10          | $50 / 200$ | $35 / 200$ |
| 9           | $46 / 200$ | $28 / 200$ |
| 8           | $41 / 200$ | $28 / 200$ |
| 7 and below | $41 / 200$ | $28 / 200$ |

---

### ZCO 2023

| Class       |     Male |   Female |
| :---------- | -------: | -------: |
| 12          | $27 / 200$ | $21 / 200$ |
| 11          | $22 / 200$ | $16 / 200$ |
| 10          | $20 / 200$ | $13 / 200$ |
| 9           | $17 / 200$ | $12 / 200$ |
| 8           | $16 / 200$ | $11 / 200$ |
| 7 and below | $16 / 200$ | $11 / 200$ |

As is clear, the cutoffs are absurdly low. In fact, none of them have ever gone above 100 points. This means you should focus almost entirely on subtasks when attempting ZCO. A foolproof strategy is to solve the easiest subtasks in both problems: that alone will usually get you past the cutoff.

## Conclusion

Competitive programming can seem overwhelming at first: new formats, strict time limits, and unfamiliar rules. But like anything else, it becomes easier the more you do it.

Don’t rush. Focus on correctness first, speed later. Learn to reason about problems instead of memorizing solutions. Over time, patterns start to repeat, and what once felt impossible becomes routine.

That’s all you really need to get started.

Welcome to competitive programming.

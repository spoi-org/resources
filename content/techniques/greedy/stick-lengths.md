---
draft: false
title: 'Stick lengths'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Stick Lengths"
weight: 2
---

{{< problem "cses-stick-lengths" >}}

## Abridged problem statement

You are given an array $a$ of length $n$. Choose an array $c$ such that $a[i]+c[i]=a[j]+c[j]$ for all $(i,j)$, and $\sum|c[i]|$ is minimized.

## Solution

Let's fix the final value each $a[i]$ ends up with: call this $x$. Then, our goal is to choose an $x$ that minimizes $\sum|a[i]-x|$.

Here's a plot of the value of $\sum|a[i]-x|$ against $x$ for $a=[0,1,4,5,7,10,12,13]$:

<div style="margin: 2rem auto; width: 34rem; position: relative;">
<svg width="510" height="265" xmlns="http://www.w3.org/2000/svg" style="font-family: inherit; overflow: visible;">
  <line x1="213" y1="223" x2="213" y2="240" stroke="currentColor" stroke-width="1" stroke-dasharray="3,3" opacity="0.45"/>
  <line x1="286" y1="223" x2="286" y2="240" stroke="currentColor" stroke-width="1" stroke-dasharray="3,3" opacity="0.45"/>
  <polyline points="67,102 176,205 213,223 286,223 395,171 468,102"
    fill="none" stroke="currentColor" stroke-width="3" stroke-linejoin="round"/>
  <line x1="30" y1="240" x2="510" y2="240" stroke="currentColor" stroke-width="1.5"/>
  <line x1="30" y1="15"  x2="30"  y2="240" stroke="currentColor" stroke-width="1.5"/>
  <line x1="25" y1="240" x2="35" y2="240" stroke="currentColor" stroke-width="1"/><text x="22" y="244" text-anchor="end" font-size="0.8rem" fill="currentColor">30</text>
  <line x1="25" y1="205" x2="35" y2="205" stroke="currentColor" stroke-width="1"/><text x="22" y="209" text-anchor="end" font-size="0.8rem" fill="currentColor">34</text>
  <line x1="25" y1="171" x2="35" y2="171" stroke="currentColor" stroke-width="1"/><text x="22" y="175" text-anchor="end" font-size="0.8rem" fill="currentColor">38</text>
  <line x1="25" y1="136" x2="35" y2="136" stroke="currentColor" stroke-width="1"/><text x="22" y="140" text-anchor="end" font-size="0.8rem" fill="currentColor">42</text>
  <line x1="25" y1="102" x2="35" y2="102" stroke="currentColor" stroke-width="1"/><text x="22" y="106" text-anchor="end" font-size="0.8rem" fill="currentColor">46</text>
  <line x1="25" y1="67"  x2="35" y2="67"  stroke="currentColor" stroke-width="1"/><text x="22" y="71"  text-anchor="end" font-size="0.8rem" fill="currentColor">50</text>
  <line x1="30"  y1="235" x2="30"  y2="245" stroke="currentColor" stroke-width="1"/><text x="30"  y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">0</text>
  <line x1="67"  y1="235" x2="67"  y2="245" stroke="currentColor" stroke-width="1"/><text x="67"  y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">1</text>
  <line x1="103" y1="235" x2="103" y2="245" stroke="currentColor" stroke-width="1"/><text x="103" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">2</text>
  <line x1="140" y1="235" x2="140" y2="245" stroke="currentColor" stroke-width="1"/><text x="140" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">3</text>
  <line x1="176" y1="235" x2="176" y2="245" stroke="currentColor" stroke-width="1"/><text x="176" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">4</text>
  <line x1="213" y1="235" x2="213" y2="245" stroke="currentColor" stroke-width="1"/><text x="213" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">5</text>
  <line x1="249" y1="235" x2="249" y2="245" stroke="currentColor" stroke-width="1"/><text x="249" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">6</text>
  <line x1="286" y1="235" x2="286" y2="245" stroke="currentColor" stroke-width="1"/><text x="286" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">7</text>
  <line x1="322" y1="235" x2="322" y2="245" stroke="currentColor" stroke-width="1"/><text x="322" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">8</text>
  <line x1="358" y1="235" x2="358" y2="245" stroke="currentColor" stroke-width="1"/><text x="358" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">9</text>
  <line x1="395" y1="235" x2="395" y2="245" stroke="currentColor" stroke-width="1"/><text x="395" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">10</text>
  <line x1="431" y1="235" x2="431" y2="245" stroke="currentColor" stroke-width="1"/><text x="431" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">11</text>
  <line x1="468" y1="235" x2="468" y2="245" stroke="currentColor" stroke-width="1"/><text x="468" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">12</text>
  <line x1="505" y1="235" x2="505" y2="245" stroke="currentColor" stroke-width="1"/><text x="505" y="258" text-anchor="middle" font-size="0.8rem" fill="currentColor">13</text>
</svg>
</div>

We can make some inferences from this graph. To begin with, it looks like the graph decreases first, then becomes flat, and then increases. In addition to this, the minimum value seems to be attained at all values from $x=5$ to $x=7$. 

Let us try generalising these observations. Define $f(x)=\sum|x-a[i]|$, and examine the value of $f(x+1)-f(x)$:

- Changing $x$ to $x+1$ brings us closer to all $a[i]>x$.
- It also brings us farther from all $a[i] \le x$.

That is,
$$
|(x+1)-a_i|-|x-a_i|=
\begin{cases}
+1 & a_i \le x \\
-1 & a_i > x
\end{cases}
$$

So, define $c(v)=|\{i; a[i] \le v\}|$, then $f(x+1)-f(x) = c(x) - [n-c(x)] = 2c(x) - n$. Therefore, $f(x+1)<f(x)$ when $c(x)<\frac{n}{2}$. Since $c(x)$ is an integer, this becomes $c(x)\le\lfloor\frac{n-1}{2}\rfloor$.

In other words, as long as there are less than $\lfloor\frac{n-1}{2}\rfloor$ elements to our left, $f(x+1)$ is better than $f(x)$. After that, $f(x+1)$ is worse (or just as good as) $f(x)$. So we pick $x$ such that there are exactly $\lfloor\frac{n-1}{2}\rfloor$ elements to the left of it in our array.

Such a value is called a median of the array. Hence, the minimum is attained when $x$ is a median of $a$. If $n$ is even, every value between the two medians is optimal.

## Code

```cpp
#include <bits/stdc++.h>

using namespace std;

using int64 = long long;

int main() {
  int n;
  cin >> n;
  vector<int> a(n);
  for (int &i : a) {
    cin >> i;
  }
  sort(a.begin(), a.end());
  int64 ans = 0;
  for (int &i : a) {
    ans += abs(a[(n - 1) / 2] - i);
  }
  cout << ans << '\n';
}
```
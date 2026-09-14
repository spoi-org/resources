---
draft: false
title: "Number Spiral"
editorial:
  platform: "CSES"
  category: "Introductory Problems"
  name: "Number Spiral"
weight: 1
---
*Editorial written by Sri Vidya Sundar.*

{{< problem "cses-number-spiral" >}}

## Abridged problem statement
Given a grid with numbers filled in the form of an outward spiral, find the number that is present in the cell $(x,y)$.

## Observation
Consider a square from the top left corner to a cell $(a,a)$. This square contains all the numbers from $1$ to $a^2$. For example, a square starting at cell $(1,1)$ and ending at cell $(3,3)$ has values from $1$ to $9$.

<div style="position:relative; margin:0 auto; width:300px; height:300px; font-family:sans-serif; text-align:center; color:black; background:transparent;">
  <div style="display:grid; grid-template-columns:repeat(5, 1fr); grid-template-rows:repeat(5, 1fr); gap:6px; width:300px; height:300px; box-sizing:border-box; padding:6px; margin:0;">
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">1</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">2</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">9</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">10</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">25</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">4</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">3</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">8</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">11</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">24</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">5</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">6</div>
    <div style="background-color:#2196F3; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">7</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">12</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">23</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">16</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">15</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">14</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">13</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">22</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">17</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">18</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">19</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">20</div>
    <div style="background-color:#E0E0E0; display:flex; align-items:center; justify-content:center; font-weight:bold; font-size:1.2rem;">21</div>
  </div>
</div>

## Solution
Now let us divide the grid into squares starting from the top left corner of the grid. 
Let us pick a cell $(x,y)$ outside a square of dimensions $a \times a$. We know that the value on cell $(x,y)$ has to be greater than $a^2$ (because all the numbers from $1$ to $a^2$ are already already completely contained inside that square). 
So, how do we use this to solve this problem?
Consider the largest square that does not contain the cell $(x,y)$—that is—the square with dimensions $(\max(x,y)-1)\times(\max(x,y)-1)$. This square being filled out uses all numbers from $1$ to $(\max(x,y)-1)^2$. After this, we have two cases:

<style>
.number-spiral-methods{display:flex;flex-wrap:wrap;justify-content:center;align-items:flex-start;gap:24px;margin:1rem auto;font-family:sans-serif}
.number-spiral-figure{display:flex;flex-direction:column;align-items:center;width:fit-content;margin:0}
.number-spiral-grid{position:relative;width:300px;height:300px;text-align:center;color:black;background:transparent}
.number-spiral-cells{display:grid;grid-template-columns:repeat(5,1fr);grid-template-rows:repeat(5,1fr);gap:6px;width:300px;height:300px;box-sizing:border-box;padding:6px}
.number-spiral-cells div{display:flex;align-items:center;justify-content:center;font-weight:bold;font-size:1.2rem}
.number-spiral-blue{background:#2196F3}
.number-spiral-gray{background:#E0E0E0}
.number-spiral-arrow{position:absolute;inset:0;width:300px;height:300px;pointer-events:none;z-index:1}
.number-spiral-figure figcaption{margin-top:.75rem;text-align:center}
</style>
<div class="number-spiral-methods">
<figure class="number-spiral-figure">
<div class="number-spiral-grid">
<div class="number-spiral-cells">
<div class="number-spiral-blue">1</div>
<div class="number-spiral-blue">2</div>
<div class="number-spiral-blue">9</div>
<div class="number-spiral-gray">10</div>
<div class="number-spiral-gray">25</div>
<div class="number-spiral-blue">4</div>
<div class="number-spiral-blue">3</div>
<div class="number-spiral-blue">8</div>
<div class="number-spiral-gray">11</div>
<div class="number-spiral-gray">24</div>
<div class="number-spiral-blue">5</div>
<div class="number-spiral-blue">6</div>
<div class="number-spiral-blue">7</div>
<div class="number-spiral-gray">12</div>
<div class="number-spiral-gray">23</div>
<div class="number-spiral-gray">16</div>
<div class="number-spiral-gray">15</div>
<div class="number-spiral-gray">14</div>
<div class="number-spiral-gray">13</div>
<div class="number-spiral-gray">22</div>
<div class="number-spiral-gray">17</div>
<div class="number-spiral-gray">18</div>
<div class="number-spiral-gray">19</div>
<div class="number-spiral-gray">20</div>
<div class="number-spiral-gray">21</div>
</div>
<svg class="number-spiral-arrow" viewBox="0 0 300 300">
<defs>
<marker id="arrowhead-method-1" markerWidth="6" markerHeight="6" refX="2" refY="3" orient="auto">
<path d="M 0 0 L 6 3 L 0 6 Z" fill="#1565C0" fill-opacity="0.62"/>
</marker>
</defs>
<path d="M 209 32 L 209 209 L 32 209" fill="none" stroke="#1565C0" stroke-width="4.5" stroke-opacity="0.62" marker-end="url(#arrowhead-method-1)" stroke-linejoin="round"/>
</svg>
</div>
<figcaption>Case (i)</figcaption>
</figure>
<figure class="number-spiral-figure">
<div class="number-spiral-grid">
<div class="number-spiral-cells">
<div class="number-spiral-blue">1</div>
<div class="number-spiral-blue">2</div>
<div class="number-spiral-blue">9</div>
<div class="number-spiral-blue">10</div>
<div class="number-spiral-gray">25</div>
<div class="number-spiral-blue">4</div>
<div class="number-spiral-blue">3</div>
<div class="number-spiral-blue">8</div>
<div class="number-spiral-blue">11</div>
<div class="number-spiral-gray">24</div>
<div class="number-spiral-blue">5</div>
<div class="number-spiral-blue">6</div>
<div class="number-spiral-blue">7</div>
<div class="number-spiral-blue">12</div>
<div class="number-spiral-gray">23</div>
<div class="number-spiral-blue">16</div>
<div class="number-spiral-blue">15</div>
<div class="number-spiral-blue">14</div>
<div class="number-spiral-blue">13</div>
<div class="number-spiral-gray">22</div>
<div class="number-spiral-gray">17</div>
<div class="number-spiral-gray">18</div>
<div class="number-spiral-gray">19</div>
<div class="number-spiral-gray">20</div>
<div class="number-spiral-gray">21</div>
</div>
<svg class="number-spiral-arrow" viewBox="0 0 300 300">
<defs>
<marker id="arrowhead-method-2" markerWidth="6" markerHeight="6" refX="2" refY="3" orient="auto">
<path d="M 0 0 L 6 3 L 0 6 Z" fill="#1565C0" fill-opacity="0.62"/>
</marker>
</defs>
<path d="M 32 268 L 268 268 L 268 32" fill="none" stroke="#1565C0" stroke-width="4.5" stroke-opacity="0.62" marker-end="url(#arrowhead-method-2)" stroke-linejoin="round"/>
</svg>
</div>
<figcaption>Case (ii)</figcaption>
</figure>
</div>

We use (i) when the blue square has an odd length, and (ii) otherwise. Without loss of generality, let us assume $x \ge y$ (the other case is symmetric):

Before reaching row/column $x$, the spiral has already filled the $(x-1)\times(x-1)$ square, so the last used number is $(x-1)^2$.

In case (i), we move $y$ more cells, so the answer is

$$
(x-1)^2+y.
$$

In case (ii), we first move $x$ cells, then move back $x-y$ cells. So the answer is

$$
(x-1)^2+x+(x-y)=(x-1)^2+2x-y.
$$

## Implementation
``` cpp
#include <bits/stdc++.h>

using namespace std;

int32_t main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  long long t;
  cin >> t;
  while (t--) {
    long long x, y;
    cin >> x >> y;
    if (x > y) {
      if (x % 2 == 1) {
        cout << ((x - 1) * (x - 1) + y);
      } else {
        cout << ((x - 1) * (x - 1) + 2 * x - y);
      }
    } else {
      if (y % 2 == 0) {
        cout << ((y - 1) * (y - 1) + x);
      } else {
        cout << ((y - 1) * (y - 1) + 2 * y - x);
      }
    }
    cout << "\n";
  }
}
```
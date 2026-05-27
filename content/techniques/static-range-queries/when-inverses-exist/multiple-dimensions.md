---
draft: false
title: 'Multiple dimensions'
editorial:
  platform: "CSES"
  category: "Range Queries"
  name: "Forest Queries"
weight: 3
---

{{< problem "cses-forest-queries" >}}

This time, we have an analogue of the first problem &mdash; but on a grid $A$ of size $N \times N$, as subrectangle queries. The naive solution would take $O(QN^2)$ time, which is obviously too slow. The first obvious optimization would be to instead create a prefix sum array for each row, resulting in $O(N^2 + QN)$ operations. This might be enough to pass for $Q = 2 \cdot 10^5$ and $N = 1000$ as in the problem above, but we want an even faster solution.

Similar to what we did for the 1D version, we will build a "prefix matrix" $P$, where $P[i][j]$ is the sum of the subrectangle bounded by the cells $(1, 1)$ and $(i, j)$. To calculate this matrix, notice how we can sum $P[i - 1][j]$ and $P[i][j - 1]$, and then subtract $P[i - 1][j - 1]$ as it was counted twice. Finally, we have to add $A[i][j]$.

<br>

<div style="display:flex; justify-content:center;">
  <table style="width:auto; border-collapse:collapse; text-align:center;">
    <caption>The green area is initially counted twice</caption>
    <tr>
      <td style="width:48px;height:48px;background-color:color-mix(in lch, #0ff, #f00);">1</td>
      <td style="width:48px;height:48px;background-color:color-mix(in lch, #0ff, #f00);">2</td>
      <td style="width:48px;height:48px;background-color:#f00;">3</td>
    </tr>
    <tr>
      <td style="width:48px;height:48px;background-color:color-mix(in lch, #0ff, #f00);">4</td>
      <td style="width:48px;height:48px;background-color:color-mix(in lch, #0ff, #f00);">5</td>
      <td style="width:48px;height:48px;background-color:#f00;">6</td>
    </tr>
    <tr>
      <td style="width:48px;height:48px;background-color:#0cc;">7</td>
      <td style="width:48px;height:48px;background-color:#0cc;">8</td>
      <td style="width:48px;height:48px;">9</td>
    </tr>
  </table>
</div>

Using this prefix matrix is similar to building it. Assume we wish to query the subrectangle $(x, y, X, Y)$. After adding $P[X][Y]$ and subtracting both $P[x - 1][Y]$ and $P[X][y-1]$ $P[x-1][y-1]$ has been subtracted twice, so we add it once more. This gives us the formula
$$
\sum_{i = x}^{X} \sum_{j=y}^{Y} A[i][j] = P[X][Y] - P[X][y - 1] - P[x - 1][Y] + P[x - 1][y - 1]
$$

It is simple to generalize this further to $M$ dimensions using the <a href="https://en.wikipedia.org/wiki/Inclusion%E2%80%93exclusion_principle">Inclusion-Exclusion Principle</a>.

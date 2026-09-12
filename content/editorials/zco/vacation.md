---
draft: false
title: "Vacation"
editorial:
  platform: "ZCO"
  name: "Vacation"
---

{{< problem "Vacation" >}}

# ZCO 2022 - Vacation
## Subtask 4 (Q <= 5)

Notice that each cell from (A, B) to (C, D) has at least one path passing through it.
If any cell from (A, B) to (C, D) has a cost of 0, then we can use the path that passes through the cell with a cost of 0 to get a path of 0 cost.

![Grid](https://pub-e92ad040f113456098b524bb71804b18.r2.dev/editor-images/d8e0d9ec-d05d-4792-9fe5-b1fdf06d28a5-1789194441085-oFWGC5w8y4p7FKyzGWX7PiJnq4p31dyC.png)

If none of the cells from (A, B) to (C, D) is 0, then no matter which path we choose, we will get a cost of 1.

The time complexity of this is O(N \times M) per day/query, since we check each cell between (A, B) and (C, D). Since there are Q queries/days, the time complexity of the solution is O(Q \times N \times M) which passes this subtask.

## Subtask 5 (At most 10 cells in the grid have a cost of 0)

We need to check whether any of the cells between (A, B) and (C, D) has a cost of 0 with a time complexity faster than O(N \times M) to improve the last solution and make it pass this subtask.

Since there are at most 10 cells with a cost of 0, we can store the positions of all cells with a cost of 0 and check whether any of these cells lies between (A, B) and (C, D) (Let the position be (x, y). Then, we check whether A <= x <= C and B <= y <= D)

This will only take 10 checks since there are at most 10 cells with a cost of 0 and hence the time complexity of this solution is O(Q \times Z) where Z is the number of zeros which passes this subtask.

## Subtask 6 (N = 1)

Prerequisite: Prefix Sums [If you do not know prefix sums, [this is a good resource](https://usaco.guide/silver/prefix-sums)]

Since N = 1, this is basically a 1D array. Let's call this array C.

We need to check whether the cost of any of the cells from C[a] to C[b] is 0 for some a and b in O(1), using preprocessing if needed.

This can be done using prefix sums.
Let pref[i] be the number of 0s from index 1 to i.

The code will look something like this:
```
if (C[i] == 0) pref[i] = pref[i-1]+1;
else pref[i] = pref[i-1];
```

We can do pref[b] - pref[a-1] to find the number of 0s between a and b. If pref[b] - pref[a-1] is greater than 0, we know that there is at least one 0 between a and b and the cost of the path will be 0, otherwise the cost of the path will be 1.

The time complexity of this solution is O(M) for preprocessing and O(Q) for processing the queries. Therefore, the total time complexity of this solution is O(M + Q), which passes this subtask.

## Subtask 8 (No additional constraints)

Prerequisite: 2D Prefix Sums [If you do not know 2D prefix sums, [this is a good resource](https://usaco.guide/silver/more-prefix-sums#2d-prefix-sums)]

We can extend the solution of Subtask 6 to use 2D prefix sums, which allows us to query the number of 0s between (A, B) and (C, D) in O(1).

The time complexity of this solution is O(N \times M) for preprocessing and O(Q) for the queries. Therefore, the total time complexity of this solution is O(N \times M + Q), which passes this subtask.

[C++ Code](https://www.codechef.com/viewsolution/1356355671)

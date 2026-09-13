---
draft: false
title: 'Smart Phone'
editorial:
  platform: "ZCO"
  name: "Smart Phone"
---

*Editorial written by Ekansh Majumder.*

{{< problem "zco-smart-phone" >}}

The price can be any value in the range $[1, 10^{8}]$, and there are up to $5000$ customers. Brute-forcing this by iterating over all possible prices will not pass the time constraints.

## Observation 1

Notice that the answer can only be one of the values in the input array. This is because the number of buyers does not change if you choose any value in between two elements in the array. 

For example, take an array of size $3$:
$$
[2, 5, 10]
$$

* If we set the phone price to $10$, only the third user can buy the phone, and anyone before it cannot.
* If we set the price to anything in the range $(5, 10)$, notice that the set of buyers remains same, but our profit per unit only decreases.
* The next change in the set of buyers happens when we set the price down to $5$; now both the second and third users can buy the phone.
* There is no change again for any price in the range $(2, 5)$.

Thus, we have reduced the search space from $[1, 10^{8}]$ candidate prices to only the $N$ values present in the input array.

## Subtask 1

In this subtask, $N \le 5000$. There are at most 5000 customers to check.

This is the brute-force solution, as we can afford an $\mathcal{O}(N^2)$ approach.

Here is a code snippet for the same:

```cpp
for (int i = 0; i < n; i++) {
  int count = 0;
  for (int j = 0; j < n; j++) {
    if (budgets[j] >= budgets[i]) {
      count++;    
    }
  }
  int curr = budgets[i] * count; // Current profit
  ans = max(max_revenue, curr); 
}
```

We iterate through the `budgets` array and set the candidate price to $\text{budgets}[i]$. We then loop through all $j$ from $0$ to $N - 1$ to find the number of customers who can afford the phone. Finally, we calculate the profit and maintain the maximum over all $i$.

The time complexity of this solution is $\mathcal{O}(N^2)$ as we use another for loop inside our initial for loop to iterate through all pairs.

## Observation 2

We are currently taking $\mathcal{O}(N)$ time to check the number of buyers for each price. Let's try to notice a pattern in this array. To begin with, let's sort it in non-increasing order (we could've also chosen non-decreasing):

$$
[53, 30, 20, 14]
$$

* When we set the price to $14$, we have $4$ buyers.
* When we set the price to $20$, we have $3$ buyers.
* When we set the price to $30$, we have $2$ buyers.

This pattern continues as long as the array remains sorted.
If the array is sorted in non-increasing order then, for each budget, everyone to the left of that budget (along with the current customer) can buy. Therefore, the number of buyers is simply the 1-based index of the budget, or $i + 1$ with 0-based indexing.

## Subtask 2

In this subtask, $N \le (10^5)$. We will try to optimize our current approach to get a $\mathcal{O}(N)$ solution.

Using observation 2, we can sort the array in non-increasing order. For each budget, the profit we can achieve is:

$$
\text{profit} = \text{budgets}[i] \times (i + 1)
$$

```c++
sort(budgets.rbegin(), budgets.rend());
int ans = 0;
for (int i = 0; i < n; i++) {
  int profit = budgets[i] * (i + 1);
  ans = max(ans, profit);
}
```

Let's analyze the time complexity of this snippet:
* Sorting the array takes $\mathcal{O}(N \log N)$.
* Iterating through the array takes $\mathcal{O}(N)$.

This gives an overall time complexity of:

$$
\mathcal{O}(N \log N + N) = \mathcal{O}(N \log N)
$$

The time complexity of this solution is $\mathcal{O}(N \log N)$ as sorting the array takes $\mathcal{O}(N \log N)$, and iterating through the array takes $\mathcal{O}(N)$.

## Implementation

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int n;
  cin >> n;

  vector<long long> budgets(n);
  for (int i = 0; i < n; i++) {
    cin >> budgets[i];
  }

  sort(budgets.rbegin(), budgets.rend());

  long long ans = 0;
  for (int i = 0; i < n; i++) {
    long long profit = budgets[i] * (i + 1);
    ans = max(ans, profit);
  }

  cout << ans << endl;
}
```
---
draft: false
title: 'Parchment'
editorial:
  platform: "ZCO"
  name: "Parchment"
---

*Editorial written by Ekansh Majumder.*

{{< problem "zco-parchment" >}}

[Binary search]({{< ref "techniques/binary-search.md" >}}) is a prerequisite for subtask 6 and above.

The solution to this problem was also discussed [here](https://www.youtube.com/watch?v=aH8i4dzOvpU) on the SPOI YouTube channel.

## Subtask #1

In this subtask, $F=1$. All the numbers are in the range $[1, 1]$, which means every element in the array is $1$.

A single segment of length $K = 0$ (which covers $[1, 1]$) can cover all numbers in just one operation. Since $F_j \ge 1$ for all queries, the answer is simply $0$ for every query.

```cpp
for (int i = 0; i < q; i++) {
  cout << 0 << "\n";
}
```

The time complexity of this solution is $\mathcal{O}(Q)$ since we iterate through the $Q$ queries and output $0$ for each query in $\mathcal{O}(1)$ time.

## Subtask #2

In this subtask, $F = 1$. You are allowed to use at most 1 segment to cover the entire array.

To cover all points using a single interval $[X, X + K]$, the interval must stretch from the minimum element to the maximum element:

$$K = \max(P) - \min(P)$$

The answer is the difference between the largest and the smallest element in the input array.

For example, given the array $[7, 4, 1, 8, 5, 2]$ and $F = 1$; to cover the entire array in one operation, we need to cover the span $[1, 8]$. The difference between the maximum and the minimum element is $8 - 1 = 7$, so we delete $[1, 1 + 7]$.

```cpp
int mini = P[0], maxi = P[0];
for (int i = 1; i < N; i++) {
  mini = min(mini, P[i]);
  maxi = max(maxi, P[i]);
}
int ans = maxi - mini;
```

The time complexity of this solution is $\mathcal{O}(N + Q)$.

## Subtask #3

Here, $F \le 2$.

We can divide this subtask into two cases. For $F = 1$, we use the same solution as subtask 2. We now focus on the case where $F = 2$.

First, sort the array in non-decreasing order: $P_0 \le P_1 \le \dots \le P_{N-1}$.

Notice that intervals are continuous, so one segment must start at index $0$ and cover a prefix of the array up to some index $i$, while the other segment covers the remaining suffix from index $i + 1$ to the maximum element at index $N - 1$.

For any chosen split point $i$:
* The first segment covers $[P_0, P_i]$, requiring a length of at least $P_i - P_0$.
* The second segment covers $[P_{i+1}, P_{N-1}]$, requiring a length of at least $P_{N-1} - P_{i+1}$.

Since both segments use the same power level $K$, the device requires:

$$K = \max(P_i - P_0, \; P_{N-1} - P_{i+1})$$

To find the minimum power level, iterate through all $i$ ($0 \le i < N - 1$) and take the minimum of these values.

```cpp
sort(P.begin(), P.end());
int ans = P[N - 1] - P[0];

for (int i = 0; i < N - 1; i++) {
  int left = P[i] - P[0];
  int right = P[N - 1] - P[i + 1];
  
  int req = max(left, right);
  ans = min(ans, req);
}
```

The time complexity of this solution is $\mathcal{O}(N \log N + Q)$, with the log factor incurred due to having to sort the array.

## Subtask #4

> $A \le 2$

Every element is in the range $[1, 2]$. We divide the problem into three cases:

* **Case 1:** The array contains only $1$s. The answer is simply $0$ (refer to subtask 1).
* **Case 2:** The array contains only $2$s. The answer is simply $0$ (refer to subtask 1).
* **Case 3:** The array contains both $1$s and $2$s:
  * If $F = 1$: We can only use $1$ segment. It must cover both $1$ and $2$, so $K = 2 - 1 = 1$.
  * If $F > 1$: We can use $2$ separate segments of length $K = 0$ (one on $[1, 1]$ and one on $[2, 2]$). Thus, the answer is $0$.

```cpp
bool has_1 = false, has_2 = false;
for (int i = 0; i < N; i++) {
  if (P[i] == 1) has_1 = true;
  if (P[i] == 2) has_2 = true;
}

for (int t = 0; t < q; t++) {
  int f;
  cin >> f;
  if (has_1 && has_2 && f == 1) {
    cout << 1 << "\n";
  } else {
    cout << 0 << "\n";
  }
}
```

The time complexity of this solution is $\mathcal{O}(N + Q)$.

## Subtask #5

*(Reading this subtask is optional but recommended.)*

> $N, Q \le 10$ and $A \le 1000$

Notice that the answer will always be in the range $[0, A]$. Anything bigger is not required since $K = A$ clears the whole range. **Keep this property in mind**, as we will use it for later subtasks and the full solution.

Iterate linearly from $0$ to $A$. For each candidate $K$, greedily count how many intervals of length $K$ are needed to cover the entire sorted array.

The very first $K$ that is acceptable (requires $\le F$ segments) is guaranteed to be the minimum valid $K$, as every smaller value failed. We can stop and output it as the answer.

For a fixed $K$, run an outer loop over the array, increment the segment count, and use an inner loop to advance past all elements covered within $[P_i, P_i + K]$.

```cpp
int getCount(vector<int> P, int K) {
  int count = 0;
  int i = 0;
  int n = P.size();
  while (i < n) {
    count++;
    int last = P[i] + K;
    while (i < n && P[i] <= last) {
      i++;
    }
  }
  return count;
}
```

Query loop:
```cpp
for (int k = 0; k <= A; k++) {
  if (getCount(P, k) <= F) { // P is the input array
    cout << k << "\n";
    break;
  }
}
```

The time complexity of this solution is $\mathcal{O}(N \log N + Q \cdot A \cdot N)$:
  * Sorting the array takes $\mathcal{O}(N \log N)$.
  * For each of the $Q$ queries, we test values of $K$ from $0$ up to $A$ (at most $A + 1$ candidates).
  * Checking each candidate $K$ takes $\mathcal{O}(N)$ time.

---

## Subtask #6

> **Note:** This approach also passes subtasks 5, 6, 7, and 8. Combined with the first four subtasks, this scores *75/100 points*. We will skip directly to the full solve after this subtask.

*(Reading subtask 5 is recommended, as this is an optimization of that method.)*

In subtask 5, we established that the answer will always be in the range $[0, A]$.

* If a length $K$ can cover all points in $\le F$ operations, then any $K' > K$ can also do it.
* If a length $K$ cannot cover all points in $\le F$ operations, then any $K' < K$ can also not do it.

The validity of $K$ is **monotonic**:

$$\{\dots, \text{false}, \text{false}, \text{false}, \text{true}, \text{true}, \text{true}, \dots\}$$

This means we do not need to linearly search for $K$. We can **binary search on the answer $K$** to find the minimum value that satisfies the condition. Our verification function is identical to the greedy check in Subtask 5.

```cpp
#include <bits/stdc++.h>

using namespace std;

// Checking if the given K is valid
bool can(int K, int F, vector<int> &nums) {
  int n = nums.size();
  int uses = 0;
  int i = 0;
  while (i < n) {
    uses++;
    int end = nums[i] + K;
    while (i < n && nums[i] <= end) i++;
    if (uses > F) return false; // Early exit
  }
  return uses <= F;
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int n, A;
  cin >> n >> A;
  vector<int> nums(n);
  for (int i = 0; i < n; i++) cin >> nums[i];
  sort(nums.begin(), nums.end());

  int q;
  cin >> q;
  vector<int> F(q);
  for (int i = 0; i < q; i++) cin >> F[i];

  for (int f : F) {
    int low = 0, high = A, ans = A;
    while (low <= high) {
      int mid = (low + high) / 2;
      if (can(mid, f, nums)) {
        ans = mid;
        high = mid - 1;
      } else {
        low = mid + 1;
      }
    }
    cout << ans << "\n";
  }
}
```

The time complexity of this solution is $\mathcal{O}(N \log N + Q \cdot N \log A)$:
  * Sorting the array $P$ initially takes $\mathcal{O}(N \log N)$.
  * For each query, binary search on $K \in [0, A]$ runs for $\lceil \log_2(A + 1) \rceil \approx 20$ iterations.
  * In each binary search step, the greedy verification function runs in $\mathcal{O}(N)$ time.

## Full solution

*(Reading subtask 6 is recommended before this section.)*

The solution precomputes the minimum number of operations required for every possible $K \in [0, A]$ and stores it in an array `cache`, where `cache[k]` represents the minimum operations needed for power level $k$.

Notice that `cache` is sorted in **non-increasing order**: as $K$ increases, the number of required operations decreases.

Each query can then be answered directly by binary searching on `cache`.

---

Instead of manually stepping element-by-element inside the simulation loop, we use `upper_bound` to jump directly to the first element strictly greater than $a[i] + K$.

```cpp
#include <bits/stdc++.h>

using namespace std;

#define int long long

vector<int> cache; // cache[k] is the min number of operations required with K = k

void cacheFill(int &k, vector<int> &a) {
  // Filling up the cache array
  cache.resize(k);
  int n = a.size();
  for (int currk = 0; currk < k; currk++) {
    int i = 0;
    int uses = 0;
    while (i < n) {
      uses++;
      int end = a[i] + currk;
      i = upper_bound(a.begin() + i, a.end(), end) - a.begin();
    }
    cache[currk] = uses;
  }
}

int32_t main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int n, a;
  cin >> n >> a;

  vector<int> arr(n);
  for (int i = 0; i < n; i++) {
    cin >> arr[i];
  }
  sort(arr.begin(), arr.end());

  int limits = a + 1;
  cacheFill(limits, arr);

  int q;
  cin >> q;
  while (q--) {
    int f;
    cin >> f;

    int low = 0;
    int high = limits - 1;
    int ans = limits;

    while (low <= high) {
      int mid = (low + high) / 2;
      if (cache[mid] <= f) {
        ans = mid;
        high = mid - 1;
      } else {
        low = mid + 1;
      }
    }
    cout << ans << '\n';
  }

  return 0;
}
```

The time complexity of this solution is $\mathcal{O}(A \log A \cdot \log N + Q \log A)$:
  * Sorting the input array takes $\mathcal{O}(N \log N)$.
  * In the precomputation step, for a fixed interval length $\text{currk}$, the simulation advances by at least $\text{currk} + 1$ in value each jump, resulting in at most $\frac{A}{\text{currk} + 1} + 1$ jumps.
  * Summing over all $\text{currk} \in [0, A]$, the total number of jumps is bounded by the Harmonic series:
    $$\sum_{\text{currk}=0}^{A} \frac{A}{\text{currk} + 1} \approx A \log{A}$$
  * Each jump uses `upper_bound`, costing $\mathcal{O}(\log N)$, giving a precomputation time of $\mathcal{O}(A \log A \cdot \log N)$ (around $2.5 \times 10^8$ operations).
  * Each of the $Q$ queries is answered via binary search over `cache` of size $A + 1$ in $\mathcal{O}(\log A)$ time.

---
draft: false
title: 'Raab Game I'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Raab Game I"
weight: 1
---

{{< problem "cses-raab-game-i" >}}

## Hint

Try to find the condition for when the scores can't be from an actual game. If they are from an actual game, come up with a systematic way to ensure player one wins $a$ times and player 2 wins $b$ times.

## Solution

For each test case, we first check if such a score is possible. The sum of the scores can't be greater than $n$ because in total, there can only be $n$ wins across $n$ rounds. 
It's also impossible for the score of 1 player to be $0$. This is because even if the winning player always plays a number 1 greater than the nil-scoring player, the last round is guaranteed to be winning for the nil-scoring player because they will have $n$ which beats the winning players $1$.

If the scores are from a valid game, we begin by printing numbers from $1$ to $n$, representing the cards played by the first player.

The second line, representing the corresponding moves of the second player, is constructed carefully to control pairwise comparisons.
The first $b$ elements are taken from the range $a+1$ to $a+b$, ensuring they are strictly larger than the next block and hence guarantee $b$ wins.
Next, the smallest $a$ elements, $1$ to $a$, are placed, ensuring wins for the first player.
The remaining elements are appended in increasing order, resulting in draws for the remaining positions.
This construction satisfies all constraints while maintaining valid permutations.

### Code:

```cpp
#include <iostream>
using namespace std;

void solve(){

    int n, a, b;
    cin >> n >> a >> b;

    // Invalid if required elements exceed n
    if (a + b > n) {
        cout << "NO\n";
        continue;
    }

    // Invalid if exactly one of a or b is zero
    if ((a == 0 || b == 0) && a + b != 0) {
        cout << "NO\n";
        continue;
    }

    cout << "YES\n";

    // 1 to n
    for (int i = 1; i <= n; i++)
        cout << i << " ";
    cout << "\n";

    // First b elements after a
    for (int i = 1; i <= b; i++)
        cout << a + i << " ";

    // Next a smallest elements
    for (int i = 1; i <= a; i++)
        cout << i << " ";

    // Remaining elements
    for (int i = a + b + 1; i <= n; i++)
        cout << i << " ";
    cout << "\n";
}
int main() {
    int t;
    cin >> t;
  //because ints and bools are loosely typed in C++, when t = 0 the while loops ends.
  //The loop runs t cycles by running from t to 1.
    while (t--)
      solve();

    return 0;
}
```

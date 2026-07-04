---
draft: false
title: 'Two Sets'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Two Sets"
weight: 1
---

{{< problem "cses-two-sets" >}}

## Hint

Try to make two sets for many different values of $n$. Try to find some patterns that emerge from the process and see if they apply generally.

## Solution

If you attempted to make two sets for different values of $n$, you would notice the first value for $n$ when this is possible is $n = 3$. Then next value would be $n = 4$, then 7, 8, and so on. What's the patterns between these numbers?

Well for $n = 3$, the 2 sets are ${1,2}$ and ${3}$. For $n = 4$ the two sets are ${1,4}$ and ${2,3}$. Notice how we paired the 1st with the 4th number and the 2nd and 3rd number. This holds true for any sequence of 4 ascending numbers. The proof for that is as follows:

If you're given 4 ascending numbers $x, x+1, x+2, x+3$, the 1st and 4th numbers will add up to $(x) + (x + 3) = 2x + 3$ which is the same as the sum of the 2nd and 3rd numbers which is $(x + 1) + (x + 2) = 2 x + 3$.

If $n$ is a multiple of $4$, you can always break up the numbers into two sets because for each group of 4, you can put one pair in one set and the other pair in another set.

The only other case is when $n$ is 3 more than a multiple of $4$. This is because of the special case of $n = 3$. The first 3 numbers can be split into ${1, 2}$ and ${3}$ and the remaining are now a multiple of 4, allowing you to split them as shown previously.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    
    //if n is not a multiple of 4 or 3 more than a multiple of 4, output "NO".
    if (n % 4 == 1 || n % 4 == 2) 
      cout<<"NO";

    else {

        cout<<"YES"<<endl;

        // Vectors to store the two sets.
        vector<int> a, b;

        // Process numbers in groups of 4 from largest to smallest,
        // assigning to sets such that each group adds equal sum to both.
        while (n > 3 && n > 0) {
                a.push_back(n);
                a.push_back(n-3);

                b.push_back(n-1);
                b.push_back(n-2);

                n = n - 4;
        }

        // Handle the remaining 3 numbers if n % 4 == 3 (balanced assignment).
        if (n == 3) {
            a.push_back(3);

            b.push_back(2);
            b.push_back(1);
        }

        // Output size and elements
        cout << a.size() << "\n";
        for (int num : a)
            cout << num << " ";

        cout << b.size() << "\n";
        for (int num : b)
            cout << num << " ";

        return 0;
    }
}
```

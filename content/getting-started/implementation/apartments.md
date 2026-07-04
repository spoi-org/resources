---
draft: false
title: 'Apartments'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Apartments"
weight: 1
---

{{< problem "cses-apartments" >}}

## Solution

We can sort both applicants and apartments, then uses a two pointer approach to match each applicant with the smallest available apartment whose size differs by at most $k$.

The two pointer approach is when you either move both pointers, or one of the pointers based on a condition.

In this case if an apartment is too small for the current applicant, we move to the next apartment which is larger.
If an apartment is too large for the current applicant, we move to the applicant who wants a large apartment.
Lastly if an apartment is of the right size, we increase the answer by 1 and go to the next apartment and next applicant.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m, k;
    cin >> n >> m >> k;

    vector<int> applicants(n), apartments(m);

    // Read applicant preferences
    for (int i = 0; i < n; i++)
        cin >> applicants[i];

    // Read apartment sizes
    for (int i = 0; i < m; i++)
        cin >> apartments[i];

    // Sort both arrays
    sort(applicants.begin(), applicants.end());
    sort(apartments.begin(), apartments.end());

    int count = 0;
    int i = 0, j = 0;

    // Two-pointer approach to match applicants to apartments
    while (i < n && j < m) {
        // Check if current apartment fits current applicant's preference
        if (abs(applicants[i] - apartments[j]) <= k) {
            count++;//increase the answer
            i++;//move to next applicant
            j++;//move to next largest appartment
        }
        // If apartment is too small, try the next larger apartment
        else if (applicants[i] - apartments[j] > k)
            j++;
        // If apartment is too big, try the next applicant who wants a bigger appartment
        else
            i++;
    }

    cout << count << endl;
    return 0;
}

```

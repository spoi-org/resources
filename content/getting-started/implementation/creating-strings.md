---
draft: false
title: 'Creating Strings'
editorial:
    platform: "CSES"
    category: "Introductory Problems"
    name: "Creating Strings"
weight: 1
---

{{< problem "cses-creating-strings" >}}

## Solution 

In `C++` there is a very useful function called `next_permutation()` which helps us tackle this exact question. This function can be used to generate the next lexicographical sequence for a string or a vector.

It returns false when no other greater permutations exists, otherwise it rearranges the string or the vector.

### Code:

```cpp

#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;

    //sort the string to get the lowest possible lexiographical sequence
    sort(s.begin(), s.end());

    vector<string> v;

    do {
        v.push_back(s);
    } while (next_permutation(s.begin(), s.end()));
    // returns false if no other permutation exists
    // otherwise it rearranges the string

    cout << v.size() << "\n";
    for (int i = 0; i < v.size(); i++) {
        cout << v[i] << "\n";
    }
}

```

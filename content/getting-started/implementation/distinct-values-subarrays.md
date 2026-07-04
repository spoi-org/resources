---
draft: false
title: 'Distinct Values Subarrays'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Distinct Values Subarrays"
weight: 1
---

{{< problem "cses-distinct-values-subarrays" >}}

## Solution

This code uses a sliding window to count subarrays with all distinct elements. The right pointer expands the window, while a frequency map tracks duplicates. If a duplicate appears, the left pointer shrinks the window until all elements are unique again. At each position, the number of valid subarrays ending there is added to the answer.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    // Frequency map for elements in the current window
    map<int, int> freq;

    int left = 0;          // Left pointer of the sliding window
    long long answer = 0;  // Total number of valid subarrays

    for (int right = 0; right < n; right++) {
        freq[a[right]]++;  // Add current element to the window

        // Shrink window until all elements are distinct
        while (freq[a[right]] > 1) {
            freq[a[left]]--;
            left++;
        }

        // Number of distinct subarrays ending at 'right'
        answer += (right - left + 1);
    }

    cout << answer << "\n";
}
```

## Alternate Solution

You can use a `set` instead of a `map`. This works by storing all elements in the current window in the `set`, when a duplicate of the element at the right pointer is found, you move the left pointer of the window and keep removing elements from the `set` until the duplicate element is removed, then add the element at the right pointer to the window.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;
 
int main(){
  //Accepting Input:

  int n;
  cin >> n;

  vector<int> v(n);
  for(int i = 0; i < n; i++)
    cin >> v[i];
  
  //Computing Answer:
  long long ans = 0;
  set<int> s; //stores the elements in the current window
  for(int l = 0, r = 0; r < n; r++){
    //The while removes keeps removing elements from the window until the duplicate of v[r] is removed.
    while(s.find(v[r]) != s.end()){
      s.erase(v[l]);
      l++;
    }

    s.insert(v[r]);//and the current element to the window
    ans += r-l+1;//number of subarrays ending at r 
  }

  cout << ans << "\n";
  return 0;
}
```

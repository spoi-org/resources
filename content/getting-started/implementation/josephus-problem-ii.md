---
draft: false
title: 'Josephus Problem II'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Josephus Problem II"
weight: 1
---

{{< problem "cses-josephus-problem-ii" >}}

## Hint

If you've thought about the question for a while, you would have realised that you need the ability to the following operations efficiently (i.e $O(log n)$):

+ Jump from a certain number to the next number $k$ places ahead.
+ Delete the element at the current index. 

 explains the data structure known as a Fenwick tree which gives you to ability to do these operations.

## Solution

We can use the Fenwick Tree (Binary Indexed Tree) as an indexed set to efficiently jump by any amount $k$ and remove elements. 

Set every element from 1 to $n$ in the fenwick tree to 1 to indicate they are all in the list 1 time.
Start at index 0, then jump $k$ places mod n so its circular, use the `search()` function to find what number is at that index + 1 (Fenwick Trees are 1 indexed) and remove it by subtracting it's frequency by 1.

The time complexity is $O(n log n)$: $n$ removals, each taking $O(log n)$ for searching and removing.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<int> fenw;

void add(int x, int k){
  for(; x <= n ; x += x & -x)// x & -x is the LSSB(x)
    fenw[x] += k;
}

int search(int idx){
  int ans = 0;

  for(int k = floor(log2(n)); k >= 0; k--){//go through the powers of 2.
    if(ans + (1 << k) <= n && fenw[ans + (1 << k)] < idx){//this element is before the idx.
      ans += 1 << k;//update the answer.
      idx -= fenw[ans];//account for all indices upto fenw[ans].
    }
  }

  return ans + 1;//ans was the value that was before idx, so one value ahead of that is at idx.
}

int main(){

  int k;
  cin >> n >> k;
  fenw.resize(n + 1);//allocating memory to the fenwick tree;

  for (int i = 1; i <= n; i++) 
    add(i, 1);//increase the frequency of all numbers by 1

  for(int rem = n, idx = 0; rem > 0; rem--){//rem is the people remaining in the circle
    // Move k steps forward in circular manner
    idx = (idx + k) % rem;

    // Find the number at index idx + 1 (+1 because a fenwick tree 1 one indexed) 
    int pos = search(idx + 1);
    cout << pos << " ";

    // Mark this number as removed by decreasing frequency to 0
    add(pos, -1);
  }

  return 0;
}

```

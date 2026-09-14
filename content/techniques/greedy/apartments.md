---
draft: false
title: 'Apartments'
editorial:
  platform: "CSES"
  category: "Sorting and Searching"
  name: "Apartments"
weight: 3
---

{{< problem "cses-apartments" >}}

## Abridged problem statement

There are n applicants and m free apartments. Your task is to distribute the apartments so that as many applicants as possible will get an apartment.
Each applicant has a desired apartment size, and they will accept any apartment whose size is close enough to the desired size.

## Solution
First we have to make two vectors in which we store the desired apartment size of the applicants and the sizes of the free apartments. Then we sort both the vectors.

After that, we will use two pointers and a counter to count the number of matches.

In the code below, vn is the desired apartment sizes, vm is the sizes of the free apartments, and k is the maximum allowed difference. Here, vn[i] is the $i$-th desired apartment size and vm[j] is the $j$-th apartment size.

When $vm[j] > vn[i] + k$, we increase $i$, which means we didn't match this applicant. Since the vector is sorted, there is no other value that will be smaller than the current value of vm[j]. So the only way to get a value that is closer to vn[i] is to move forward to the next applicant.

If $vm[j] < vn[i] - k$, this means that the apartment size is too small. So we increase $j$ to find the next apartment, which is always bigger than or equal to the size of the current apartment since the vector is sorted.

In the else condition, we have the matching condition. When there is a match, we increase the counter and also increase both $i$ and $j$, as we have used them now and they can't be used again.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
 
int main() {
    int n,m,k;
    cin >> n >> m >> k;
    vector<int>vn;
    vector<int>vm;
    for(int i = 0;i<n;i++){
        int x;
        cin >> x;
        vn.push_back(x);
    }
    for(int i = 0;i<m;i++){
        int x;
        cin >> x;
        vm.push_back(x);
    }
    sort(vn.begin(),vn.end());
    sort(vm.begin(),vm.end());
    int i,j,c;
    i = j = 0;
    c = 0;
    while(i<n&&j<m){
        if(vm[j]>vn[i]+k){
            i++;
        }
        else if(vm[j]<vn[i]-k){
            j++;
        }
        else{
            c++;
            i++;
            j++;
        }
    }
    cout << c;
}

```

---
draft: false
title: 'Nested Ranges Count'
editorial:
    platform: "CSES"
    category: "Sorting and Searching"
    name: "Nested Ranges Count"
weight: 1
---

{{< problem "cses-nested-ranges-count" >}}

## Hint

Try sorting the intervals in ascending order of left index and descending order of right index. What algorithm could you come up with where you only have to iterate once through the list to get the answer? Think about why this sorting method was mentioned and what advantages it has. 

## Solution

The problem asks us to find two values for each range: the number of other ranges it contains, and the number of other ranges that contain it.
A naive solution comparing every pair would be too slow ($O(n^2)$). We'll need a better approach.

Say we were to sort every interval in ascending order of their left index and in descending order of their right index. This seems pretty random, however if you think about the first range in the sorted list, you can guarantee that any range that comes after it will be inside it simply by checking if it's right index is less than the right index of the first range. 

This applies to all ranges in the sorted list i.e. for any range $i$, any subsequent range $j$ will be contained by $i$ if the right index of range $j$ is less than $i$.

Likewise, the converse also applies, for any range $i$, the **count** of ranges that $i$ is **contained** by is all ranges $j$, where $j < i$, such that right index of $j$ is more than right index of $i$.

Let's look at an example to understand this better:

Say we're given the following ranges, for now they're going to be sorted in the order we want

$
{{1, 10}, {2, 8}, {2, 6}, {5, 7}}
$

Now the first range cannot be contained by anyone, because so the answer for range 0 is 0.

Range 1 is contained by Range 0 because $10 > 8$, so the answer for range 1 is 1.

Range 2 is contained by both range 0 and 1 because $10 > 6 "and" 8 > 6$. Therefore the answer for range 2 is 2.

Lastly range 3 is contained by ranges 0 and 1 but **not** by range 2 because $10 > 7$, $8 > 7$, but $6 < 7$. Therefore the answer for range 3 is 2.

The output of the second line for the question hence would be $0, 1, 2, 2$

This however only ends up solving half the question, we still need to find out for every range $i$, how many ranges it contains.

For this we can use the same sorting order, just iterate in reverse. The reasoning is the same as before: for any range $i$, the **count** of ranges that $i$ **contains** is the number of ranges $j$, where $j > i$ such that right index of $j$ is less than the right index of $i$.

Using the ranges given previously, range 3 can contain no other ranges, therefore it's answer is 0.

Range 2 does **not** contain Range 3 because $7 > 6$. Therefore its answer is 0.

Range 1 contains both range 2 and range 3 because $6 < 8$ and $7 < 8$. Therefore its answer is 2.

Lastly range 0 contains range 1, range 2, and range 3, because $8 < 10$, $6 < 10$, and $7 < 10$. Therefore it's answer is 3.

While the approach seems logical, how can we efficiently find out how many ranges have a right end point less than the current range or greater than the current range. For that, we can use a Fenwick tree as an indexed set. You can add the right index of all ranges either succeeding or preceding and then find the how many right indexes are more or less than the current range.

### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> fen;

struct Range{
	int l, r, idx; 

  bool operator<(const Range& ran){//operator overloading for the custom sorting.
    return l < ran.l || l == ran.l && r > ran.r;
  }
};

void add(int x, int k){
	for(; x < fen.size(); x += x & -x)
		fen[x] += k;
}

int sum(int x){
	int ans = 0;
	for(; x > 0; x -= x & -x)
		ans += fen[x];
	return ans;
}

int main(){
	ios_base::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	//freopen("NestedRangesCount.in","r",stdin);
	//freopen("NestedRangesCount.out","w",stdout);
	int n;
	cin >> n;

	vector<Range> v(n);
	vector<int> comp;

	for(int i = 0; i < v.size(); i++){
		cin >> v[i].l >> v[i].r;
		v[i].idx = i;
		comp.push_back(v[i].r);
	}
  
  //Index compression for the right endpoints:
  sort(comp.begin(), comp.end()); 
  comp.erase(unique(comp.begin(), comp.end()), comp.end());

  for(int i = 0; i < v.size(); i++)
    v[i].r = lower_bound(comp.begin(), comp.end(), v[i].r) - comp.begin() + 1; 

  sort(v.begin(), v.end()); 
  fen.resize(comp.size() + 1);

	vector<int> c(n), d(n);// c[i] is the number of ranges v[i] contains, d[i] is the number of ranges that v[i] is contained by.

	for(int i = v.size()-1; i >= 0; i--){
    // for every range, add the number of ranges with r < v[i].r. l > v[i].r because of the sorting order.
    // if l = v[i].l , then the smaller ranges will get added first to correctly find c[i] because of the sorting order.
    // sum(v[i].r) - 1 gives that number.
		add(v[i].r,1);
		c[v[i].idx] = sum(v[i].r) - 1;
	}

  //Resetting the Fewnick tree.
	fen.clear();
	fen.resize(comp.size() + 1);

	for(int i = 0; i < v.size(); i++){
    // for every range, add the number of ranges with r > v[i].r. l < v[i].l because of the sorting order.
    // if l = v[i].l , then the larger ranges will get added first to correctly find d[i] because of the sorting order.
    // sum(fen.size() - 1) - sum(v[i].r-1) - 1 gives that number.
		add(v[i].r,1);
		d[v[i].idx] = sum(fen.size()-1) - sum(v[i].r-1) - 1;
	}

	for(int i = 0; i < c.size(); i++)
		cout << c[i] << " ";
	cout << endl;

	for(int i = 0; i < d.size(); i++)
		cout << d[i] << " ";
	cout << endl;

	return 0;
}
```

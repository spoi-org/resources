---
date: '2026-05-25T21:47:38Z'
draft: false
title: 'Breadth-first search'
weight: 3
cascade:
  type: docs
---

However, this is not the *only* way to traverse a graph. Consider this alternative:

<figure>
  <img
    src="/bfs-example.webp"
    alt="Breadth-first search animation"
    style="display:block; width:75%; height:auto; margin:0 auto;"
  >
</figure>

Here, we can see that nodes are visited in order of their distance from the root node. In fact, this is a very important property that makes breadth-first search (and variants) very useful.

For now, let's think about how we'd implement this. The whole idea is that orange nodes here indicate nodes that have been added to a **queue**. Green nodes are nodes that have been visited. Note that there is no concept of backtracking here, since breadth-first search is not recursive.

```cpp
queue<int> q;
q.push(start);
while (!q.empty()) {
  int u = q.front();
  q.pop();
  // skip already visited nodes
  if (vis[u]) {
    continue;
  }
  vis[u] = true;

  // now enqueue each neighbour of u
  for (int &i : adj[u]) {
    q.push(i);
  }
}
```


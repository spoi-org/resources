---
date: '2026-05-25T21:47:38Z'
draft: false
title: 'Introduction II'
cascade:
  type: docs
---

You may have noticed that, in the figure shown earlier, the nodes were connected by arrows pointing from one node to another. However, sometimes, arrows may be omitted. 

## Directed and undirected graphs

- When a graph is drawn with explicit arrows from one node to another, it is **directed**.
- If the arrows are omitted (and instead there are only lines connecting two nodes), the graph is **undirected**.

Here is how the same graph from earlier would look like if it was undirected.

<figure>
  <img src="/graph-undirected.svg"
       alt="Example graph"
       style="display:block; width:100%; height:auto; margin:0 auto;">

  <figcaption>
    Again, thanks to 
    <a href="https://anacc22.github.io/another_graph_editor/">this website</a>.
  </figcaption>
</figure>

## Common conventions

An arrow or line (formally, an **edge**) between two nodes, in 95% of contexts, means that you can, in some sense, move from one node to the other. If the edge is directed, you can only move in the direction it points. Otherwise, you can move in both directions. It may be helpful to think of nodes as cities and edges as roads connecting them.

## How graphs are represented in code

### Adjacency matrices

There are many ways of representing graphs in code. One possible way is using something known as an adjacency matrix— that is— a two-dimensional boolean array that has its $(i, j)$-th entry as `true` if there is an edge between nodes $i$ and $j$. 

```cpp
vector adj(n + 1, vector<bool>(n + 1)); // +1 because of 1-indexing

// create some edges
adj[1][2] = true; // (1,2)
adj[1][3] = true; // (1,3)
adj[2][1] = true; // (2,1)
```

Keep in mind that this can be used for both directed and undirected graphs, and exact implementation details are upto you. For example, here are some ways of using an adjacency matrix for an undirected graph:

```cpp
// assume that we must create an undirected edge (u, v)

// set both endpoints explicitly
adj[u][v] = adj[v][u] = true;

// or, we could try making a canonical index
adj[min(u, v)][max(u, v)] = true;

// or, we could even use a one-dimensional vector after creating a
// bijective function that maps 2d -> 1d, although this is generally overkill
auto map = [&](int i, int j) { return i * m + j; };
adj_1d[map(min(i, j), max(i, j))] = true;
```

### Adjacency lists

However, adjacency matrices are rarely used in most cases. The most popular method of storing graphs in code is an adjacency list, which essentially, for each node $i$, stores a list of all nodes that have an outgoing edge from $i$.

Here's code that converts an edge list to an adjacency list:

```cpp
// taking in the edge list
int num_edges;
cin >> num_edges;
vector<pair<int, int>> edges;
for (int i = 0, u, v; i < num_edges; ++i) {
  cin >> u >> v;
  edges.push_back({u, v});
}

// adjacency list
vector<vector<int>> adj(n + 1);
for (int i = 0, u, v; i < num_edges; ++i) {
  u = edges[i].first;
  v = edges[i].second;
  adj[u].push_back(v);
}
```

We assume that each node is $\le n$.

Also, note that an edge list is also a valid way of representing a graph in code.

## Graph traversals

To begin with, let's start with a simple(r) problem: given a graph and two nodes $a$ and $b$, find whether there exists a way to get from node $a$ to node $b$ by traversing edges.

We can attempt to solve this by trying every single path recursively:

```cpp
// a is the node we're at right now,
// b is the node we need to reach
bool solve(int a, int b) {
  if (a == b) {
    return true;
  }
  for (int &i : adj[a]) {
    if (solve(i, b)) {
      return true;
    }
  }
  return false;
}
```

However, you should quicky realize that the number of paths on a general graph is... infinite! For example, consider a graph with three nodes and three edges: $[1, 2, 3]$, $[(1,2), (2,1), (2,3)]$. There are an infinite number of paths from $1$ to itself, and while there actually is a path from $1$ to $3$, the above code would never find it.

Understanding this issue and its fix is one of the most important parts of graph theory.

## How do we fix this?

To fix this, we must uncover one fundamental truth about graphs:

> If there is a path between any two nodes $u$ and $v$ in a graph, then there is a path consisting only of unique nodes.

Also, conventionally, a path that does not repeat nodes is known as a **simple path**. Turns out, proving this statement is really easy. Consider any potential path that repeats a node (call this node $a$). Now look at the any two occurences of $a$. Assume your path is:

$$
[u,1,2,4,5,{\color{red}a},9,8,14,{\color{red}a},v]
$$

We can simply remove the nodes between the two $a$'s and one of the $a$'s and we obtain another valid path:


$$
[u,1,2,4,5,{\color{green}a},v]
$$

In essence, the whole proof is just us trimming cycles away. A cycle is a path that starts and ends at the same node.

## Implementing this fix

How do we use this insight to fix our algorithm? Well, we can keep track of the nodes we have already visited, and if we are about to recurse into one of them, we should stop ourselves from doing so, as this will not lead to any new path being discovered that would not already have been discovered by us had we not recursed into the already visited node.

```cpp
vector<bool> vis(n + 1);

bool solve(int a, int b) {
  vis[a] = true;
  if (a == b) {
    return true;
  }
  for (int &i : adj[a]) {
    if (i == b) {
      return true;
    }
    if (vis[i]) {
      continue;
    }
    if (solve(i, b)) {
      return true;
    }
  }
  return false;
}
```
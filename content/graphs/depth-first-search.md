---
date: '2026-05-25T21:47:38Z'
draft: false
title: 'Depth-first search'
weight: 2
cascade:
  type: docs
---

Right now, we figured out how to check whether a path from a node $a$ to another node $b$ exists. However, even though we only care about one specific destination node ($b$), the worst-case complexity of the code we wrote previously is $\mathcal{O}(n + m)$. For example, consider the following graph:

<figure>
  <img src="/graph-line.svg"
       alt="Example graph"
       style="display:block; width:100%; height:auto; margin:0 auto;">

  <figcaption>
    Taken from <a href="https://anacc22.github.io/another_graph_editor/">this website</a>.
  </figcaption>
</figure>

If $a=1$ and $b=8$, we traverse all nodes and all edges before realising that there is a path from $1$ to $8$.

For this reason, we usually solve this problem in two steps:

- Visit every node we can (without stopping!) from node $a$, while continuing to track visited nodes
- Then check if $b$ was visited—that is—whether or not `vis[b]` is true

This has the same worst-case time complexity, but we now know whether or not a path exists from $a$ to every other node in the graph.

Here's how we'd code this:

```cpp
vector<bool> vis;

void explore(int u) {
  if (vis[u]) {
    return;
  }
  vis[u] = true;
  
  for (int &i : adj[u]) {
    explore(i);
  }
}

dfs(a);
// now check the value of vis[b]!
```

As you may have guessed, the `explore()` function we just coded is commonly known as 'depth-first search'. This is because the recursive structure of the function causes the graph to be explored 'depth-first'.

<figure>
  <img
    src="/dfs-example.webp"
    alt="Depth-first search animation"
    style="display:block; width:100%; height:auto; margin:0 auto;"
  >
</figure>


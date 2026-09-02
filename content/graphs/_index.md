---
date: '2026-05-25T21:47:38Z'
draft: false
title: 'Graphs'
cascade:
  type: docs
---

Unless you come from a background in mathematical olympiads, graphs will be an entirely novel concept. While their abstract definition leaves little to appreciate, you will see how they can be used to model tons of different problems.

## What is a graph?

A graph is a collection of nodes and edges. Edges connect (exactly) **two** different nodes. For example, a graph with nodes $[1,2,3,4,5]$ and edges $[(1,2), (1,3), (2,4), (2,5), (4,2)]$ can be represented with a figure like this:

<figure>
  <img src="/graph-example.svg"
       alt="Example graph"
       style="display:block; width:100%; height:auto; margin:0 auto;">

  <figcaption>
    Image generated with the help of
    <a href="https://anacc22.github.io/another_graph_editor/">this website</a>.
  </figcaption>
</figure>

Well, alright, but what would we ever use graphs for? Keep reading, the end goal of this introductory article is to solve a [maze problem](https://cses.fi/problemset/task/1193/)!
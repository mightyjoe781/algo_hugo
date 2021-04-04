+++

title = "4 Properties of DFS Forests"

+++

### Properties of DFS Forests

Traversing the internal nodes of tree in preorder gives the vertices in the order in which DFS visits them.

We can divide edges in 2 types

1.  Edges representing a recursive call (*tree edges*)
2. Edges connecting a vertex with an ancestor in its DFS tree that is not its parent (*back edges*)

So we can now divide tree links in 4 types

We refer to a link from $v\rightarrow w$ in DFS tree that represent tree edge as

- A *tree* link if $w$ is unmarked
- A *parent* link if `st[w]` is $v$

and a link from $v\rightarrow w$ that represent a back edge as

- A *back* link if `ord[w] < ord[v]`
- A *down* link if `ord[w] > ord[v]`

So each tree edge in the graph corresponds to a tree/parent link in DFS tree, and each back edge in the graph corresponds to back/down link in DFS tree.


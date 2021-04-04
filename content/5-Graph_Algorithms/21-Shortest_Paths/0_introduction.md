+++

title = "0-introduction"

+++

### Shortest Path

This chapter deals with problems as *find the lowest-weight path between two given vertices*.

We refer to weighted digraphs as networks.

We use pointers to abstract edges for weighted digraphs to broaden the applicability of our implementation. Since there is only one representation of each edge, we don't need to use `from` function in edge class while using iterator and second thing we sometimes want reverse graph, but we need different approach than that taken by program 19.1 because that implementation creates edges to create the reverse, and we assume a graph ADT whose clients provide pointers to edges should not create edges on its own.

Generally dealing with networks sometimes we want to keep self-loops in all representation. This convention allows algorithms the flexibility to use sentinel max. value weight to indicate a vertex can't be reached from itself.

many application also call for parallel edges, perhaps with differing weights but thankfully we won't consider them for simplicity.

All connectivity properties of digraphs that we considered in chapter 19 are relevant in networks.

**Definition 21.1** *A **shortest path** between two vertices s and t in a network is a directed simple path from s to t with property that no other such path has a lower weight*

Some points to consider are if t is not reachable from s, there is no path at all, and therefore there is no shortest path. There may be multiple paths of the same weight from one vertex to another; we typically want to find one of them.

It is not necessary for all edge on a negative weight cycle to be of negative weights; what counts is the sum of edge weights. we use term *negative cycle* to refer to directed cycles whose total weight is negative.

**Source-sink shortest path** Given a start vertex s and a finish vertex t, find a shortest path in graph from s to t. s- source vertex and t-sink vertex.

**Single-source shortest path** Given a start vertex s, find the shortest path from s to each other vertex in graph.

**All-pairs shortest path** Find the shortest path connecting each pair of vertices in the graph, we sometimes use the term *all shortest * to refer to this set of $V^2$ paths.

Some Applications

**Road Maps** Tables that give distances between all pairs of major cities are a prominent feature of many road maps.

**Airline routes.**


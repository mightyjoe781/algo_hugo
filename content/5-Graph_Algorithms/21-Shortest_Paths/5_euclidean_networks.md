+++

title = "5-euclidean networks"

+++

### Euclidean Networks

In applications where networks model maps, primary interest is often in finding the best route from one place to another. We want to find : a fast algorithm for source-sink path problem in Euclidean networks,which are networks whose vertices are points in plane and whose edge weights are defined by geometric distance between the points.

These networks have 2 important properties

- distances satisfy a triangle inequality
- vertex positions give a lower bound on path length

Often, Euclidean networks are also *symmetric*. Edges run in both directions.

Basic Idea is straightforward : Priority-first search provides a general mechanism to search for paths in graphs. With Dijkstra's algorithm, we examine paths in order of their distances from the start vertex. This ordering endures that, when we reach sink we have examined all paths in the graph that are shorter, none of which took us to sink.

But Euclidean Graphs carry more information that if we look for a path from s to d and see a vertex v, then we know that not only we have to take path along v and take best path from v to d, but also the best we could do in travelling from v to d is first to take an edge v-w and then to find a path whose length is straight-line distance from w to d. With PFS we can easily include this information and improve performance.

We use standard algorithm but use sum of the following three quantities as priority of each edge v-w

- length of the known path from s to v
- weight of edge v-w
- distance from w to t

We do 2 changes

- instead of initializing `wt[s]` at the beginning of search to 0.0, we set it to quantity dist(s,d)
- we define priority P to be function `(wt[v] + e->wt() + distance(w,d)-distance(v,d))` instead of `wt[v]+e->wt()` that we have used in 21.1

These changes are known as Euclidean heuristic.

**Property 21.11** *Priority-first search with the Euclidean heuristic solves the source–sink shortest-paths problem in Euclidean graphs.*

Euclidean heuristics affects performance but not the correctness of Dijkstra's algorithm for source-sink shortest-path computation..


+++

title = "7-mincost flow reductions"

+++

### Mincost-Flow Reductions

Its a general problem solving model that can encompass a variety of useful practical problems.

The mincost-flow problem is more general than maxflow problem, since any mincost-problem solution is acceptable for maxflow problem. Therefore we can reduce all maxflow reductions to mincost-flow reductions.

**Property 22.29** *The single-source-shortest-path problem (in networks with no negative cycles) reduces to mincost-feasible-flow problem*

**Property 22.30** *In mincost-flow problems, we can assume, without the loss of generality, that edges costs are nonnegative.*

if there is a edge with $x<0$ then we just replace cost with $-x$.

For negative-cost edges, if solution to the mincost-flow problem for the transformed network puts flow `f` in the edge `v-u` we put flow `c-f` in `u-v` in original network; for positive-cost edges, the transformed network has the same flow as in the original. This flow assignment preserves the supply/demand constraint at all the vertices.

**Transportation** Solve the mincost-flow problem for a bipartite distribution network where all edges are directed from a supply vertex to a demand vertex and have unlimited capacity.

**Property 22.31** *The transportation problem is equivalent to the mincost-flow problem.*

Given a standard distribution network with $V$ vertices and $E$ edges, build a transportation network with $V$ supply vertices,  $E$ demand vertices and $2E$ edges, as follows. For each vertex in the original network, include a vertex in the bipartite network with supply or demand value set of the original value plus the sum of capacities of the outgoing edges; and for each edge $u-v$ in the original network with capacity c, including a vertex in the bipartite network with supply or demand value -c (we use notation [u-v] to refer to this vertex). For each edge u-v in the original network, include two edges in the partite network; one from u to [u-v] with the same cost, and edges in the bipartite network : one from u to [u-v] with the same cost, and one from v to [u-v] with cost 0.

This following one-to one correspondence preserves costs between flows in the two networks : An edge u-v has flow value f in the original network if and only if edge u-[u-v] has flow value f, and edge v-[u-v] has flow value c-f in the bipartite network (those two flows must sum to c because of supply-demand constraint at vertex[u-v].)

Thus any mincost flow in one network corresponds to mincost-flow in the other.

**Assignment** Given a weighted bipartite graph, find a set of edges of minimum total weight such that each vertex is connected to exactly one other vertex.

**Property 22.32** The assignment problem reduces to the mincost-flow problem.

**Mail carrier** Given a network (weighted digraph), find a cyclic path of minimal weight that includes each edge at least once (see Figure 22.52). Recall that our basic definitions in Chapter 17 make the distinction between cyclic paths (which may revisit vertices and edges) and cycles (which consist of distinct vertices, except the first and the final, which are the same).
The solution to this problem would describe the best route for a mail carrier (who has to cover all the streets on her route) to follow. A solution to this problem might also describe the route that a snow-plow should take during a snowstorm, and there are many similar applications.

This problem is extension of Euler-tour problem.

**Property 22.33** The mail carrier’s problem reduces to the mincost-flow problem.
+++

title = "7-negative weights"

+++

### Negative Weights

Negative weights are not mere mathematical curiosity; on the contrary, they significantly extend the applicability of shortest-paths problems as a model for solving other problems.

Perhaps most important effect is that when negative weights are present, *low-weight shortest paths tend to have more edges than higher-weight paths.* For positive weights, our emphasis was on looking for *shortcuts*; but when negative weights are present we seek *detours* that take as many edges with negative weights as we can find. With negative weights, we are looking for long paths rather than short paths.

The first idea that suggests itself to remedy the situation is to find the smallest (most negative ) edge weight, then to add absolute value of that number to all edge weights and transform the network into one with no negative weights. This naive approach doesn't work at all, because shortest paths in the new network bear little relation to shortest paths in the old one.

**Shortest paths in network with no negative cycles** Given a network that may have negative edge weights but does not have any negative-weight cycle, solve one of the following problem

- Find shortest path connecting two given vertices (shortest-path problem)
- find shortest path from a given vertex to all the other vertices (single-source problem)
- find shortest paths connecting all pairs of vertices (all-pairs problem)

**Property 21.19** *The job-scheduling-with-deadlines problem reduces to the shortest-paths problem in networks that contain no negative cycles.*

**Negative cycle detection** *Does a given network have a negative cycle ? If it does, find one such cycle.*

on the one hand this problem is not necessarily easy; on the other hand, it is not necessarily difficult. Our first challenge will be to develop an algorithm for this task.

**Arbitrage** many newspapers print tables showing conversion rates among the world's currencies. We can view such tables as adjacency-matrix representation of complete networks. Paths in the network specify multistep conversions.

Can we detect negative cycles in a network or find shortest paths in networks that contain no negative cycles ? The existence of efficient algorithms to solve these problems does not contradict the NP-hardness of the general problem that we proved in Property 21.18 because no reduction from Hamilton-path problem to either problem is known.

Dijkstra's algorithm does not work in presence of negative weights, even when we restrict attention to networks that contain no negative cycles.

**Property 21.20** *Floyd’s algorithm solves the negative-cycle–detection problem and the all-pairs shortest-paths problem in networks that contain no negative cycles, in time proportional to $V^3$.*

Can we do better than $V^3$ ? can we find a solution that is as efficient as Dijkstra's ?

Approach developed by R. Bellman and L. Ford in the late 1950s, provides a simple and effective basis for attacking single-source shortest paths problem in network that contains no negative cycles.

To compute shortest paths from a vertex s, we maintain (as usual) a vertex-indexed vector `wt` such that `wt[t]` contains the shortest path length from s to t. We initialize wt[s] to 0 and all other `wt` entries as large sentinel values, then compute the shortest path as follows :

*Considering the network's edges in any order, relax along each edge. Make V such passes.*

Graph represented with adjacency lists, we could implement Bellman-Ford algorithm to find the shortest paths from a start vertex s by initializing the `wt` entries to a value larger than any path length and the `spt` entries to null ptrs, then proceed as follows:

````c++
wt[s] = 0;
for (i = 0; i < G->V(); i++)
    for (v = 0; v < G->V(); v++)
        {
        if (v != s && spt[v] == 0) continue;
        typename Graph::adjIterator A(G, v);
        for (Edge* e = A.beg(); !A.end(); e = A.nxt())
            if (wt[e->w()] > wt[v] + e->wt())
        		{ wt[e->w()] = wt[v] + e->wt(); st[e->w()] = e; }
	}
````

**Property 21.21** *With the Bellman–Ford algorithm, we can solve the single source shortest-paths problem in networks that contain no negative cycles in time proportional to V E.*

**Program 21.9 Bellman-Ford algorithm**

````c++
SPT(Graph &G,int s) : G(G), spt(G.V()), wt(G.V(),G.V())
{
    queue<int> Q; int N = 0;
    wt[s] = 0.0;
    Q.put(s); Q.put(G.V());
    while(!Q.empty())
    {
        int v;
        while((v==Q.get()) == G.V())
        { if(N++ > G.V()) return; Q.put(G.V());}
        typename Graph::adjIterator A(G,v);
        for(Edge* e = A.beg(); !A.end(); e = A.nxt()){
            int w = e->w();
            double P = wt[v] + e->wt();
            if(P<wt[w])
            {wt[w] = P; Q.put(w); spt[w] = e;}
        }
    }
}
````

For dense graphs this method is no good than Floyd's algorithm, which finds all shortest paths, rather than just those from a single source.

Some variations of Bellman-ford are faster than the FIFO-queue version but all takes the same time at least $VE$ in worst case.

**Property 21.22** *With the Bellman–Ford algorithm, we can solve the negative-cycle–detection problem in time proportional to $V E$.*

Now lets move to solving the all-pair shortest-paths problem.

Transforming the network into a network that has only non negative weights and that has the same shortest-paths structure.

Given we have array `wt` containing weights of all edges of a graph. Now we define a operation *reweighting* the graph as follows :

- To rewrite an edge, add to that edge's weight the difference between the weight of the edge's source and destination
- To rewrite a network, reweight all of that network's edges.

Code for reweighting

````c++
for (v = 0; v < G->V(); v++)
{	typename Graph::adjIterator A(G, v);
    for (Edge* e = A.beg(); !A.end(); e = A.nxt())
    	e->wt() = e->wt() + wt[v] - wt[e->w()]
}
````

 **Property 21.23** *Reweighting a network does not affect its shortest paths.*

Reweighting is no help in network with negative cycles : The operation doesn't change the weight of any cycle, so we cannot remove negative cycles by reweighting.

With non-negative edge weights, we can then solve it using Dijkstra's algorithm.

**Property 21.24** In any network with no negative cycles, pick any vertex s and assign to each vertex v a weight equal to the length of a shortest path to v from s . Reweighting the network with these vertex weights yields nonnegative edge weights for each edge that connects vertices reachable from s.

In summary, we can solve the all-pairs shortest-paths problem in networks that  contain negative edge weights but no negative cycles by proceeding as follows:

- Apply the Bellman-Ford algorithm to find a shortest-paths forest in the original network.
- If the algorithm detects a negative cycle, report that fact and terminate  Reweight the network from the forest.
- Apply the all-pairs version of Dijkstra’s algorithm to the reweighted network.

After this computation the paths matrix gives shortest paths in both network, and distances matrix give the path length in reweighted network.

**Property 21.25** With Johnson’s algorithm, we can solve the all-pairs shortest-paths problem in networks that contain no negative cycles in time proportional to $VE\log_d V $, where d  = 2 if E<2V, and d = E/V otherwise.


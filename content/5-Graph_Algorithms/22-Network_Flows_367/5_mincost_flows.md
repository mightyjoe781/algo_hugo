+++

title = "5-mincost flows"

+++

### Mincost flows

it is not unusual to have numerous solution for maxflow problem so we would like to impose a condition on them for choosing one of them. We could prefer the one that uses fewest edges or shortest paths, or there existing a comprising disjoint paths. Such general problems fall into the general model known as *mincost flow problem*.

**Definition 22.8** The flow cost of an edge in a flow network with edge costs is the product of that edge’s flow and cost. The cost of a flow is the sum of the flow costs of that flow’s edges.

continue to assume that capacities are positive integers less than M. We also assume edge costs to non-negative integer less than C.

**Mincost Maxflow** Given a flow network with edge costs, find a maxflow such that no other maxflow has lower cost.

**Mincost Feasible flow** recall that we define a flow in a network with vertex weights to be *feasible* if the sum of the vertex weights is negative and the difference between each vertex's outflow and inflow. Such a network, find a feasible flow of minimal cost.

The describe the network model for the mincost-feasible-flow problem, we use the term *distribution network* for brevity to mean "capacitated flow network with edge costs and supply or demand weights on vertices"

**Program 22.8 Computing flow cost**

````c++
static int cost(Graph &G)
{ 	int x = 0;
    for (int v = 0; v < G.V(); v++)
    {
        typename Graph::adjIterator A(G, v);
        for (Edge* e = A.beg(); !A.end(); e = A.nxt())
            if (e->from(v) && e->costRto(e->w()) < C)
            	x += e->flow()*e->costRto(e->w());
    }
    return x;
}
````

**Property 22.22** The mincost–feasible-flow problem and the min-cost maxflow problems are equivalent.

To include the cost in flow network we add and integer `pcost` to edge class and member function `cost()` that return cost to clients.

**Definition 22.9** Given a flow in a flow network with edge costs, the **residual network** for the flow has the same vertices as the original and one or two edges in the residual network for each edge in the original, defined as follows: For each edge u-v in the original, let f be the flow, c the capacity, and x the cost. If f is positive, include an edge v-u in the residual with capacity f and cost-x; if f is less than c , include an edge u-v in the residual with capacity c-f and cost x.

Edges in the residual network that represent backward edges have *negative* cost.

we use following function to do this

```c++
int costRto(int v)
	{return from(v) ? -pcost : pcost; }
```

*Because of the negative edge costs, these networks can have negative-cost cycles*

**Property 22.23** *A maxflow is a **mincost maxflow** if and only if its residual network contains no negative-cost (directed) cycle.*

This property immediately to a simple generic algorithm for solving the mincost-flow problem, called the cycle-canceling algorithm

*Find a maxflow. Augment the flow along any negative-cost cycle in the residual network, continuing until none remain.*

Since we have developed algorithm for computing a maxflow and for finding negative cycles, we immediately have the implementation of the cycle-canceling algorithm. We use any maxflow implementation to find the initial maxflow and the *Bellman-Ford Algorithm* to find negative cycles. To these two implementation we just add a loop to augment flow along the cycles.

As for the maxflows, the existence of this generic algorithm guarantees that every mincost-flow problem has a solution where flows are all integers ; and the algorithm computes such a solution.

**Property 22.24** The number of augmenting cycles needed in the generic cycle-canceling algorithm is less than ECM.

**Corollary** The time required to solve the mincost-flow problem in a sparse network is $O(V^3CM)$.

**Program 22.9 Cycle Canceling**

````c++
template <class Graph, class Edge> class
MINCOST
{ 	const Graph &G;
    int s, t;
    vector<int> wt;
    vector <Edge *> st;
    int ST(int v) const;
    void augment(int, int);
    int negcyc(int);
    int negcyc();
    public:
        MINCOST(const Graph &G, int s, int t) : G(G),
            s(s), t(t), st(G.V()), wt(G.V())
            { 	MAXFLOW<Graph, Edge>(G, s, t);
            	for (int x = negcyc(); x != -1; x = negcyc())
            		{ augment(x, x); }
        }
};
````

It is possible to develop a strategy that finds negative-cost cycles and ensure that the number of negative-cost cycles used in less than $VE$. This result is significant because it establishes the fact that the mincost-flow problem is tractable.

The mincost-flow problem represent the most general problem-solving model that we have yet examined, so it is perhaps surprising that we can solve it with such a simple implementation.
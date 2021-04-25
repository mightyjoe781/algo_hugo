+++

title = "1-flow networks"

+++

### Flow Networks

Lets consider a idealized physical model : Imagine a interconnected oil pipes of varying sizes, with flow direction controlling switches. Consider a single source and sink. At each vertex there is equilibrium i.e. inflow = outflow and pipe capacity is considered in same units.

If there is a equilibrium at switch we don't need to worry about anything just fill pipes to full capacity. Otherwise, not all pipes are full but oil flows through the network, controlled by switch setting at the junctions such that amount of inflow and outflow at vertices is equal. This equilibrium at junction implies an equilibrium in the network as a whole;

Given these facts, we want answer to answer to that problem : What switch setting will maximize the amount of oil flowing from source to sink ?

We can model this situation as network that has single source and sink. The edges is oil pipes, vertices : junctions with switch, edge weight : pipe capacity;

We assume edges are directed corresponding to a flow direction. Each pipe has a certain flow which is less than or equal to capacity.

This flow-network abstraction is a useful problem-solving model that applies directly to a variety of application and indirectly to still more.

**Definition 22.1** *We refer to a network with a designated source s and a designated sink t as an **st-network***

**Definition 22.2** *A **flow network** is an st-network with positive edge weights, which we refer to as **capacities**. A **flow** in a flow network is a set of nonnegative edge weights—which we refer to as **edge flows** —satisfying the conditions that no edge’s flow is greater than that edge’s capacity and that the total flow into each internal vertex is equal to the total flow out of that vertex.*

**Maximum Flow** Given a *st*-network, find a flow such that no other flow from s to t has larger value. For brevity we refer to such a flow as a *maxflow* and the problem of finding one in a network as the *maxflow problem* value, but we generally want to know a flow (edge flow values) that achieves that value.

Ca we allow multiple source and sinks ? should we able to handle networks with no sources or sinks ? Can we allow flow in either direction in edges ? Can we have restriction for the vertices instead of edges ?

**Property 22.1** *Any st-flow has the property that outflow from s is equal to the inflow to t.*

This property is true for any single vertex, by local equilibrium.

**Corollary** The value of the flow for the union of two sets of vertices is equal to the sum of the values of the flows for the two sets minus the sum of the weights of the edges that connect a vertex in one to a vertex in the other.

We can dispense the dummy vertices, augment any flow network with an edge from t to s with flow and capacity equal to the network's value, and know that inflow is equal to outflow for any set of nodes in augmented network. Such a flow is called a circulation, and this construction demonstrates that the maxflow problem reduces to finding a circulation that maximize the flow along a given edge.

**Property 22.2 (Flow decomposition theorem)** *Any circulation can be represented as flow along a set of at most E directed cycles.*

**Corollary** Any st-network has a maxflow such that the subgraph induced by nonzero flow values is acyclic.

**Corollary** Any st-network has a maxflow that can be represented as flow along a set of at most E directed paths from s to t.

To implement maxflow algorithms, we use GRAPH class of Chapter 20 but with pointers to a more sophisticated EDGE class. Instead of single weight that we used, we use `pcap` and `pflow` private data members. Even though this network are directed graphs, our algorithms need to traverse edges in both direction, so we use the *undirected* graph representation from Chap 20 and member function from to distinguish u-v from v-u.

Since flow networks are typically sparse, we use an adjacency-list based GRAPH representation like the SparseMultiGRAPH implementation. We also limit the flow to m bit integer less than 32 cause the float values comparisons make our algorithms heavy.

We sometimes refer to edges as having infinite capacity, or, equivalently, as being *uncapacitated*.

Given below is a client that checks whether a flow satisfies the equilibrium condition at every node and return that flow's value if the flow does.

**Program 22.1 Flow check and value computation**

````c++
template <class Graph, class Edge> class check
{
  public:
    static int flow(Graph &G, int v)
    {
        int x = 0;
        typename Graph::adjIterator A(G,v);
        for(Edge* e = A.beg(); !A.end(); e = A.nxt())
            x += e->from(v) ? e->flow():-e->flow();
        return x;
    }
    static bool flow(Graph &G, int s, int t)
    {
        for(int v = 0; v < G.V(); v++)
            if((v!=s) && (v!=t))
                if(flow(G,v) != 0) return false;
        int sflow = sflow(G,s);
        if(sflow < 0) return false;
        if(sflow + flow(G,t)!=0) return false;
        return true;
    }
};
````


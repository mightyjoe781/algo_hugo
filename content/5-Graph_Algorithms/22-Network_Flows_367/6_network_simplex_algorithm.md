+++

title = "6-network simplex algorithm"

+++

### Network Simplex Algorithm

The run time of cycle-canceling algorithm is based on not just the number of negative-cost cycles that the algorithm uses to reduce the flow cost but also the time that the algorithms uses to find each of the cycles. We can reduce runtime of both operation significantly with method know as *network simplex* algorithms. It is based on maintaining a tree data restructure and reweighting costs such that negative cycles can be identified quickly.

each network edge u-v is in one of three states

- Empty, so flow can be pushed from only u to v
- Full, so flow can be pushed from only v to u
- Partial (neither empty not full), so flow can pushed either way

**Definition 22.10** *Given a maxflow with no cycles of partial edges, a feasible spanning tree for the maxflow is any spanning tree of the network that contains all the flow's partial edges.*

First step in network simplex algorithm is to build a spanning tree. one way to build is to compute maxflow, cycles of partial edges by augmenting along each cycle to fill/empty one its edges, then to add full/empty edges to remaining partial edges to make a spanning tree.

**Definition 22.11** Given a flow in a flow network with edge costs, let c (u, v)denote the cost of u-v in the flow’s residual network. For any potential function ϕ, the reduced cost of an edge u-v in the residual network with respect to ϕ, which we denote by c (u, v), is defined to be the value c(u, v)− (ϕ(u)− ϕ(v)).

**Property 22.25** We say that a set of vertex potentials is valid with respect to a spanning tree if all tree edges have zero reduced cost. All valid vertex potentials for any given spanning tree imply the same reduced costs for each network edge.

**Property 22.26** We say that a nontree edge is eligible if the cycle that it creates with tree edges is a negative-cost cycle in the residual network. An edge is eligible if and only if it is a full edge with positive reduced cost or an empty edge with negative reduced cost.

**Property 22.27** If we have a flow and a feasible spanning tree with no eligible edges, the flow is a mincost flow.

Or we can say if we have a flow and a set of vertex potentials such that reduced costs of tree edges are all zero, full nontree edges are all nonnegative, and empty nontree edges are all nonpositive, then the flow is a mincost flow.

feasible spanning trees give us vertex potentials, which give us reduced costs, which give us eligible edges, which give us negative-cost cycles. Augmenting along a negative-cost cycle reduces the flow cost and also implies changes in the tree structure. Changes in tree structure imply changes in vertex potentials; and changes in reduced cost imply changes in set of eligible edges. After making all these changes, we can pick another eligible edge and start process again. This generic implementation of cycle-canceling algorithm for solving the mincost-flow problem is called *network simplex* algorithm

*Build a feasible spanning tree and maintain vertex potentials such that all tree vertices have zero reduced cost. Add an eligible edge to the tree, augment the flow along the cycle that it makes with tree edges, and remove from the tree an edge that is filled or emptied, continuing until no eligible edges remain.*

**Property 22.28** If the generic network simplex algorithm terminates, it computes a mincost flow.

First choice that we face in developing an implementation of the network simplex algorithm is what representation to use for the spanning tree. We have three primary computation task that involve the tree

- Computing the vertex potential
- Augmenting along the cycle (and identifying an empty or a full edge on it)
- Inserting a new edge and removing an edge on the cycle formed Each of these tasks is an interesting exercise in data structure and algorithm design.
  - There are several DS and numerous algorithm that we might consider, with varied performance characteristics.

**Program 22.10 Vertex potential calculation**

````c++
int phiR(int v)
{
    if(mark[v] == valid) return phi[v];
    phi[v] = phiR(ST(v)) - st[v]->costRto(v);
    mark[v] = valid;
    return phi[v];
}
````

Given two nodes in a tree, their *least common ancestor* (LCA) is the root of the smallest subtree that contains them both.

The cycle that we form by adding edge connecting two nodes consists of that edge plus the edge on the two paths from the two nodes to their LCA. The augmenting cycle formed by adding v-w goes through v-w to w, up the tree the LCA of v and w (say r), then down the tree to v, so we have to consider edges in opposite orientation in the two paths.

As before, we augment along the cycle traversing the paths once to find the max amount of flow that we can push through their edges and then traversing the two paths again, pushing flow through them.

**Program 22.11 Augmenting along a cycle**

````c++
int lca(int v, int w)
{
    mark[v] = ++valid; mark[w] = valid;
    while(v!=w)
    {
        if(v!=t) v = ST(v);
        if(v!=t && mark[v] == valid) return v;
        mark[v] = valid;
        if(w!= t) w = ST(w);
        if(w!=t && mark[w] == valid) return w;
        mark[w] = valid;
    }
    return v;
}
Edge *augment(Edge *x){
    int v = x->v(), w = x->w(); int r = lca(v,w);
    int d = x->capRto(w);
    for(int u = w; u!= r; u = ST(u))
        if(st[u]->capRto(ST(u)))
            d = st[u]->capRto(ST(u));
    for(int u = v; u!=r; u = ST(u))
        if(st[u]->capRto(u)<d)
            d = st[u]->capRto(u);
    r->addflowRto(w,d); Edge* e = x;
    for(int u = w; u!= x; u = ST(w))
    {
        st[u]->addflowRto(ST(u),d);
        if(st[u]->capRto(ST(u)) == 0) e= st[u];
    }
    for(int u = w; u!= x; u = ST(w))
    {
        st[u]->addflowRto(u,d);
        if(st[u]->capRto(ST(u)) == 0) e= st[u];
    }
    return e;
}
````

We use a trick to avoid paying the cost of initializing all the marks each time that we call it. We maintain the marks as global variables, initialized to zero.

Our third tree-manipulation task is to substitute and edge u-v for another edge in the cycle that it creates with tree edges. Again the LCA of u and v is important, because the edge to be removed either on the path from u to LCA or on the path from v to LCA.

**Program 22.12 Spanning tree substitution**

````c++
bool onpath(int a, int b, int c)
{
     for(int i = a; i != c; i = ST(i))
         if(i==b) return true;
    return false;
}
void reverse(int u, int x)
{
    Edge *e = st[u];
    for(int i = ST(u); i != x; i = ST(i))
    { Edge *y = st[i]; st[i] = e; e = y; }
}
void update(Edge *w, Edge *y)
{
    int u = y->w(), v = y->v(), x = w->w();
    if(st[x] != w) x = w->v();
    int r = lca(u,v);
    if(onpath(u,x,r))
    { reverse(u,x); st[u] = y; return; }
    if(onpath(v,x,r))
    { reverse(v,x); st[v] = y; return; }
}
````



**Program 22.13 Eligible Edge Search**

````c++
int costR(Edge *e, int v)
{ 	int R = e->const() + phi[e->w()] - phi[e->v()];
	return e->from(v) ? R : -R; }
Edge *besteligible(){
    Edge *x = 0;
    for(int v = 0, min = C*G.V(); v < G.V(); v++)
    {
        typename Graph::adjIterator A.(G,v);
        for(Edge* e = A.beg(); !A.end(); e = A.nxt())
            if(e->capRto(e->other(v))>0)
                if(e->capRto(v) == 0)
                    if(costR(e,v) < min)
                    { x= e; min = costR(e,v);}
    }
    return x;
}
````



**Program 22.14 Network Simplex (Basic Implementation)**

````c++
template <class Graph, class Edge> class MINCOST
{
  const Graph &G; int s,t; int valid;
    vector<Edge *> st; vector<int> mark, phi;
    void dfsR(Edge);
    int ST(int);
    int phiR(int);
    int lca(int,int); Edge *augment(Edge *);
    bool onpath(int,int,int);
    bool reverse(int,int);
    void update(Edge *, Edge *);
    int costR(Edge *,int); Edge *besteligible();
public:
    MINCOST(Graph &G,int s,int t): G(G),s(s),t(t), st(G.V()),
    mark(G.V()-1), phi(G.V()){
        Edge *x = new Edge(s,t,M+G.V(),C+G.V());
        G.insert(x);
        x->addflowto(t,x->cap());
        dfsR(x);
        for(valid = 1; ; valid++)
        {
            phi[t] = x->costRto(s); mark[t] = valid;
            for(int v = 0 ; v<G.V(); v++)
                if(v!=t) phi[v] = phiR(v);
            Edge *x = besteligible();
            if(costR(x,x->v()) == 0) break;
            update(augment(x),x);
        }
        G.remove(x); delete x;
    }
};
````



**Program 22.15 Network simplex (improved implementation)**

````c++
int old = 0;
for (valid = 1; valid != old; )
    {
        old = valid;
        for (int v = 0; v < G.V(); v++)
        {
            typename Graph::adjIterator A(G, v);
            for (Edge* e = A.beg(); !A.end(); e = A.nxt())
                if (e->capRto(e->other(v)) > 0)
                    if (e->capRto(v) == 0)
                        { update(augment(e), e); valid++; }
        }
    }
````

One critical fact that is to be noted a algorithm might not even terminate, because full/empty edges on spanning tree can stop us from pushing flow along the negative cycle that we identify. That is, we can identify an eligible edge and negative cycle that it makes with spanning tree edges, but the maximum amount of flow that we can push along the cycle may be 0. In this case, we still substitute the eligible edge for an edge on the use.

We initialize the spanning tree with sink at root and source as its only child, and a search tree of graph induced by remaining nodes in the residual network. The implementation uses the parent-edge representation of tree in the array st and related parent-vertex function ST.

If there is more than one full/empty edge on augmenting cycle, the substitution algorithm in Program 22.12 always deleted from the tree the one closest to LCA of eligible edge's two vertices. Fortunately, it has been shown that this particular strategy for choosing the edge to remove from the cycle suffices to ensure that the algorithm terminates.

One final question remains in implementation design that is should we use a DS to hold eligible edges and if yes then which is more appropriate DS. The answer in dependent on dynamics of solving particular instances of the problem. If total number of eligible edges is small then its worth maintaining separate DS; if most edges are eligible most of the time, it is not.


+++

title = "3-Reachability and Transitive Closure"
weight = 3

+++

### Reachability and Transitive Closure

**Definition 19.5** *The **transitive closure** of a digraph is a digraph with same vertices but with an edge from s to t in the transitive closure if and only if there is a directed path from s to t in the given digraph.*

![image-20210114114811758](/3_Reachability_and_Transitive_Closure.assets/image-20210114114811758.png)

transitive enclosure (bottom) of a digraph (up).

**Boolean Matrix Multiplication**

A Boolean matrix is a matrix whose entries are all binary values either 0 or 1. Given two Boolean Matrices A and B, compute their product matrix C, using the logical `and` and `or` operations instead of arithmetic operation * and +, respectively.

Suppose A is adjacency matrix of a digraph A and we use the preceding code to compute $C = A * A^2$. for each pair of vertices s and t, we put an edge from s to t in C if and only if there is some vertex i for which there is both a path from s to i and a path from i to t in A.

In other words path in $A^2$ corresponds precisely to directed paths of length 2 in A. If we have self loops in A then $A^2$ also has edges of A; otherwise it doesn't.

![image-20210114115403793](/3_Reachability_and_Transitive_Closure.assets/image-20210114115403793.png)

**Property 19.6** We can compute the transitive closure of a digraph by constructing the latter's adjacency matrix $A$, adding self-loops for every vertex, and computing $A^V$

We can build up transitive closure in place matrix using this method.

````c++
for(i = 0; i< V; i++)
    for( s = 0 ; s < V; s++)
        for( t = 0; t <V; t++)
            if(A[s][i] && A[i][t]) A[s][t] = 1;
````

This classical method is invented by S. Warshall in 1962, is method of choice for computing the transitive closure of dense digraphs.

Code might look similar to boolean matrix product but difference lies in order of the for loops.

**Property 19.7** : *With Warshall's algorithm, we can compute the transitive closure of a digraph in time proportional to $V ^ 3$*

We can improve performance of Warshall's algorithm with a simple transformation of the code : We move the test of `A[s][i]` out of the inner loop because its value doesn't change as t varies. This allows us to avoid execution when `A[s][i]` is zero.

**Program 19.3 Warshall's algorithm**

````c++
template class tcGraph, class Graph> class TC
{
    tcGraph T;
    public:
    TC(const Graph &G) : T(G)
    {
        for(int s = 0; s<T.V(); s++)
            T.insert(Edge(s,s));
        for(int i = 0; i<T.V(); i++)
            for(int s = 0; s<T.V(); s++)
                if(T.edge(s,i))
                    for(int t = 0; t < T.V(); t++)
                        if(T.edge(i,t))
                            T.insert(Edge(s,t))
    }
    bool reachable(int s, int t) const
    {return T.edge(s,t);}
};
````

We can use term *abstract transitive closure* to refer to an ADT that provides clients with the ability to test reachability after preprocessing a digraph.

**Property 19.8** : *We can support constant-time reachability testing (abstract transitive closure) for a digraph, using space proportional $V^2 $ and time proportional to $V^3 $ for preprocessing*.

Finally we will consider two very important problems that will imply that we need more faster methods.

First, we consider relationship between the transitive closure and the *all-pairs shortest paths* problem. For di-graphs, the problem is to find, for each pair of vertices, a directed path with minimal number of edges.

given a digraph, we initialize a $V\times V$ Matrix $A$ by setting `A[s][t]` to 1. if there is an edge from `s` to `t` and to the sentinel value $V$ if there is no such edge. Our goal is to set `A[s][t]` equal to length of a shortest path from s to t, using a sentinel value $V$ to indicate that there is no such path.

````c++
for( i = 0; i < V; i++)
    for( s = 0; s < V; s++)
        for( t = 0; t < V; t++)
            if(A[s][i] + A[i][t] < A[s][t])
                A[s][t] = A[s][i] + A[i][t];
````

This method is a special case of *Floyd's Algorithm* for finding shortest paths in weighted graphs.



Second, the transitive-closure problem is also closely related to the Boolean matrix-multiplication problem. The basic algorithms that we have seen for both problem requires time proportional to $V^3$.

**Property 19.9** *We can use any transitive-closure algorithm to compute the product of two Boolean matrices with at most a constant-factor difference in running time.*

Boolean Matrix multiplication is difficult problem and people have been working on it for decades. Best know is runtime of $V^2$. If we solve it in linear time then we have solution to transitive closure problem. This is known as `reduction`.

**Property 19.10** *With DFS, we can support constant query time for the abstract transitive closure of a digraph, with space proportional to $V^2$ and time proportional to $V(E+V)$ for preprocessing (computing the transitive closure).*

**Program 19.4 DFS-based transitive closure**

````c++
template <class Graph> class tc
{
    Graph T; const Graph &G;
    void tcR(int v, int w)
    {
        T.insert(Edge(v,w));
        typename Graph::adjIterator A(G,w);
        for(int t = A.beg(); t!= A.end(); t = A.nxt())
            if(!T.edge(v,t)) tcR(v, t);
    }
    public:
    	tc(const Graph &G) : G(G), T(G.V(),true)
        { for(int v = 0 ; v<G.V(); v++) tcR(v,v);}
    bool reachable(int v, int w)
    { return T.edge(v,w);}
}
````

For sparse digraph this search based approach is method of choice.
+++

title = "7-Reachability in DAGs"
weight = 7
+++

### Reachability in DAGs

Any method for topological sorting can serve as the basis for a transitive-closure algorithm for DAGs, as follows : We proceed through the vertices in reverse topological order, computing the reachability vector for each vertex from the rows corresponding to its adjacent vertices. The reverse topological sort ensures that all those rows have already been computed. In total, we check each of V entries in the vector corresponding to destination vertex of each of the E edges, for a total running time proportional to $VE$. Although its simple to implement but no more efficient for DAGs than for general graph.

**Property 19.13** *With dynamic programming and DFS, we can support constant query time for the abstract transitive closure of a DAG with space proportional to $V^2$ and time proportional to $V^2 + VX$ for preprocessing, X is the number of cross edges in the DFS forest*.

**Program 19.9 Transitive closure of a DAG**

````c++
template <class tcDag, class Dag> class dagtC
{
    tcDag T; const Dag &D;
    int cnt;
    vector<int> pre;
    void tcR(int w)
    {
        pre[w] = cnt++;
        typename Dag::adjIterator A(D,w);
        for(int t = A.beg(); t!= A.end(); t = A.nxt())
        {
            T.insert(Edge(w,t));
            if(pre[t] > pre[w]) continue;
            if(pre[t] == -1) tcR(t);
            for(int i = 0; i<T.V(); i++)
                if(T.edge(t,i)) T.insert(Edge(w,i));
        }
    }
    public :
    	dagTC(const Dag &D) : D(D), cnt(0),
    		pre(D.V(), -1) T(D.V(), true)
            {for(int v = 0; v<D.V(); v++)
             if(pre[v] == -1) tcR(v);}
    	bool reachable(int v,int w) const
        { return T.edge(v,w);}
};
````

If DAG has no down edges run time is not better than transitive-closure algorithm we done for general digraphs.

This problem of finding an optimal algorithm for computing the transitive closure of dense DAGs is still unsolved. The best know worst-case performance bound is $VE$.




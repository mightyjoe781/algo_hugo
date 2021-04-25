+++

title = "2-Depth first search"

+++

### Depth-First Search

To visit a vertex, we mark it as having been visited, then recursively visit all the vertices that are adjacent to it and that have not yet been marked.

`ord` vector stores the order of traversal of vertices.

**Program 18.1	Depth-first search of a connected component**

````c++
#include <vector>
template <class Graph> class cDFS
{
    int cnt;
    const Graph &G;
    vector <int> ord;
    void searchC(int v)
    {
        ord[v] = cnt++;
        typename Graph::adjIterator A(G,v);
        for(int t = A.beg(); !A.end(); t = A.nxt())
            if(ord[t] == -1) searchC(t);
    }
    public:
    	cDFS(const Graph &G, int v = 0):
    		G(G),cnt(0), ord(G.V(),-1)
            {searchC(v);}
    	int count() const {return cnt;}
    	int operator[](int v) const {return ord[v];}
};
````

![image-20210110085932147](/2_Depth_first_search.assets/image-20210110085932147.png)

DFS trace

![image-20210110085954775](/2_Depth_first_search.assets/image-20210110085954775.png)

Existence of parallel edges is inconsequential for DFS cause edges are marked visited once. Search Dynamics can vary quite differently according to graph representation we apply DFS.


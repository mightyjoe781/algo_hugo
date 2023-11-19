+++

Title = "MST"

weight = 2

+++

Given a connected, undirected, and weighted graph G, select a subset $E' \in G $ such that graph G is (still) connected and weight of selected edge E' is minimal !!

To satisfy connectivity criteria

- we need at least V-1 edges that form a tree and this tree must span all $V \in G$.

- MST can be solved with several well known algorithms
  - Prim's
  - Krushkal's

#### Kruskal's Algorithm

This algorithm first sorts E edges based on non-decreasing weight. Then greedily try to add each edge into MST as long as such addition doesn't form a cycle. This check can be done using lightweight Union-Find Disjoint Sets implementation.

- Runtime O(sorting + trying to add each edgy x Cost of Union-Find)
- $O(E \log E + E \times (\approx 1)) = O(E\log E) = O(E \log {V^2} = O(E\log V))$

````c++
// inside int main()
vector<pair <int,ii>> EdgeList;
for(int i = 0; i < E; i++) {
    scanf("%d %d %d", &u, &v, &w);
    EdgeList.push_back({w,{v,w}});}
sort(EdgeList.begin(), EdgeList.end()); // sort by weight

int mst_cost = 0;
UnionFind UF(V);
for(int i = 0; i < E; i++) {
    pair<int , ii> front = EdgeList[i];
    if(!UF.isSameSet(front.second.first, front.second.second)) {
        mst_cost += front.first;
        UF.unionSet(front.second.first,front.second.second);
    } }
printf("MST cost = %d \n", mst_cost);
````

#### Prim's Algorithm

This algorithm takes a starting vertex and flags it as taken and enqueues a pair of information into a priority queue. The weight `w` and the other end point `u` of edge 0->u that is not taken yet.

These pairs are sorted in the priority queue based on increasing weight, and if tie, by increasing vertex number. Then Prim's algorithm greedily selects pair (w,u) in front of priority queue which has the minimum weight w - if the end point of this edge - which is u has not been taken before. ( to prevent cycle).

- O(process each edge once x cost of enqueue/dequeue) = $O(E \log E)$ = $O(E \log V)$

````c++
vi taken;
priority_queue<ii> pq; // default setting for C++ STL PQ is max heap
void process(int vtx) {      // -ve sign used to reverse sort order
    taken[vtx] = 1;
    for(auto v : adj[vtx]) { // note v here is (id,weight)
      if(!taken[v.first])
          pq.push({-v.second,-v.first});
  }  }
````

````c++
// inside int main()
taken.assign(V,0);
process(0);              // no vertex is taken at beginning
mst_cost = 0;
while(!pq.empty()) {
  ii front = pq.top(); pq.pop();
    u = -front.second, w = -front.first; // fix negation
    if(!taken[u])
      mst_cost += w, process(u);// take u, process all edge incident to u
}
printf("MST cost = %d\n",mst_cost);
````

#### Famous Variants of MST Applications

- ##### 'Maximum' Spanning Tree

The solution for this is very simple : Modify Kruskal's algorithm a bit, we no simply sort the edges based on non-increasing weight.

- ##### 'Minimum' Spanning Subgraph

In this variant, we do not start with a clean slate. Some edges in the given graph have already been fixed and must be taken as part of the solution. These default edges may form a non-tree in the first place. Our task is to continue selecting the remaining edges (if necessary) to make the graph connected in the least cost way. The resulting Spanning Subgraph may not be a tree and even if its a tree, it may not be the MST. That's why we put term 'Minimum' in quotes and use the term 'subgraph' rather than `tree`.

After taking into account all the fixed edges and their cost we can continue running Kruskal's algorithm on the remaining free edges until we have a spanning subgraph (or spanning tree).

- ##### Minimum 'Spanning Forest'

In this variant, we want to form a forest of K connected components (k subtrees) in the least cost way where K is given beforehand.

To get the minimum spanning forest is simple. Run Kruskal's algorithm as normal, but as soon as the number of connected components equals to the desired pre-determined number K, we can terminate the algorithm.

- ##### Second Best Spanning Tree

A solution for this variant is a modified Kruskal's: sort the edges in $O(E\log E) = O(E\log V)$ then find MST using Kruskal's in $O(E)$. Next for each edge in the MST, temporarily flag it so that it can't be chosen, then try to find MST again in $O(E)$ but now excluding that flagged edge. Note that we do not have to resort the edges at this point. The best spanning tree found after this process is the second best ST.

O(sort the edges once + find the original MST + find the second best ST) = $O(E\log V + E + VE) = O(EV)$

- ##### Minimax (and Maximin)

The minimax path problem is a problem of finding he minimum of maximum edge weight among all possible paths between two vertices i to j. The cost of a path from i to j is determined by maximum edge weight along this path. Among all these possible paths from i to j, pick the one with the minimum max-edge weight.

The problem can be modeled as MST problem. With a rationale that path with low individual edge weights even if the path is longer in terms of number of vertices/edges involved, then having the MST of given weighted graph is correct step. The MST is connected thus ensuring a path between any two vertices. The minimax path solution is thus the max edge weight along the unique path between vertex i and j in this MST.

The overall complexity is O(build MST + one traversal on the resulting tree.)  As E = V-1 in a tree, any traversal is just O(V).

Total Complexity : O(E log V + V) = O(E log V)


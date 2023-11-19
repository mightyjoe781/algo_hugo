+++

Title = "Single-Source Shortest Paths"

weight = 3

+++

## SSSP

Motivating Problem : Given a weighted graph G and a starting source vertex `s`, what are the shortest paths from s to every other vertices of G ?

This problem is well known as SSSP (Single-Source shortest path) problem on a weighted graph. There are efficient algorithm to solve this SSSP problem.

If graph is unweighted (or all edges have equal or constant weight), we can use the efficient $O(V+E)$ BFS algorithm. For general weighted graph, BFS doesn’t work and we should use algorithms like $ O((V+E) \log V)$ Dijkstra’s algorithm or the $O(VE)$ Bellman Ford’s algorithm.

### SSSP on Unweighted Graph

Since graph is unweighted graph, distance between two vertices is 1 unit and BFS explores graph layer by layer.

Some problems may require us to reconstruct the actual shortest path, not just the shortest path length and can be achieved easily by `vi p`.

````c++
void printPath(int u) {       // extract info from `vi p`
  if(u == a) 
  	printf("%d",s); return; // base case, at source s
  printPath(p[u]);
  printf(" %d",u); }
````

````c++
// inside int main()
vi dist(V, INF); dist[s] = 0; // dist s to s is zero
queue<int> q; q.push(s);
vi p;
while(!q.empty()) {
  int u = q.front(); q.pop();
  for(auto v : adj[u]) {		 // integer pair
    if(dist[v] == INF) {
      dist[v] = dist[u] + 1;
      p[v] = u;
      q.push(v);
    } } }
printPath(t), printf("\n"); // prints path from vertex p
````

### SSSP on Weighted Graph

For this problem BFS won’t work, we use *greedy* Edsger Wyde Dijkstra’s algorithm. There are many ways to implement the algorithm. We will use C++ STL’s *priority_queue* implementation for sake of simplicity.

This Dijkstra’s variant maintains a priority queue called as pq that stores pairs of vertex information. The first and the second item of the pair is distance from vertex from the source and vertex number, respectively.

**NOTE : code below assumes graph is implemented as (v,d) format in adjacency list** while pq flips this format for the sake of utilizing the priority queue.

This pq is sorted based on increasing distance from the source and tie, by vertex number. This is different from another Dijkstra’s implementation that uses binary heap feature that is not supported in this built-in library.

So pq contains `(0,s)` base case initially and greedily picks a vertex `(d,u)` from the front of pq. If the distace to u from source recorded in d greater than dist[u], it ignores u; otherwise, it process u. The real reason for this check is explained below.

*When this algorithm process u, it tries to relax all neighbors v of u. Every time it relaxes an edge u -> v, it will en-queue a pair(newer/shorter distance to v from source) into pq and leave the inferior pair (older/longer distance to v from source) inside pq. This is called as Lazy Deletion*. 

This approach leaves more than one copy of the same vertex in pq with different distances from source. That is why we have the check earlier to process only first `dequeued` vertex information pair which has correct/shorter distance (other copies are longer).

````c++
vi dist(V, INF); dist[s] = 0;
priority_queue< ii, vector<ii>, greater<ii>> pq;
pq.push(ii(0,s));
while(!pq.empty()) {
  ii front pq.top(); pq.pop();
  int d = front.first, u = front.second;
  if(d > dist[u]) continue;
  for(auto v : adj[u]) {
    if(dist[u] + v.second < dist[v.first]) { 
      dist[v.first] = dist[u] + v.second; // relaxation
      pq.push(ii(dist[v.first],v.first));
    }
  }
}
````

#### Sample Application : UVa 11367 - Full Tank ?

Abridged Problem : Given a connected weighted graph *length* that stores the road lengths between E pairs of cities i and j, the prices p[i] of fuel at each city i, and the fuel tank capacity c of a car. Determine the cheapest trip cost from starting city s to ending city e using the fuel capacity c.

This problem actually shows the importance of graph modeling. The explicitly given graph in this problem is weighted graph of road networks. However we cannot solve this problem with just this graph. This is because the state of this problem requires not just the current location but also the fuel level at that location. Otherwise we cannot determine whether car is able to complete the trip.

Therefore we require a pair of information to represent the state : (*location, fuel*) and by doing so, the total number of vertices of the modified graph explodes from just 1000 vertices to 100000 vertices. We call the modified graph ‘state-space graph’

Solution : In the state-space graph, the source vertex is state (s,0) - at starting city s with empty fuel tank and target vertices are states (e,any) - at ending city with any level of fuel between [0….c]. There are two types of edges in State-Space Graph : 0-weighted edge that goes from vertex (x, fuel_x) to vertex(y,fuel_x - len(x,y)) if the car has sufficient fuel to travel from vertex x to vertex y, and the p[x] weighted edge that goes from vertex (x, fuel_x) to vertex (x, fuel_x + 1) if the car can be refuel at vertex x by one unit of fuel (fuel capacity < c). Now running dijkstra on this State-Space graph gives us the solution for this problem.

### SSSP on Graph with Negative Weight Cycle

typical implementation of dijkstra’s algorithm on negative-graph can produce wrong answer. However, Dijkstra’s implementation variant shown before will work just fine, only lit bit of slower. Because Dijkstra’s implementation we saw before will keep inserting new vertex information pair into the priority queue every time it does a relax operation. So if graph has no negative weight cycle, the variant will keep propagating the shortest path distance information until there is no more possible relaxation (which implies shortest path is found). However, when given a graph with negative weight cycle, the variant - if implemented as shown before will be trapped in an infinite loop.

To solve SSSP prooblem in potential presence of negative cycle, the more generic (slower) Bellman Ford’s algorithm must be used. Idea is simple :

*Relax all E edges (in arbitrary order) V-1 times!*

Main part of the code is more simpler than BFS and Dijkstra’s code.

````c++
vi dist(V,INF); dist[s] = 0;
for(int i = 0; i < V-1; i++) {// relax V-1 times
  for(int u = 0; u < V; u++) {
    for(auto v: adj[u])
      dist[v.first] = min(dist[v.first], dist[u]+v.second);
  }
}
````

Here Complexity becomes $O(V^3)$ if graph is Adjacency Matrix or $O(VE)$ if graph is stored as adjacency list. Both are slow as compared to Dijkstra’s.

*Corollary: we proved that after relaxing all E edges V-1 times, the SSSP should have been solved, i.e. we cannot relax any more edge. If we can still relax an edge, there must be negative cycle in our graph*.

````c++
bool hasNegativeCycle = false;
// after running above bellman ford
// one more pass to check for possible relaxation
// relax V-1 times
for(int u = 0; u < V; u++) {
  for(auto v: adj[u])
    if(dist[v.first] > dist[u] + v.second)
      hasNegativeCycle = true;
}
printf("Negative Cycle Exists ? %s\n", hasNegativeCycle ? "YES" : "NO");
````

## APSP

Given a connected, weighted graph G with V $\le$ 100 and two vertices s and d. Find the maximum possible value of `dist[s][i] + dist[i][d]` over all possible $i \in [0 ... V-1]$ .

This problem requires shortest path information from all possible sources (all possible vertices) of G. We can make V calls of Dijkstra’s algorithm that we have learned earlier. However we can solve the problem in shorter way in terms of code. If weighted graph has V $\le$ 400, then there is another algorithm that is simple to code.

Load the small graph into a Adjacency Matrix and run the following 4-liner. When it terminates AdjMat `[i][j]` will contain the shortest path distance beween two vertices i and j in G.

````c++
// inside main
// adj[i][j] contains the weight of edge i to j
// or INF (1B) if there is no such edge
for(int k = 0; k < V; k++)
  for(int i = 0; i < V; i++)
    for(int j = 0; j < V; j++)
      adj[i][j] = min(adj[i][j], adj[i][k] + adj[k][j])
````

Above algorithm is called as Floyd Warshall’s algorithm, invented by Robert W Floyd and Stephen Warshall. This is a DP algorithm that clearly runs in O(V^3) due to 3 nested loops.

In general above code solves the APSP problem rather can calling SSSP algorithm multiple times.

1. V calls of O((V+E) log V) Dijkstr’s = $O(V^3 \log V)$ if $E = O(V^2)$.
2. V calls of O(VE) Bellman Ford’s  = $O(V^4)$ if $E = O(V^2)$.

### Other Applications

#### Solving SSSP Problem on Small Weighted Graph

If we have the APSP information we also know the SSSP information.

Printing Shortest Path

A common issue encountered by programmers who use four-liner Floyd Warshall’s without understanding how it works is when they are asked to print shortest paths too. In BFS/Dijkstra’s/ Bellman Ford’s algorithm we just need to remember the shortest paths spanning tree by using a 1D vector to store parent information for each vertex. In Floyd Warshall’s we need to store a 2D parent matrix. The modified code is as

````c++
for(int i = 0; i < V; i++)
  for(int j = 0; j < V; j++)
    p[i][j] = i; // initialize the parent matrix
for(int k = 0; k < V; k++)
  for(int i = 0; i < V; i++)
    for(int j = 0; j < V; j++)
      if(adj[i][k] + adj[k][j] < adj[i][j) {
        adj[i][j] = adj[i][k] + adj[k][j];
        p[i][j] = p[k][j];
      }
//------------------------------------------------------
void printPath(int i, int j) {
  if(i!=j) printPath(i,p[i][j]);
  printf(" %d", j);
}
````

#### Transitive Closure (Warshall’s Algorithm)

Given a graph, determine if vertex i is connected to j, directly or indirectly. This variant uses bitwise operators which are much faster than arithmatic operators.

Initially set entire adj to 1 if i connected to j otherwise 0.

After running Floyd’s Algorithm we can check if any two vertices are connected by check adj matrix

````c++
for(int k = 0; k < V; k++)
  for(int i = 0; i < V; i++)
    for(int j = 0; j < V; j++)
      adj[i][j] != (adj[i][k] & adj[k][j]);
````

#### Minimax and Maximin

We have seen the minimax(and maximin) path problem earlier. The solution using Floyd Warshall’s is shown below. First intialize `adj[i][j]` to be the weight of edge (i,j). This is default minimax cost for two vertices that are directly connected. For pair i-j without any direct edge, set `adj[i][j] = INF`. Then we try all possible intermediate vertex k. The minimax cost `adj[i][j]` is minimum of either (itself) or (the maximum between `adj[i][k]] or adj[k][j]`) However this approach works only for  V $\le$ 400.

````c++
for (int k = 0; k < V; k++)
	for(int i = 0; i < V; i++)
		for(int j = 0; j < V; j++)
			adj[i][j] = min(adj[i][j], max(adj[i][k],adj[k][j]))
````

#### Finding the Cheapest/Negative Cycle

We know Bellman Ford will terminate after O(VE) steps regardless of the type of input graph, same is the case with Floyd Warshall it terminates in $O(V^3)$.

To solve this problem, we intially set the main diagonal of the adj matrix to a very large value, i.e. `adj[i][i] = INF`, which now mean shortest cyclic paht weight strating from vertex i goes thru up to V-1 other intermediate vertices and return back to i. If `adj[i][i]` is no longer INF for any $i \in [0…V-1]$, then we have a cycle. The smallest non-negative `adj[i][j]` $\forall i \in [0…V-1]$ is the cheapest cycle.

If `adj[i][j]` < 0 for any $i \in [0…V-1]$, then we have a negative cycle because if we take this cyclic path one more time, we will get even shorter `shortest` path.

#### Finding the Diameter of a Graph

The diameter is maximum shortest path distance between any pair, just find the i and j s.t. its maximum of the matrix adj.

#### Finding the SCCs of a Directed Graph

Tarjan’s algorithm can be used to identify SCCs of a digraph. However code is bit long. If input graph is small, we can also identify the SCCs of graph in $O(V^3)$ using Warshall’s transitive closure algorithm and then use the following check.

 To find all the members of SCC that contains vertex i check all other vertices $V\in [0…V-1]$. If `adj[i][j] && adj[j][i]` is true, then vertex i and j belong to same SCC.

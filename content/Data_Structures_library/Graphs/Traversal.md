+++

Title = "Traversal"

weight = 1

+++

#### Depth First Search (DFS)

- traverses graph in 'depth-first' manner
- Time Complexity
  - $O(V+E)$ : Adjacency List
  - $O(V^2)$ : Adjacency Matrix

````c++
typedef pair<int,int> ii;
typedef vector<ii> vii;
typedef vector<int> vi;
const UNVISITED = -1;
const VISITED = 1;

vi dfs_num;				// initially all set to unvisited

void dfs(int u) {
  dfs_num[u] = VISITED;
    for(auto v:adj[u]) {
       if(dfs_num[v] == UNVISITED)
         dfs(v);
  } }
````

#### Breadth First Search (BFS)

- traverses graph in 'breadth-first' manner
- Time Complexity
  - $O(V+E)$ : Adjacency List
  - $O(V^2)$ : Adjacency Matrix

````c++
// inside int main() -- no recursion
vi d(V,INF); d[s] = 0;					// distance from source s to s is 0
  queue<int> q; q.push(s);

  while(!q.empty()) {
    int u = q.front(); q.pop();
    for(auto v:adj[u]) {
      if(d[v] == INF) {
        d[v] = d[u] + 1;
        q.push(v);
 } } }
````

#### Finding Connected Components (Undirected Graph)

- Can be done using Union-Find and BFS also.

````c++
// inside int main() -- this is DFS Solution
numCC = 0;
dfs_num.assign(V,UNVISITED);
for(int i = 0; i < V; i++)
  if(dfs_num[i] == UNVISITED)
      ++numCC;
````

#### Flood Fill - Labeling/Coloring the Connected Components

This version actually counts the size of each component and colors it.

````c++
int dr[] = {1,1,0,-1,-1,-1, 0, 1};	// trick to explore implicit 2D grid
int dc[] = {0,1,1, 1, 0,-1,-1,-1};

int floodfill(int r, int c, char c1, char c2) {
    if(r < 0 || r >= R || c < 0 || c >= C) return 0;
    if(grid[r][c] != c1) return 0;
    int ans = 1;
    grid[r][c] = c2;
    for(int d = 0; d < 8; d++)
        ans += floodfill(r+dr[d], c+dc[d], c1, c2);
    
    return ans;
}
````

#### Topological Sort (DAG)

- Linear ordering of vertices in the DAG so that vertex `u` comes before vertex `v` if edge (u to v) exists in DAG.
- A DAG has at least one and possibly more topological sort(s);

````c++
vi ts;         // global vector to store toposort in reverse order
void dfs2(int u) {
    dfs_num = VISITED;
    for(auto v:adj[u]) {
        if(dfs_num[v] == UNVISITED)
            dfs2(v);
    }
    ts.push_back(u);                // this is the only change we need
}
````

````c++
// inside int main()
ts.clear();
memset(dfs_num,UNVISITED, sizeof dfs_num);
for(int i = 0; i < V; i++)
    if(dfs_num[i] == UNVISITED)
        dfs2(i);
for(int i = (int) ts.size() - 1; i >= 0; i--)       // read backwards
    cout << ts[i] << " ";
````

#### Bipartite Graph Check

- also known as 2/bi-colorable graph
- BFS is more natural choice for this problem

````c++
// inside int main
queue<int> q; q.push(s);
vi color(V,INF); color[s] = 0;
bool isBipartite = true;
while(!q.empty() & isBipartite) {
    int u = q.front(); q.pop();
    for(auto v: adj[u]) {
        if(color[v] == INF) {
            color[v] = 1 - color[u];
            q.push(v); }
		else if(color[v] == color[u]) {
            isBipartite = false; break; } } }
````

#### Graph Edges Property Check via DFS Spanning Tree

- Running DFS on a connected graph generates a DFS *spanning tree* (or *spanning forest* if graph is disconnected). With the help of one more vertex state: EXPLORED = 2 (visited but not yet completed) on top of VISITED (visited and completed).
- We can classify graph edge into three types
  1.  Tree Edge : The edge traversed by DFS i.e. from a vertex currently with state EXPLORED to a vertex state UNVISITED
  2. Back edge : Edge that is part of a cycle, i.e. from state EXPLORED to a vertex with state EXPLORED too. This is important application of algorithm as it detects cycles
  3. Forward/Cross edges from vertex with state EXPLORED to vertex with VISITED.

````c++
void graphCheck(int u) {
    dfs_num[u] = EXPLORED;
    for(auto v: adj[u]) {
        if(dfs_num[v] == UNVISITED) {	// Tree Edge, EXPLORED->UNVISITED
            dfs_parent[v] = u;
            graphCheck(v);
        }
        else if(dfs_num[v] == EXPLORED) {
            if(v == dfs_parent[u])    // to differentiate these two case
                printf(" Two ways (%d,%d)-(%d,%d)\n",u,v,v,u);
            else// the most frequent application, check if graph is cyclic
                printf(" Back Edge (%d,%d) (cycle)\n",u,v);
        }
        else if(dfs_num[v] == VISITED) 		// EXPLORED=>VISITED
            printf(" Forward/Cross Edge (%d,%d)",u ,v);
    }
    dfs_num[u] = VISITED;
}
````

````c++
// inside int main()
dfs_num.assign(V,UNVISITED);
dfs_parent.assign(V,0);

for(int i = 0; i < V; i++)
  if(dfs_num[i] == UNVISITED)
    printf("Component %d:\n",++numComp), graphCheck(i);// 2 lines in 1 !
````

#### Finding Articulation Points and Bridges (Undirected Graph)

- This method is a DFS variant by John Edward Hopcroft and Robert Endre Tarjan.
-  we maintain`dfs_num(u)` and `dfs_low(u)`.
  - dfs_num : stores iteration counter when the vertex `u` is visited for first time.
  - dfs_low : stores lowest dfs_num reachable from current DFS spanning subtree of u.
- At the beginning `dfs_num(u) = dfs_low(u)` when vertex `u` is visited for the first time. Then `dfs_low(u)` can be made smaller if there is a cycle ( a back edge exists). Note we do not update `dfs_low(u)` with a back edge (u,v) if `v` is a direct parent of `u`.

````c++
void articulationPointAndBridges(int u) {
  dfs_low[u] = dfs_num[u] = dsfNumberCounter++;// dfs_low[u] <= dfs_num[u]
  for(auto v: adj[u]) {
    if(dfs_num[v] == UNVISITED) {
      dfs_parent[v] = u;                        // a tree edge
        if(u == dfsRoot) rootChildren++;     // special case if u is root
        
        articulationPointAndBridges(v);
        
        if(dfs_low[v] >= dfs_num[u])         // for articulation Point
            articulation_vertex[u] = true;   // store this information
        if(dfs_low[u] > dfs_num[u])
            printf(" Edge (%d,%d) is a bridge\n",u,v);
        dfs_low[u] = min(dfs_low[u],dfs_low[v]); // update dfs_low[u]
    }
      else if(v != dfs_parent[u])  // a back edge and not direct cycle
        dfs_low[u] = min(dfs_low[u],dfs_num[v]); // update dfs_low[u]
  }
}
````

````c++
// inside main
dfsNumberCounter = 0; dfs_num.assign(V,UNVISITED); dfs_low.assign(V,0);
dfs_parent.assign(V,0); articulation_vertex.assign(V,0);
printf("Bridges:\n");
for(int i = 0; i < V; i++)
    if(dfs_num[i] == UNVISITED) {
        dfsRoot = i; rootChildren = 0; articulationPointAndBridges(i);
        articulation_vertex[dfsRoot] = (rootChildren > 1); // special case
    }
````

#### Finding Strongly Connected Components (Directed Graph)

SCC is defined as : If we pick any pair of vertices u and v in the SCC, we can find a path from u to v and vice-versa. If SCCs are contracted they form a DAG.

There are 2 famous techniques for finding SCCs - Kosaraju's and Tarjan's algorithm, below we show Tarjan's Algorithm as it naturally extends from our previous discussion also due to Tarjan.

- Tarjan's Algorithm : Basic Idea is that SCCs form subtrees in the DFS spanning tree. ON top of computing dfs_num and dfs_low for each vertex, we also append vertex u to the back of stack S ( implemented as vector) and keep track of the vertices that are currently explored via vi visited. Algorithm runs in $O(V+E)$.

  

````c++
vi dfs_num, dfs_low, S, visited;
void tarjanSCC(int u) {
  dfs_low[u] = dfs_num[u] = dfsNumberCounter++;
  S.push_back(u);
  visited[u] = 1;
  for(auto v: adj[u]) {
    if(dfs_num[v] == UNVISITED)
        tarjanSCC(v);
    if(visited[v])
      dfs_low[u] = min(dfs_low[u], dfs_low[v]); }
    
    if(dfs_low[u] == dfs_num[u]) { // if this is the root of an SCC
      printf("SCC %d:", ++numSCC);
      while(1) {
        int v = S.back(); S.pop_back();  visited[v] = 0;
        printf(" %d",v);
        if(u == v) break;
      }
      prinf("\n");
   } }
````

````c++
// inside int main()
dfs_num.assign(V,UNVISITED); dfs_low.assign(V,0); visited.assign(V,0);
dfsNumberCounter = numSCC = 0;
for(int i = 0; i < V; i++)
    if(dfs_num[i] == UNVISITED)
        tarjanSCC(i);
````

- Same complexity as above. Its based on DFS.  Basic Idea of this algorithm is to run DFS twice. The first DFS is done on the original directed graph and record the post-order traversal of the vertices as in finding Topological Sort. The second DFS is done on transpose of the original directed graph using the 'post-order' ordering found by first DFS.

````c++
void Kosaraju(int u, int pass) { // pass = 1 (original), 2 (transpose)
  dfs_num[u] = 1;
  vii neighbor;       // use different adjacency list in two passes
  if(pas == 1) neighbor = adj[u]; else neighbor = adjT[u];
  for(auto v : neighbor) {
      if(dsf_num(v) == DFS_WHITE)
          Kosaraju(v,pass);
  }
  S.push_back(u);
}
````

````c++
// in int main()
S.clear();
dfs_num.assign(N, DFS_WHITE);
for(i = 0; i < N; i++)
    if(dfs_num[i] == DFS_WHITE)
        Kosaraju(i,1);
numSCC = 0;
dfs_num.assign(N, DFS_WHITE);
for(i = N-1; i >= 0; i--)
    if(dfs_num[S[i]] == DFS_WHITE) {
        numSCC++;
        Kosaraju(S[i],2); // AdjListT-> the transpose of original graph
    }
printf("There are %d SCCs\n", numSCC);
````


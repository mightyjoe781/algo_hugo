+++

title = "Graphs 1"
date = 2021-04-24T10:24:25+05:30
weight = 17

tags = ["DSA", "practice", "graphs","connectivity","dfs"]

+++

#### Why we use graphs

Graphs help us model a specific type of data that has some dependency and relationships.

Eg. Facebooks friends data, Routes between cities and places (models the connectivity and distance) , Networking (connectivity), Pre-requisite of courses (knowledge and dependency), File Systems, Resource Allocation in OS (handling deadlocks).

#### Representation of Graphs

Represented as a Nodes and edges. Nodes : data while edge represents relationships.

Most Simplest implementation is adjacency matrix but its not memory efficient for sparse graphs. We use adjacency list for sparse graphs.

These relationships between data can be 

- Directed (perquisite , resource allocation)
- Undirected (friendship)

Relationships can have weights too! 

[Clone Graph](https://leetcode.com/problems/clone-graph/)

2 things are of prime importance.

- Traverse the graph
- Create new graph nodes

// general structure of dfs

````c++
// pseudo_code
void dfs(Graph G, Node s){
    visited[s] = true;
    // explore neighbours
    for(nbrs in s)
        if(!visited[nbrs])
            dfs(g,nbrs);
}
````

Above code is framework of DFS, that just traverses the graph, we intend to incorporate a way to clone the graph into it.

````c++
typedef Node* link;
void dfs(link start,unordered_map<link,link>& m){
    link new_node = new Node(start->val);
    m[start] = new_node;

    // visit all the neighbours
    for(auto& nbr: start->neighbors){
        if(m.find(nbr) == m.end())
            dfs(nbr,m);
        new_node->neighbors.push_back(m[nbr]);
    }
}
link cloneGraph(link node) {
    if(!node) return NULL;
    unordered_map<link,link> m;		// notice map built on links
    dfs(node,m);
    return m[node]; }
````

Connectivity Problems

[Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

If we start dfs on a node it will eventually traverse the entire connected components.

We are asked to find the number of connected components.

````c++
void dfs(vector<vector<int>>& graph, vector<bool>& visited,int start){
    visited[start] = true;
    for(int i = 0; i < (int)graph.size(); i++){
        if(graph[start][i] && !visited[i])
            dfs(graph,visited,i);
    }
}
int findCircleNum(vector<vector<int>>& isConnected) {
    int n = isConnected.size();
    vector<bool> visited(n,false);
    int res = 0;
    for(int i = 0; i<n; i++){
        // if not visited , visit and mark connected components
        if(!visited[i]){
            res++;
            dfs(isConnected,visited,i);
        }
    }     return res; }
````

[Satisfiability of Equality Equation](https://leetcode.com/problems/satisfiability-of-equality-equations/)

Facebook question ! :)

a==b -> this of it as a,b are nodes and == is a edge representing the connection

so a==b , b==c , d == b then these all nodes are connected.

Lets handle case a!=b

if we find a,b,c,d equal we can label them 0,0,0,0

similarly we find e,f same we label then 1,1

and g,i : 2,2

now if we find a!=b , and if they have same label that means expression is false.

````c++
void dfs(unordered_map<char,vector<char>>& graph, char start, unordered_map<char,int>& visited,int label){
    visited[start] = label;
    for(auto& nbr:graph[start]){
        if(visited.find(nbr) == visited.end())
            dfs(graph,nbr,visited,label);
    }
}

bool equationsPossible(vector<string>& equations) {
    // construct graph
    unordered_map<char,vector<char>> graph;
    unordered_map<char, int> visited;
    for(auto& eqn : equations){
        // checking the case of nonequal same nodes
        // a!=a always false
        if(eqn[0] == eqn[3]){
            if(eqn[1] == '!')
                return false;
        }
        // creating a graph
        if(eqn[1] == '=' ){
            graph[eqn[0]].push_back(eqn[3]);
            graph[eqn[3]].push_back(eqn[0]);
        }
    }
    int label = 0;
    for(auto& node : graph){
        // dividing items into proper groups
        if(visited.find(node.first) == visited.end()){
            dfs(graph,node.first,visited,label);
            label++;
        }
    }
    // process inequalities
    for(auto& eqn : equations)
        if(eqn[1] == '!')
            if(visited.find(eqn[0])!= visited.end() &&
               visited.find(eqn[3])!= visited.end() &&
               visited[eqn[0]] == visited[eqn[3]])
                return false;
    return true; }
````

There are some subtle testcases like `a!=a` its a contradiction and we can always return false.

Second is seeing the characters that have no label ( they were never connected in first place like x!=z) we handle that case using the if condition while processing inequalities.

**Path (a,b)** : sequence of nodes followed from a to reach b and these node shouldn't be repeated.

If nodes are repeated then its no longer a path rather it becomes a **walk(a,b)**

**Connectivity(a,b)** : two nodes a and b are said to be connected if there exists a path between a and b.

Connectivity of Graph : A graph is said to be connected if $\exist$ a path b/w a & b $\forall$ a,b $\in$ V, a $\ne$ b.

- Given a graph, find out whether it is connected or not
  - traversal of graph leaves no unvisited vertices.
- Components such that each component is connected, find the number of connected components
  - start with a node and start dfs from there, running dfs once on a component will traverse it.

**Modelling Graph Problems :**

There might be 2 Cases

1. Explicitly given graph
2. No given explicitly

Steps to model / solve graph problems

1.  Define your graph for the problem (sometimes its trivial, sometimes its not)
2. Bucket the problem into one of existing problem ( connectivity,DAG, etc )

**Connectivity Problems**

[Number of Islands](https://leetcode.com/problems/number-of-islands/)

lets redefine as graph

- (i,j) such that A(i,j) = 1 is a node
- v_1 and v_2 are adjacent iff they are 4 neighbors of each other.

Then problem reduces to the finding the connected components of graph.

Point to Note : Graph that you define doesn't have to construct explicitly!

Solving the problem without inherently constructing the graph is a skill to hone, We can make our dfs such that it does that for us! :)

````c++
vector<vector<int>> nbrs = {{1,0},{0,1},{-1,0},{0,-1}};
void dfs(vector<vector<char>>& grid, int i, int j, vector<vector<bool>>& visited){
    visited[i][j] = true;
    // perform dfs on the neighbors
    for(auto& nbr : nbrs){
        int x = i + nbr[0];
        int y = j + nbr[1];
        // (x,y) is adjacent to (i,j)
        if(x < 0 || y <0 || x >= grid.size() || y >= grid[0].size()
           || visited[x][y] || grid[x][y] == '0')
            continue;
        dfs(grid,x,y,visited);
    } 
}
int numIslands(vector<vector<char>>& grid) {
    int res = 0;
    vector<vector<bool>> visited(grid.size(),vector<bool> (grid[0].size(), false));
    for(int i = 0; i < grid.size(); i++){
        for(int j = 0; j < grid[0].size(); j++){
            if(grid[i][j] == '1' && !visited[i][j]){
                dfs(grid,i,j,visited);
                res++;
            }
        }
    }
    return res;
}
````

[Surrounded Region](https://leetcode.com/problems/surrounded-regions/)

Do a dfs on boundaries !

````c++
vector<vector<int>> nbrs = {{0,1}, {1,0}, {-1,0}, {0,-1}};
void dfs(vector<vector<char>>& board, int i, int j, vector<vector<bool>>& visited){
    visited[i][j] = true;

    // neighbors
    for(auto& nbr: nbrs){
        int x = i + nbr[0];
        int y = j + nbr[1];

        if(x < 0 || y < 0 || x >= board.size()
           || y >= board[0].size() || visited[x][y] || board[x][y] == 'X')
            continue;
        dfs(board,x,y,visited);
    }
}
void solve(vector<vector<char>>& board) {
    int m = board.size(), n = board[0].size(), i, j;
    vector<vector<bool>> visited(m,vector<bool>(n,false));
    // top and bottom boundries
    for(i = 0; i <n ; i++){
        if(!visited[0][i] && board[0][i] =='O')
            dfs(board,0,i,visited);
        if(!visited[m-1][i] && board[m-1][i] == 'O')
            dfs(board,m-1,i,visited);
    }
    // left and right boundaries
    for(i = 0; i <m ; i++){
        if(!visited[i][0] && board[i][0] =='O')
            dfs(board,i,0,visited);
        if(!visited[i][n-1] && board[i][n-1] == 'O')
            dfs(board,i,n-1,visited);
    }
    // flip the relevant 0s
    for(i = 0; i< m; i++){
        for(j = 0; j < n; j++){
            if(board[i][j] == 'O' && !visited[i][j])
                board[i][j] ='X';
        }
    }}
````

[Number of Distinct Islands](https://leetcode.com/problems/number-of-distinct-islands/)

Given a 2D array grid 0's and 1's island as a group of 1's representing land connected by 4- neighbors.

Count number of distinct islands. An island is considered to be same as another if and only if one island can be translated (and not rotated/reflected) to equal the other.

Hmm make a island into a string xD and then store it in the map (kind of like serialization) , now for each island check before whether its stored in map then its not distinct and don't count it in result.

Now how to serialize : Create a traversal strings representing traversal DRLU representing the down,right,left,up!

````c++
vector<vector<int>> nbrs = {{0,1}, {1,0}, {-1,0}, {0,-1}};
vector<char> dirs = {'R','U','L','D'};
void dfs(vector<vector<char>>& grid, int i, int j, vector<vector<bool>>& visited,string& island){
    visited[i][j] = true;

    for(int k = 0; k < nbrs.size(); k++){
        // neighbors
        int x = i + nbr[k][0];
        int y = j + nbr[k][1];

        if(x < 0 || y < 0 || x >= grid.size()||
           y >= grid[0].size()||visited[x][y] || grid[x][y] == 0)
            continue;
        
        island+=dirs[k];
        dfs(board,x,y,visited);      
    }
    island+="#";
}
int numDistinctIsland(vector<vector<int>>& grid){
    int m = grid.size(), n = grid[0].size(), i, j;
    unordered_set<string> islands;
    vector<vector<bool>> visited(m,vector<bool>(n,false));
    
    for(i = 0; i< m; i++){
        for(j = 0; j < n; j++)
            if(grid[i][j] == 1 && !visited[i][j]){
                string island_patter="";
                dfs(grid,i,j,visited,island_pattern);
                if(!islands.find(island_pattern)== islands.end())
                    islands.insert(island_pattern);
            }
    }
    return (int) islands.size();
}
````




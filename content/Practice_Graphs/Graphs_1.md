+++

title = "Graphs_1"
date = 2021-04-24T10:24:25+05:30
weight = 17

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
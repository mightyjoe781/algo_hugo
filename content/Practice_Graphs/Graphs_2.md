+++

title = "Graphs 2"
date = 2021-04-24T23:01:25+05:30
weight = 18

tags = ["DSA", "practice", "graphs","cyclicity"]

+++

#### Strong and Weak Connectivity

These terms arise in Directed Graph.

Weak Connectivity : Take a directed graph and convert into undirected graph and do connectivity operations on that is called as weak connectivity.

Strong Connectivity : (A,B) are strongly connected $\iff \exist $   path b/w A & B.

​									A --> B and B-->A

#### Graph Cyclicity

Cycle : A sequence of nodes and edges such that first and last node of sequence is same.

Undirected Graph : Find out whether it contains a cycle : there should be a cyclic walk.

Solution : dfs discovers a visited node xD ! beware the case of neighbor :)

Now how about directed graph : Cycle when the walk connects back to a node we were visiting but wait we should have a way to memorize the path we are visiting !

If we have some back edge but its not connecting back to path we are traversing that means its not cyclic.

Now to maintain the memory of current path we are traversing :) use a stack or a visited vector but we have to mark it false while backtracking.

[Course Schedule](https://leetcode.com/problems/course-schedule/)

We keep the track of path which we are traversing with `curr_path` and then set it false when we backtrack.

````c++
bool containsCycle(vector<vector<int>>& graph, vector<bool>& visited, vector<bool>& curr_path, int i){
    bool t = false;

    for(auto& nbrs:graph[i]){
        if(curr_path[nbrs] == true)
            t = true;

        if(visited[nbrs]){
            curr_path[nbrs] = true;
            visited[nbrs] = true;
            t = t || containsCycle(graph,visited,curr_path,nbrs);
            curr_path[nbrs] = false;
        }
    }
    return t; 
}
bool canFinish(int n, vector<vector<int>>& prerequisites) {
    // construct the graph
    // adjacency list
    vector<vector<int>> graph(n);
    vector<bool> visited(n,false);
    vector<bool> curr_path(n,false);
    // directed edge from b_i to a_i 
    for(auto& pre:prerequisites){
        graph[pre[1]].push_back(pre[0]);
    }
    // find out this graph contains cycles
    bool t = false;
    for(int i = 0; i < n ; i++)
        if(!visited[i]){
            visited[i] = curr_path[i] = true;
            t = t || containsCycle(graph,visited,curr_path,i);
            curr_path[i] = false;
        }    return !t;}
````

[Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

Its topological sort !

Well first try to find the course which doesn't have a any prerequisite.

Now how to identify that course ? we need extra information! we can assign a inflow and outflow to nodes.

Make a indegree vector contains the number of edges to i { from any of the nodes from the graph}.

We can traverse from the node with indegree zero

````c++
vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    // populate the graph and indegree vector
    vector<vector<int>> graph(numCourses);
    vector<int> in(numCourses,0);
    vector<int> curr_set;
    int idx = 0;
    vector<int> res;

    for(auto& pre:prerequisites){
        // edge from 1 to 0
        graph[pre[1]].push_back(pre[0]);
        in[pre[0]]++;
    }

    // initialize the intial list to traverse
    for(int i = 0; i < numCourses; i++){
        if(in[i] == 0)
            curr_set.push_back(i);
    }
    // traverse this list :
    while(idx < curr_set.size()){
        int curr_node = curr_set[idx];
        res.push_back(curr_node);
        // decrease the indegree by 1 for all the nbrs of idx
        for(auto& nbr : graph[curr_node]){
            in[nbr]--;
            if(in[nbr] == 0)
                curr_set.push_back(nbr);
        }
        idx++;
    }     return (int)res.size() == numCourses ? res:vector<int>(); }
````

Now `curr_set` we use, we could have used the queue or maps or anything that allows fast retrieval and keep together similar indegree node (0).



DAG : Directed Acyclic Graph

Topological Sort : ordering of a DAG s.t all the edges are from left to right.


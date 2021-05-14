+++

title = "Graphs 2"
date = 2021-04-24T23:01:25+05:30
weight = 18

tags = ["DSA", "practice", "graphs","cyclicity","shortest-path"]

+++

#### Strong and Weak Connectivity

These terms arise in Directed Graph.

Weak Connectivity : Take a directed graph and convert into undirected graph and do connectivity operations on that is called as weak connectivity.

Strong Connectivity : (A,B) are strongly connected $\iff \exists $   path b/w A & B.

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
bool containsCycle(vector<vector<int>>& graph, vector<bool>&
                   visited, vector<bool>& curr_path, int i){
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
        }    
    return !t;}
````

[Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

Its topological sort !

Well first try to find the course which doesn't have a any prerequisite.

Now how to identify that course ? we need extra information! we can assign a inflow and outflow to nodes.

Make a indegree vector contains the number of edges to i { from any of the nodes from the graph}.

We can traverse from the node with indegree zero

````c++
vector<int> findOrder(int numCourses,
                      vector<vector<int>>& prerequisites) {
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
    }     
    return (int)res.size() == numCourses ? res:vector<int>(); }
````

Now `curr_set` we use, we could have used the queue or maps or anything that allows fast retrieval and keep together similar indegree node (0).



DAG : Directed Acyclic Graph

Topological Sort : ordering of a DAG s.t all the edges are from left to right.

### Shortest Path

[Word Ladder](https://leetcode.com/problems/word-ladder/)

Vertex of graph is : All the words in the dictionary including start and end word.

$v_i - v_j $

Connect them if it is possible to get $v_j$ by changing exactly one letter of $v_i $ & vice-versa.

Now problem reduced to finding shortest path from source to sink in an undirected graph!

Now if we do a level order traversal and the level that first see's the sink is the solution to problem.

Wait do we need to construct graph explicitly ! Though it looks cost efficient to build graph but that doesn't affect the performance that much because anyway we have to look at the all possibilities!

So let's do without creating the graph.

````c++
int ladderLength(string beginWord, string endWord,
                 vector<string>& wordList) {
    unordered_set<string> dict(wordList.begin(),wordList.end());
    queue<pair<string,int>> q;
    unordered_set<string> visited;

    // initialize with the beginWord
    q.push({beginWord,1});
    visited.insert(beginWord);

    // level-order traversal
    while(!q.empty()){
        pair<string,int> t = q.front();
        q.pop();

        if(t.first == endWord){
            return t.second;
        }
	// go thru neighbors, mark the visited and push them in queue.
        string word = t.first;
        int dist = t.second;
        for(int i = 0; i < word.size(); i++){
            // generate all words with 1 character distance
            for(int j = 0; j < 26; j++){
                word[i] = (j+'a');

                if(dict.find(word) != dict.end() &&
                   visited.find(word) == visited.end()){
                    visited.insert(word);
                    q.push({word,dist+1});
                }
                word[i] = (t.first)[i];
            }
        }
    }     
    return 0;
}
````

[01 Matrix](https://leetcode.com/problems/01-matrix/)

For each zero solution is zero.

While for each 1, we could do BFS on each cell and distance is the answer.

There is a issue : BFS on worst case $O(n^2)$ while overall problem becomes $O(n^4)$, A simple modification to BFS could bring it down to $O(n^2)$.

Nodes are all ones and edges are 4-neighbor.

So solution below first picks up zeros and distance is marked zero for them and as a graph node they are put onto a queue.

then for each zero we find its neighbors which are unvisited, we mark them the distance and put nodes that are marked with distance on the queue again so they can propagate their level distance.

Its correct approach as 1's are processed after all zeroes are processed. In a way when zero exhausts we have to reach 1s then we won't encounter any zeroes anymore otherwise it won't be the shortest distance.

````c++
vector<vector<int>> offset = {{0, 1}, {1, 0}, {-1, 0}, {0,-1}};
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    // q[0] = row indx.
    // q[1] = col indx.
    // q[2] = distance.
    int m = mat.size(), n = mat[0].size();
    queue<vector<int>> q;
    vector<vector<int>> res(m,vector<int>(n,-1));

    // Initialize the q and visited array (res).
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(mat[i][j] == 0) {
                q.push({i,j,0});
                res[i][j] = 0;
            }
        }
    }

    // Level-order Traversal.
    while(!q.empty()) {
        vector<int> curr = q.front();
        q.pop();

        // Go through the nbrs.
        for(auto& entry : offset){
            int x = curr[0] + entry[0];
            int y = curr[1] + entry[1];

            if(x < 0 || y < 0 || x >= m || y >= n)
                continue;

            if(res[x][y] == -1) {
                res[x][y] = curr[2]+1;
                q.push({x,y,curr[2] + 1});
            }
        }
    }
    return res;
}
````

Here Level-Order Traversal can be done in 2 ways

- Before pushing into the queue check if visited

1. If not visited then mark it visited before pushing (Implemented Above)

2. not visited then don't mark but just push in queue
   - pop and mark it visited

What is consequence of both approaches

Approach 2 obviously will have a node more than once :) but what does that imply !

That means there are more than one-shortest path !

Approach 1's queue size doesn't increase too much but in case of approach 2 queue size might go quite large.

````c++
vector<vector<int>> offset = {{0, 1}, {1, 0}, {-1, 0}, {0,-1}};
vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
    // q[0] = row indx.
    // q[1] = col indx.
    // q[2] = distance.
    int m = mat.size(), n = mat[0].size();
    queue<vector<int>> q;
    vector<vector<int>> res(m,vector<int>(n,-1));

    // Initialize the q and visited array (res).
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(mat[i][j] == 0) {
                q.push({i,j,0});	// % %
            }
        }
    }

    // Level-order Traversal.
    while(!q.empty()) {
        vector<int> curr = q.front();
        q.pop();
        
        // % difference from above code %
        if(res[curr[0]][curr[1]] != -1)
            continue;
        
        res[curr[0]][curr[1]] = curr[2];

        // Go through the nbrs.
        for(auto& entry : offset){
            int x = curr[0] + entry[0];
            int y = curr[1] + entry[1];

            if(x < 0 || y < 0 || x >= m || y >= n)
                continue;

            if(res[x][y] == -1) {
                q.push({x,y,curr[2] + 1}); // % %
            }
        }
    }
    return res;
}
````

Check out the memory of both submissions xD

Why we did this though, Read up Dijkstra's Algorithm!

[As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible/)

Topological Sort !

[Alien Dictionary]()

We can represent the problem as directed graph. Now if there is a cycle or self loops then its invalid.

Vertices will be unique characters from the words.

After the construction of graph we have to return topological sort of the graph!

Now should we make graph explicitly. Yes indeed because topological sort requires the neighbors but we can't have neighbors through given structure of input and will cost $O(n^2)$ to every letter.

We used the unordered_set just to quickly check whether there is a edge already there.

````c++
string alienOrder(vector<string>& words) {
    // construct the graph
    // nodes - letters
    // a->b iff a < b
    // Adjacency list.
    unordered_map<char, unordered_sets<char>> graph;
    int n = words.size();
    unordered_map<char, int> indegree;
    // preprocessing step for intializing to empty value
    // if there is some vertex that doesn't get processed 
    // while graph construction i.e. stand alone vertices
    for(int i = 0; i < n; i++){
        for(int k = 0; k < words[i].size(); k++) {
            if(graph.find(words[i][k]) == graph.end()) {
                graph[words[i][k]] = {};
                indegree[words[i][k]] = 0;
            }
        }
    }
    // Graph Construction.
    for(int i = 0; i < n; i++){
        for(int j = i+1; j < n; j++){
            // equal words...
            if(words[i] == words[j])
                continue;
            
			int k = 0, l = 0;
            while(k < words[i].size() && l < words[j].size()
                 && words[i][k] == words[j][l]){
                k++;l++;
            }
            // words[j] is a prefix of words[i].
            if(l == words[j].size())
                return "";
            
            if(k == words[i].size())
                continue;
            
            if(graph[words[i][k]].find(words[j][l]) == 
                                       graph[words[i][k]].end()) {
                // undirected graph!
                 graph[words[i][k]].push_back(words[j][l]);
                indegree[words[j][l]]++;
            }
           
        }
    }
   	// Topological sort
    queue<char> q;
    
    // Initialize the queue
    for(const auto& entry:indegree){
        if(entry.second == 0)
            q.push(entry.first);
    }
    while(!q.empty()){
        char ch = q.front();
        q.pop();
        res+= q;
        for(const auto& nbrs: graph[ch]){
            indegree[nbrs]--;
            if(indegree[nbrs] == 0)
                q.push(nbrs);
        }
    }
    return res.size() == graph.size() ? res : "";
}
````

Improvements : We need to check only consecutive words to create graph.

Because graph captures transitive relation by the fact input is sorted in alien dictionary implying word $ a, b, c$ in order mean $a < b$ and $ b < c$ but that directly implies $ a < c $.
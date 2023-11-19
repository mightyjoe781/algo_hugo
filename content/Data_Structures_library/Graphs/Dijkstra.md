+++

Title = "Dijkstra"

weight = 10

+++

This is simplest implementation of Dijkstra in C++.

Assumes input is given as input[i] : $[u_i,v_i,w]$ , a source vertex `s` , a destination vertex `d`.

Below code solves the `dist[]` : which will in the end contain required distance as `dist[d]`.

Note : below code assumes vertex in range `[0,1,...n-1]`

Reference : https://www.coursera.org/lecture/algorithms-part2/dijkstras-algorithm-2e9Ic

````c++
typedef pair<int,int> pi;
int dijkstra(vector<vector<int>>& input, int s, int d, int n) {
    // construct graph
    vector<vector<int>> G(n);
    for(auto& inp:input)
        G[inp[0]].push_back({inp[1],inp[2]});
    // Create distance array
    vector<int> dist(n,INT_MAX);
    //$ vector<int> p(int,-1);
    dist[s] = 0;
    // priority queue : min_heap
    priority_queue<pi,vector<pi>,greater<pi>> pq;
    pq.push({0,s});
    while(!pq.empty()){
        int u = pq.top().second;
        pq.pop();
        // explore and do relaxation on neighbors
        for(auto& it: G[u]) {
            int v = it.first;
            int w = it.second;           
            // there exists shorted path to v through u
            if(dist[u] + w < dist[v]) {
                // update distance
                dist[v] = dist[u] + w;
                //$ p[v] = u;
                pq.push({dist[v],v});
            }
        }
    }
    // now dist contains shortest distance of all vertices from s
    return dist[d];
}
````



A variant of above algorithm for listing out the shortest path is to uncomment the `//$`  lines and use this function to recurse on the path vector.

Note : this implementation assumes that there is a shortest path and above ^ algorithm found it out already :)

````c++
vector<int> restore_path(int s, int d, vector<int>& p) {
    vector<int> path;
    for(int v = d; v!=s; v=p[v])
        path.push_back(v);
    path.push_back(s);
    // reverse the path do correct the ordering
    reverse(path.begin(),path.end());
    return path;
}
````


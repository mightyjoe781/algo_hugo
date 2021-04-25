+++

title = "BST Review"
date = 2021-04-17T09:01:25+05:30
weight = 10

+++

#### Review

Ordered Search Structures  we use whenever there is a need for ordering. Great examples are BSTs and we used lower_bound and upper_bound

set,map => sorted order.

Lets review some of the basics of BSTs. // don't ask me why its not on top.

Date Models : usually the most of data we encounter is multidimensional so for such a multidimensional data we use identifiers ( like primary keys).

so its exhibits a key , value model

key - integer and value can be anything from primitive data to struct.

all maps and sets are identical (internal implementation) and analogues with each other except in their use case , so maps and sets are implemented using BSTs.

There is a important point of difference that is how they differentiate items

- Maps use *key* for item comparisons ( ordering )
- Sets use *data* for comparison ( ordering )

Similarly unordered_maps and unordered_sets

- unordered_maps use *key* for hashing
- unordered_sets use entire data for hashing

#### BSTs (Ordered Search Structures)

- Insertion ($O (\log n)$)
- Deletion ($O (\log n)$)
- Search ($O (\log n)$)
- lower_bound ($O (\log n)$)
- upper_bound ($O (\log n)$)



BST have a property that the root node is always less than the all the element of tree rooted at the left children of the root and greater than equal to all the elements of tree rooted at the right children of the root.



Search  : exact simulation of binary search

Upper Bound : do exact same step as execute search and for each node that you visit and keep checking

````c++
if(n.value > I){
    if(n.value - I < min-diff){
        min_diff = n.val-I;
        ans = n.val;
    }
}
````

Basically we are trying to find the minimum difference at each node to take branching decision.

Terminology .

Path (s,e) : paths that start for the root and end at a leaf node.

Height of a Tree : length of longest path.

#### Review of What we have learnt till now with some design based problems.

[Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/) : 

Note : here we are getting time stamps in increasing order of time.

unordered_map<int , vector<pair<int,int>>> u;

so we can run binary search on vector to find upper bound of timestamp.

````c++
unordered_map<string , vector<pair<int,string>>> u;
TimeMap() {}
void set(string key, string value, int timestamp) {
    u[key].push_back({timestamp,value});
}
string get(string key, int timestamp) {
    vector<pair<int,string>>&v = u[key];
    // FF*TTT
    // p(x) : x > timstamp
    // last F
    int lo = 0,hi = v.size()-1,mid;
    while(lo < hi){
        mid = lo + (hi-lo+1)/2;
        if(v[mid].first > timestamp)
            hi = mid-1;
        else
            lo = mid;
    }
    if(v[lo].first <= timestamp)
        return v[lo].second;
    return "";
}
````

So what will you do if timestamps are not in increasing order. :) use BSTs instead of vector.

[LRU Cache](https://leetcode.com/problems/lru-cache/) : 

Similar problem but timestamps are not fixed since `get` updates the timestamp.

We need a notion of time and we need to update time when we do get operation.

we want to do 2 operation efficiently

- find the position of key in the list
- delete + insert at the beginning

unordered_map < key , value > s.t. key helps us look up fast and value will be pointer

to the item in list. So we can efficiently delete and insert.

We can either swap that node with heap and replace it or we can keep a doubly linked list that can help us cut the node out of list

`list < >` in STL is by default doubly linked list.

`list <key>` that is enough for our problem.

`unordered_map<int , pair <value,pointer >> u;`

````c++
list<int> l;
unordered_map<int, pair<int, list<int>::iterator>> m;
int c;
LRUCache(int capacity) {
    c = capacity; 
}
void update(int key){
    // Erase
    l.erase(m[key].second); 
    // insert at the head
    l.push_front(key);
    // update the reference in the map
    m[key].second = l.begin();
}

int get(int key) {
    if(m.find(key) == m.end())
        return -1;
    int res = m[key].first;
    // update this to the first
    update(key);
    return res;
}

void put(int key, int value) {
    // update
    if(m.find(key) != m.end()){
        update(key);

        //update the value
        m[key].first = value;
    }
    else {
        if(l.size() < c){
            // brand new insert
            // cache is not full
            // insert at the head of the list
            l.push_front(key);
            // insert in the map
            m[key] = make_pair(value,l.begin());
        } else {
            // cache is full
            // remove the LRU entry
            int k = l.back();
            // remove from the list
            l.pop_back();
            // remove from the map
            m.erase(k);

            // insert 
            // insert at the head of the list
            l.push_front(key);
            // insert in the map
            m[key] = make_pair(value, l.begin());

        }     

    }
}
````


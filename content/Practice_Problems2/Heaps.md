+++

title = "Heaps"
date = 2021-05-17T12:25:25+05:30
weight = 2

+++

#### Heap

Partially ordered tree complete tree and usually have 2 children.

Top on the max heap is maximum element and in case of min heap top element is minimum of all.

It gives Min/Max query in $O(1)$ time.

STL implementation

```c++
// Max Heap
priority_queue<int> maxq;
// Min Heap
priority_queue<int, vector<int>, greater<int>> minq;
```

[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

````c++
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int> pq;
    for(int i = 0; i < nums.size(); i++)
        pq.push(nums[i]);
    k--;
    while(k--)
        pq.pop();
    return pq.top();
}
````

[Merge K sorted List](https://leetcode.com/problems/merge-k-sorted-lists/)

Using Min Heap

````c++
typedef ListNode* link;
ListNode* mergeKLists(vector<ListNode*>& lists) {
    // Using MinQ
    priority_queue<int,vector<int>, greater<int>> minq;
    for(link list:lists){
        link head = list;
        while(head != NULL){
            minq.push(head->val);
            head = head->next;
        }
    }
    // Recreate the sorted list
    link root = NULL;
    if(minq.size() == 0) return root;
    root = new ListNode(minq.top());
    minq.pop();
    link res = root;
    while(minq.size()){
        link temp = new ListNode(minq.top());
        root->next = temp;
        root = root->next;
        minq.pop();
    }
    return res;
}
````

[Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)

We should focus on maintaining the order of ugly numbers because most of numbers are not ugly.

Assume we have $U_k$ as $k^{th}$ ugly number then $U_{k+1}$ must be min($L_1 *2, L_2 *3, L_3 *5$)

````c++
typedef long long int ll;
int nthUglyNumber(int n) {
    priority_queue<ll,vector<ll>,greater<>> minq;
    minq.push(1);
    if(n==1) return 1;
    ll cnt = 1;
    while(cnt < n){
        ll temp = minq.top();
        minq.pop();
        if(temp%2 == 0){
            minq.push(temp*2);
        } else if(temp%3 == 0){
            minq.push(temp*2);
            minq.push(temp*3);
        } else {
            minq.push(temp*2);
            minq.push(temp*3);
            minq.push(temp*5);
        }
        cnt++;
    }
    return minq.top();
}
````

<hr>

[Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

Classic problem!

Maintain 2 Heaps Max heap and min heap for i the object and try to maintain the heap size to be equal. This implementation is leaned toward maxheap! i.e. size of maxheap <= size of minheap

````c++
/** initialize your data structure here. */
priority_queue<int> maxq;
priority_queue<int, vector<int>, greater<int>> minq;
MedianFinder() {}

void addNum(int num) {
    if(maxq.size() == minq.size()){
        minq.push(num);
        maxq.push(minq.top());
        minq.pop();
    } else {
        maxq.push(num);
        minq.push(maxq.top());
        maxq.pop();
    }
}
double findMedian() {
    if(maxq.size() == minq.size())
        return (double)(maxq.top()+minq.top())/2;
    return maxq.top();
}
````



[K closest points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

Priority Queue Based solution!

````c++
typedef vector<int> point;
struct compare{
    bool operator()(point a, point b){
        return a[0]*a[0] + a[1]*a[1] > b[0]*b[0] + b[1] * b[1];
    }
};
vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
    // sort(start, end, compare);
    priority_queue< point, vector<point>, compare> minq(points.begin(), points.end());
    vector<point> res;
    while(k--){
        res.push_back(minq.top());
        minq.pop();
    }
    return res;
}
````

Solution 1 linear :D

````c++
vector<vector<int>> kClosest(vector<vector<int>>& A, int K) {
    partial_sort(A.begin(),A.begin()+K,A.end(),[](vector<int>& a,vector<int>& b){
        return a[0]*a[0]+a[1]*a[1] < b[0]*b[0] + b[1]*b[1] ; 
    });  
    return vector< vector<int >> (A.begin(),A.begin()+K);
}
````

[Top K frequent words](https://leetcode.com/problems/top-k-frequent-words/)

````c++
typedef pair<string,int> pp;
struct compare{
    bool operator()(pp a, pp b){
        if(a.second == b.second)
            return a.first > b. first;
        else 
            return a.second < b.second;
    }
};
vector<string> topKFrequent(vector<string>& words, int k) {
    unordered_map<string,int> umap;
    // calculate the freq of each word
    for(string s: words) umap[s]++;

    priority_queue<pp,vector<pp>,compare> maxq;
    // push map into max heap
    for(auto itr : umap){
        maxq.push({itr.first, itr.second});
    }
    vector<string> res;
    // push top k frequent string
    while(k--){
        res.push_back(maxq.top().first);
        maxq.pop();
    }
    return res;
}
````

[Minimum Cost to Hire K workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)


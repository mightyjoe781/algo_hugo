+++

title = "Sorting"
date = 2021-05-16T17:01:25+05:30
weight = 1

+++

#### Sorting Algorithm

- Algorithms which puts elements of list in order/arrange
  - Numerical Order
  - Lexicographical Order
  - Custom Object
- 4 parameter's of evaluation of sorting algorithm
  - Computational Complexity
  - Stability : Relative order will be same
  - Memory Usage
  - Adaptability : preparedness of input array to get least running time.
- Bubble Sort
  - $O(n^2)$ , best case : sorted , worst case : reverse order, stable, In-place
- Selection Sort
  - $O(n^2)$ , unstable ( can be made stable) , In-place
- Insertion Sort
  - $O(n^2)$ , idea is we keep two sets one is sorted and another unsorted set and goal is to move unsorted elements slowly towards sorted set.
  - Stable (depends on ordering) , In-place
  - little faster than selection sort.
- generally Bubble < Selection < Insertion

#### Custom Comparator

STL 

sort(start_iterator, end_iterator, compare_function)

This compare_function is how comparisons will be done.

````
bool compare(a,b)
	return a<b;
````

This way we can change sorting order :)

[Custom Sort String](https://leetcode.com/problems/custom-sort-string/)

````c++
class Solution {
public:
    static string tmp;
    static bool compare(char a, char b){
        return tmp.find(a) < tmp.find(b);
    }
    string customSortString(string S, string T) {
        tmp = S;
        sort(T.begin(),T.end(),compare);
        return T;
    }
};
string Solution::tmp;
````

Notice how `tmp` is define globally because its static! 

or we could make it global

This shows lambda function implementation. Note here we could have passed string `s` directly in scope. `[]`

````c++
string customSortString(string S, string T) {
    sort(T.begin(),T.end(),[&S](const char a, const char b){
        return S.find(a) < S.find(b);
    });
    return T;
}
````

[Squares of a sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

````c++
vector<int> sortedSquares(vector<int>& nums) {
    sort(nums.begin(), nums.end(),[] (const int a, const int b){
        return abs(a) < abs(b);
    });
    for(auto &x : nums)
        x = (x*x);
    return nums;
}
````

[Sort Character By frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

````c++
string frequencySort(string s) {
    int freq[256] = {0};
    for(char c:s) freq[c]++;
    sort(s.begin(),s.end(),[&freq](const char a, const char b){
        if(freq[a] == freq[b]) return a < b;
        return freq[a] > freq[b];
    });
    return s;
}
````

Lambda function have

- [] Scope
  - putting `[=]` means take up the entire scope
  - otherwise pass the variables to be utilized
- () parameters
- {} definition

[Largest Number](https://leetcode.com/problems/largest-number/)

````c++
string largestNumber(vector<int>& nums) {
    sort(nums.begin(), nums.end(),[=](const int a, const int b){
        return to_string(a)+to_string(b) > to_string(b)+ to_string(a);
    });

    string res = "";
    if(nums[0] == 0) return "0";
    for(int i:nums) res+=to_string(i);
    return res;
}
````

[Rank Teams by Votes](https://leetcode.com/problems/rank-teams-by-votes/)



<hr>

Sorting Algorithms

- Comparisons Based (Best Case $O(n\log n)$)
  - Bubble
  - Insertion
  - Selection
  - Mergesort
  - Quicksort
- Non Comparison Based (Best Case $O(n)$)
  - Counting
  - Bucket
  - Radix

#### Counting Sort

Whenever input is a small range. It offers $O(n)$ at the cost of extra space. Can we preserve the stability : ? We could use hash or vector of linked list.

Create a frequency DAT and just traverse the DAT and print ! 

#### Bucket Sort

Bucket the number -> segment # in equal portions

[1 ... k] -> finite series and number are sampled equally.

make buckets and then internally sort within each bucket and keep pushing result in the auxiliary vector!

Which data structures is preferred for buckets ? key , value (list) :)

Now since bucket size is fixed we can use internal sorting for bucket sorting.

Worst Case : $O(n^2)$ Uneven distribution case! insertion + internal sorting + redistribution

Best Case : $O(n)$ Even distribution ( one number in each bucket)

#### Radix Sort

Sorting based on LSB - > MSB :). It is covered extensively in the section 3 sorting quite extensively.

 #digits should be finite in nature

limited padding.

Time Complexity  : n*k ~ O(n)	

Space Complexity : $O(n)$

- Padding 
  - Integers : 0 from LHS
  - String/Char : A from RHS

[Maximum Gap](https://leetcode.com/problems/maximum-gap/)

Linear Time Complexity indicates that its not a comparison based sorting.

Sort the numbers - > sort (radix sort)

counting sort -> because range of no. id too high

Find the max diff.

First of all we are calculating how many times we have to run the loop ( size of maximum bit traversal ). `(maxn/bit)` or use `log n` :)

we are getting last n bits of the given `nums` and then we are doing counting sort!

and we need to sort the original array according to sort of last digit!

`(nums[i]/bit)%10` gives the iteration bit :) and we go LSB to MSB

Also note the cumulative sum being used to get the ranking of the bit while finding its order ! This ranking vector will remain and persist while entire sorting!

````c++
int maximumGap(vector<int>& nums) {
    int n = nums.size();
    int aux[n];
    int maxn = *max_element(nums.begin(), nums.end());
    int bit = 1;

    while(maxn/bit > 0){
        // capture the bit
        int cnt[10] = {0};

        for(int i = 0; i < n; i++) cnt[(nums[i]/bit)%10]++;
        // implementing counting sort 
        // cummulative sum
        for(int i = 1; i < 10; i++) cnt[i]+=cnt[i-1];
        // put back numbers in sorted order for that iteration
        for(int i = n-1; i >= 0; i--){
            aux[cnt[(nums[i]/bit)%10]-1] = nums[i];
            cnt[(nums[i]/bit)%10]--;
        }
        // put back number in nums array
        for(int i = 0; i < n; i++) nums[i] = aux[i];
        // iteration to increase bit!
        bit = bit*10;
    }
    // finding the maximum difference!
    int res = 0;
    for(int i = 1; i < n; i++){
        res = max(res,nums[i]-nums[i-1]);
    }
    return res;
}
````

Suggested to dry to get better understanding of the problem!

[Merge Intervals](https://leetcode.com/problems/merge-intervals/)

Classic Problem :D

sort the intervals on basis of start time and check whether `curr[end] > next[start]` to decide intersecting intervals.

````c++
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    // sort wrt start time
    sort(intervals.begin(), intervals.end(),[](vit a,vit b){
        return a[0] < b[0];
    });

    // iterate thru intervals
    int n = intervals.size();
    vector<vit> res;
    vit prev = intervals[0];

    // start merging intervals
    for(int i = 1; i < n; i++){
        vit curr = intervals[i];
        // if there is overlap
        if(prev[1] >= curr[0]){
            // update the endtime of current
            prev[1] = max(prev[1],curr[1]);
        } else {
            res.push_back(prev);
            prev = curr;
        }
    }
    //push the last intervals
    res.push_back(prev);
    return res;
}
````

<hr>

[Non-Overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

````c++
typedef vector<int> vit;
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    // sort wrt end time
    sort(intervals.begin(), intervals.end(),[](vit a, vit b){
        return a[1] < b[1];
    });
    // take the first interval
    int n = intervals.size();
    int cnt = 0;
    if(n == 0) return 0;

    int end = intervals[0][1];
    // iterate thru all other intervals (start with 2nd one)
    for(int i = 1; i<n; i++){
        vit v = intervals[i];
        // conflict -> resolve using end first criteria
        // no overlap
        if(end <= v[0]) end = v[1];
        else {
            end = min(v[1],end);
            cnt++;
        }
    }
    return cnt;
}
````

#### Problems on Merge Sort

[Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

In-place sorting and we do this from behind of the array A.

````c++
void merge(vector<int>& A, int m, vector<int>& B, int n) {
    int i = m-1;
    int j = n-1;
    int k = m+n-1;
    while(i>=0 && j>=0){
        if(A[i] > B[j]) A[k--] = A[i--];
        else            A[k--] = B[j--];
    }
    while(j>=0)         A[k--] = B[j--];
}
````

[Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

````c++
typedef vector<int>::iterator vitr;
int cnt = 0;

void merge_sort(vitr start, vitr end){
    if(end-start <= 1) return;
    vitr mid = start + (end-start)/2;
    merge_sort(start,mid);
    merge_sort(mid,end);
    // combine
    for(vitr i = start, j = mid; i < mid; i++){
        // if I is greater than 2j then i+1 will also be
        // greater than Js encountered so far
        while(j < end && *i > 2L* (*j)) j++;
        // so keep on adding previous Js into subsequent calls
        cnt += (j-mid);
    }
    inplace_merge(start,mid,end); // STL function
}
int reversePairs(vector<int>& nums) {
    merge_sort(nums.begin(),nums.end());
    return cnt;
}
````

[Counts of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

Same code can be reused! :D

````c++
typedef vector<pair<int,int>>::iterator pit;
vector<int> res;

void merge_sort(pit start, pit end){
    if(end-start <= 1) return;
    pit mid = start + (end-start)/2;
    merge_sort(start,mid);
    merge_sort(mid,end);
    for(pit i = start, j = mid; i < mid; i++){
        while(j < end && i->first > j->first) j++;
        res[i->second]+=(j-mid);
    }        
    inplace_merge(start,mid,end); // STL function
}

vector<int> countSmaller(vector<int>& nums) {
    vector<pair<int,int>> temp;
    for(int i = 0; i < nums.size(); i++){
        temp.push_back({nums[i],i});
        res.push_back(0);
    }
    merge_sort(temp.begin(),temp.end());
    return res;
}
````


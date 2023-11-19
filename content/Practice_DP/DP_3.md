+++

title = "DP 3"
date = 2021-04-22T08:43:25+05:30
weight = 14

+++

#### [LIS ( Longest Increasing Subsequence)](https://leetcode.com/problems/longest-increasing-subsequence/)

We were looking at subarrays till last article. now its subsequence (its not supposed to contiguous instead it maintains order of appearance )

1.  for every $s_i$ : LIS ending at i

res = max{$s_i$}

2. now how to find the $s_i$ <- Length of LIS ending at $i$

at any $j<i$  if $A[i]>A[j]$ we can extend the subsequence

3. $ S_i = 1 + max_{j\le i} {S_j} $ and $A[i]>A[j]$

Checking DP

4. Prefix Array with dimension 1, size <- n

5. Base Case

   dp[0] = 1

````c++
int lengthOfLIS(vector<int>& nums) {
    int n = nums.size(),i,j, res = 1;
    vector<int> dp(n,1);

    for(i = 1; i < n; i++){
        // compute dp[i]
        for(j = i-1; j >= 0; j--)
            if(nums[i] > nums[j])
                dp[i] = max(dp[i],1+dp[j]);
        res = max(res,dp[i]);
    }     return res;  }
````

#### [ Longest Arithmetic Subsequence](https://leetcode.com/problems/longest-arithmetic-subsequence/submissions/)

$S_i$ <- length of the largest Arithmetic sequence ending at i.

![image-20210422092137045](/DP_3.assets/image-20210422092137045.png)

common difference = $A[i] - A[j]$

for index $j$ we will store LAS (common difference$d$) ending at j

$j$ -> for every cd LAS ending at $j$

Extra Step...

Dimension -> 2D DP

Nature -> prefix arrays, common difference

Data Structure -> use unordered_map<int,int> : key -> cd , value -> length of longest chain.

````c++
int longestArithSeqLength(vector<int>& A) {
    int n = A.size(), i, j, cd, res = 0;
    vector<unordered_map < int, int >> dp(n);

    for(i = 0; i< n; i++){
        // compute dp[i]
        for(j = 0; j < i; j++){
            cd = A[i] - A[j];
            if(dp[j].find(cd) == dp[j].end())
                dp[i][cd] = 2;
            else
                dp[i][cd] =  1+dp[j][cd];

            res = max(res,dp[i][cd]); }
    }     return res;  }
````

Above gives TLE : one quick fix is the line after calculating cd , second way using a vector of 1000 size because its possible to get small common difference .

Maps were giving TLE because , maps are not always O(1) , instead its average case performance. 

````c++
int longestArithSeqLength(vector<int>& A) {
    int n = A.size(), i, j, cd, res = 0;
    vector<vector<int>> dp(n, vector<int> (1001,0));

    for(i = 0; i< n; i++){
        // compute dp[i]
        for(j = 0; j < i; j++){
            cd = A[i] - A[j];
            dp[i][cd+500] = max(2,1+dp[j][cd+500]);
            res = max(res,dp[i][cd+500]);
        }
    }     return res; }
````

#### [Word Break](https://leetcode.com/problems/word-break/)

All possible splits can be a calculated and some are valid and some are invalid splits.

so we can have splits at 0 , 1, 2 , ... n-1 giving $s_1, s_2, s_3, ... s_{n-1}$ 

"catsanddog" 

 0123456789

we can do a split [1,4,7]  or make [3,6,8] and more..

S : set of all possible splits.

1. Criteria for splitting, we can mark first split at 0 , 1, 2 , ...

   so s1 : split that has first split at 0 mark

   these splits are mutually exclusive and exhaustive in nature

2. $s_1$ : <- A[1...n-1] such that they all are in dictionary

   $s_2$ : <- A[2...n-1]

   Suffix Strings.

3.  We should have a split s.t. all words [0...i-1] for some s_i are in dictionary

   ans = OR{w[0...i-1] $\in$  D && $S_i$}

   S = $w_1|w_2| w_3| ... $

   D = {$w_1, w_2, w_1 w_2 $}

Is DP needed , 1D DP, suffix array n size

Order to fill.

for $j > i$ 

Yes ! 

````c++
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    int i, j, n = s.size();
    vector<bool> dp(n+1,false);
    dp[n] = true;
    string temp = "";
    for(i = n-1; i>=0 ; i--){
        //compute s_i
        temp = "";
        for(j = i; j <n; j++){
            temp+=s[j];
            if(dict.find(temp) == dict.end())
                continue;
            dp[i] = dp[i] || dp[j+1];

            if(dp[i])
                break;
        }
    }     return dp[0]; }
````

[Work-Break II] :
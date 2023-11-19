+++

title = "Recursion 1"
date = 2021-05-13T23:01:25+05:30
weight = 4

+++

### Recursion

- What / How -> First Part (Pre-read)
- Writing your own recursive solution (frameworks)

What is Recursion

- Function calling itself
- Divide and Conquer
- Mathematical Concept!

**Call by value**

Memory Layout

- Stack (local function data : arguments and inside data)
- Heap (dynamic like malloc, new)
- Global (global variable, code)

Flow of execution

- Main
- Stack is allocated for every functions

**Call by reference**

- Change the value at calls
  - Explicit return type ( use struct to create new data type :D)
  - Implicit return type
- Space Optimized
  - don't need extra memory declaration in memory stack.

##### [Subsets](https://leetcode.com/problems/subsets/)

In case of set there is no notion of order!

[2,3,5]

**Subsets** : Pick zero or more elements from above array in no order. e. g. {2}, {3,5}, {2,5} and {5,2}  are identical !

**Subarray** : A set of contiguous set from the array. e. g. {3,5} , {2,3}  are subarray while {2,5} and {5,2} are not subarray.

**Subsequences** : A set of elements of array taken in order. e. g. {2,5} is subsequence while {5,2} is not.

Given size $n$ array.

Number of Subsets   : $2^n$

Number of Subarray : $\frac{n(n-1)}{2}$ + 1 (empty :D)

Number of Subsequences : $2^n$

Lets take the Array as : [2,3,5]

Now if we looks at the solution set of the problem

1. We ask ourselves, Is it possible to divide the solution set using some criteria

- Disjoint ($c_1 \wedge c_2 = \phi$​ )
- Universal ($ c_1 \vee c_2 = U$​ )

- We can interpret solution sets as : $c_1$​ solution that includes first elements in subset, $c_2$​​ : solution that doesn't include the first element.

2. Define $c_1 \text{ and } c_2$​​ as smaller instance of original problem 

   - $c_1$ : number of subset of [3,5]
   - $c_2$ : any subsets of [3,5] and append 2 to it.

3.  Solution is suffix array.

   Define recurrence relation.

   `f(arr , 0, n-1) = f(arr, 1, n-1)`​​  $\vee$ `f(arr,1,n-1) + {2}`

4. Define Base Cases. (set containing 1 element)

   `f(arr, n-1, n-1) = {{}, {arr[n-1]}}`
   
   General framework of recursion code.

 ````c++
 vector<vector<int>> f(vector<int>& nums, int i, int end) {
     // Base Case.
     if(i == end){
         return {{},{nums[end]}};
     }
     // Recursive Step
     // Get C2 & C1
     // Get C2
     vector<vector<int>> c2 = f(nums,i+1,end);
 
     // Get C1
     vector<vector<int>> c1 = c2;
     
     for(int j = 0; j < c2.size(); j++)
         c1[j].push_back(nums[i]);
 
     // combine them
     vector<vector<int>> res = c2;
     for(int j = 0; j < c1.size(); j++)
         res.push_back(c1[j]);
 
     return res;
 }
 
 vector<vector<int>> subsets(vector<int>& nums) {
     return f(nums,0,nums.size()-1);
 }
 ````

The same thing can be solved using passing the contribution of current element down the function calls.

````c++
void f(vector<int>& nums, int i, int end, vector<int> contri, vector<vector<int>> & res){
    if(i == end){
        res.push_back(contri);
        
        contri.push_back(nums[end]);
        res.push_back(contri);
        return;
    }
    
    // Recursive
    // C2.
    f(nums,i+1,end, contri,res);
    
    // C1.
    // Contribution at this step is nums[i]
   	contri.push_back(nums[i]);
    f(nums,i+1,end, contri, res);
}
vector<vector<int>> subsets(vector<int>& nums){
    vector<vector<int>> res;
    
    // Implicit return
    f(nums, 0, nums.size()-1, {}, res);
    return res;
}
````

<hr>

[Combination](https://leetcode.com/problems/combinations/)

lets say : [1,2,3,4] k = 2

Solution set = {[1,2] , [2,3], [3,4], [1,4] , [2,4], [1,3] }

1. we can divide it into two chunks such that they contain 1 and doesn't contain 1

This division is mutually exclusive and exhaustive.

$c_1$ : includes first element

$c_2$ : doesn't include first element

2. Defining in terms of original problem

$c_2$ : all possible k sized subsets of P(Arr[1...n-1] , k)

$c_1$ : all possible $k-1$ sized subsets of P(Arr[1,n-1], k-1) with Arr[0] appended.

3. Defining Recurrence

Original Problem : P(arr, 0, n-1, k)

$c_1$ : P(arr,1,n-1,k-1)

$c_2$ : P(arr, 1, n-1, k )

4. Base Case

- start > end 
  - k = 0 (solution) : `{{}}`
  - k > 0 (no solution)
- start <= end
  - k = 0 -> `{{}}`

````c++
vector<vector<int>> f(int start, int end, int k){
    // Base Case.
    if(k == 0)
        return {{}};
    if(start > end)
        return {};
    
    // Recursive Step
    // C2
    // Doesn't include the first element
    vector<vector<int>> c2 = f(start+1,end,k);

    // C1
    // Include the first element
    vector<vector<int>> temp = f(start+1, end, k-1);

    // Append the first element
    vector<vector<int>> c1 = temp;
    for(int i = 0; i < temp.size(); i++)
        c1[i].push_back(start+1);

    // Merge
    // Res is c1 U c2
    vector<vector<int>> res = c2;
    for(int i = 0 ; i < c1.size(); i++)
        res.push_back(c1[i]);
    return res;
}
vector<vector<int>> combine(int n, int k) {
    return f(0,n-1,k);
}
````

Using contribution method

````c++
void f(int start, int end, int k, vector<int> contri, vector<vector<int>> & res ){
    // Base Case.
    if(k == 0){
        res.push_back(contri);
        return ;
    }
    if(start > end)
        return ;

    // Recursive Step
    // C2. Doesn't include the first element
    f(start+1,end,k, contri, res);

    // C1. Include the first element
    contri.push_back(start+1);
    f(start+1, end, k-1,contri, res);
}
vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> res;
    f(0,n-1,k,{},res);
    return res;
}
````

Above code has a lot of local copies of `contri` and increases memory usage :) but what about passing by reference.

we have to add one line of code to fix this :D delete the accumulated `contri` while passing it up to the caller back again.

Corrected and fixed code :D

````c++
void f(int start, int end, int k, vector<int>& contri, vector<vector<int>> & res ){
    // Base Case.
    if(k == 0){
        res.push_back(contri);
        return ;
    }
    if(start > end)
        return ;

    // Recursive Step
    // C2. Doesn't include the first element
    f(start+1,end,k, contri, res);

    // C1. Include the first element
    contri.push_back(start+1);
    f(start+1, end, k-1,contri, res);
    contri.pop_back();
}
vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> res;
    vector<int> contri;
    f(0,n-1,k,contri,res);
    return res;
}
````

[Combination Sum](https://leetcode.com/problems/combination-sum/)

1. Split solution into chunks

   $c_1$ : not including first element

   $c_2$ : contains at least one instance of first element

2. Smaller subproblem

   - $c_2$ : solution for `arr(1,n-1,sum)`
   - $c_1$ : solution for `arr(0,n-1,sum-A[0])`

3. Recurrence

   - P : `arr,0,n-1, sum`
   - c2 : `P(arr,1,n-1,sum)`
   - c1 : `P(arr,0,n-1, sum - A[0])`

4. Base Cases

   - if array becomes empty
     - sum > 0 : {}	 | np
     - sum = 0 : {{}}  | put in the result
     - sum < 0 : {}     | np
   - sum <= 0
     - sum == 0 : {{}} | put in the res
     - sum < 0 : {}      | np

````c++
void f(vector<int>& arr, int start, int end, int target, vector<int>& contri, vector<vector<int>>& res){
    //Base Setps
    if(target == 0) {
        res.push_back(contri);
        return;
    }
    if(target < 0 || start > end)
        return;
    
    //solve c2
    //not including first element
    f(arr,start+1,end,target,contri,res);
    //solve c1 include first element atleast once
    contri.push_back(arr[start]);
    f(arr,start, end, target-arr[start],contri, res);
    contri.pop_back();
}
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int>> res;
    vector<int> contri;
    f(candidates, 0, candidates.size()-1,target, contri, res);
    return res;
}
````

Pruning : `if(rem < k) return;` i. e. `if(end-start+1 < k) return;`

[Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

1. Split criteria : Sort the array
   - and then include the at least one instance of  first element(smallest)
   - don't include first element and jump to next unique number
2. Subproblems
3. Recurrence
4. Base Case

````c++
void f(vector<int>& candidates, int start, int end, int target,vector<int>& contri, vector<vector<int>>& res){
    //base step
    if(target == 0) {
        res.push_back(contri);
        return;
    }

    if( target < 0 || start > end) return;
    //recursive step

    //Not include the smallest ele. at all
    //find the first occurence of number > ar[start]
    int j = start + 1;
    while(j <= end && candidates[j] == candidates[start]) j++;

    f(candidates, j, end, target , contri , res);
    contri.push_back(candidates[start]);
    f(candidates,start + 1, end, target - candidates[start], contri, res);
    contri.pop_back();
}
vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    vector<int> contri;
    vector<vector<int>> res;
    vector<int> suffix(candidates.size(),0);
    sort(candidates.begin(), candidates.end());
    f(candidates, 0 , candidates.size() -1 , target, contri, res);
    return res;
}
````


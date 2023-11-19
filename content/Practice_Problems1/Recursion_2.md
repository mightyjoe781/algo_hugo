+++

title = "Recursion 2"
date = 2021-05-14T23:01:25+05:30
weight = 5

+++

##### Cues for Recursion/DP

- Combinatorial
  - Counting
  - Enumeration
- Optimization

Strategy to split

- Based on the first element of input (Inclusion / Exclusion)

Subproblems we saw was mostly

- Suffix Subarray

Number of suffix subarray : $n$ (including original one)

Dimension : 1 D

But for combination problem we had subproblem of size k and size k-1, dimension : $n*k$  (2D).

[Permutation](https://leetcode.com/problems/permutations/)

[1,2,3]

All permutations ?

Split solution into chunks on a split strategy

- n chunks
- begins with 1 {[1,3,2], [1,2,3]}
- begins with 2 {[2,1,3], [2,3,1]}
- begins with 3 {[3,1,2], [3,2,1]}

Subproblem : permutation calculation of the subarray removing the item.

- These are subsequences for array! not the suffix array

Representation for subproblem.

- Keeping a visited array will be helpful to represent the subproblems.
- Or we can always send the element not to be included to be swapped with the first element and that way we have a continuous suffix array as a subproblem

##### 1. Keeping visited array (Optimized)

````c++
void f(vector<int>& nums, vector<bool>& visited, vector<int>& contri, vector<vector<int>>& res){
    //Base Step
    //When everything is visited
    int i;
    for(i=0; i <nums.size(); i++){
        if(!visited[i])
            break;
    }
    if(i == nums.size()){
        res.push_back(contri);
        return;
    }
    //Recursive Step
    //Iterate over all possibilities for current position
    for( i = 0; i< nums.size(); i++)
    {
        //consider the ones that aren't visited
        if(!visited[i]) {
            //Passdown the contri
            //Try for this elt at the current position
            contri.push_back(nums[i]);
            visited[i] = true;
            f(nums,visited,contri, res);
            contri.pop_back(); // important step
            visited[i] = false;
        } 
    }
}
vector<vector<int>> permute(vector<int>& nums) {
    vector<bool> visited(nums.size(),false);
    vector<vector<int>> res;
    vector<int> contri;
    f(nums, visited, contri, res);
    return res;
}
````

##### Optimization on base case : :D

````c++
if(contri.size() == nums.size()){
    res.push_back(contri);
    return;
}
````

##### 2. Swapping based solution (creates explicit suffix array)

````c++
void f(vector<int>& nums,int j, vector<int>& contri, vector<vector<int>>& res){
    //Base Step
    //When everything is visited
    if(j == nums.size()){
        res.push_back(contri);
        return;
    }

    //Recursive Step
    //Iterate over all possibilities for current position
    for(int i = j; i< nums.size(); i++)
    {
        //consider the ones that aren't visited
        //Passdown the contri
        //Try for this elt at the current position
        contri.push_back(nums[i]);
        swap(nums[j],nums[i]);
        f(nums,j+1,contri, res);
        //undo operation to maintain tree validity
        contri.pop_back();
        swap(nums[j],nums[i]);
    }
}
vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int> contri;
    f(nums, 0, contri, res);
    return res;
}
````

[Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

Split strategy :

- divide into n chunks
- set of solution where first partition(a) "a|a|b" 
- set of solution where first partition(aa) "aa|b"
- set of solution where first partition(aab) "$\phi$" // prefix is not palindrome
- ....

Subproblem

$c_i$ : All the palindrome partition of elements of array after $i$ and then append palindrome before $c_i$ 

Its Suffix array problem.

1 D subproblem.

`P(arr, i)`

````c++
bool isPalindrome(string& s){
    int start= 0, end = s.size()-1;
    while(start <= end){
        if(s[start] != s[end])
            return false;
        start++,end--;
    }
    return true;
}
void f(string& s, int start, vector<string>& contri, vector<vector<string>>& res){
    //Base case
    if(start == s.size()){
        res.push_back(contri);
        return;
    }

    //Try all possibilities of putting up the partition
    string part;
    for(int j = start; j < s.size(); j++){
        // s[start .. j] is the first partition
        part = s.substr(start, j-start+1);

        if(!isPalindrome(part))
            continue;
        contri.push_back(part);
        f(s,j+1, contri, res);
        contri.pop_back(); // clear parent call! important
    }
}
vector<vector<string>> partition(string s) {
    vector<vector<string>> res;
    vector<string> contri;
    f(s,0,contri,res);
    return res;
}
````

[Letter combination of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

"34871"

for 3 : there are 3 chunks 

chunks that contain only d , e , f

then the problem is precisely same for the next `arr(1,n-1)` , suffix array!

````c++
vector<string> digitToAlphabet;
void initializeMap(){
    digitToAlphabet.resize(10);
    digitToAlphabet[2] = "abc";
    digitToAlphabet[3] = "def";
    digitToAlphabet[4] = "ghi";
    digitToAlphabet[5] = "jkl";
    digitToAlphabet[6] = "mno";
    digitToAlphabet[7] = "pqrs";
    digitToAlphabet[8] = "tuv";
    digitToAlphabet[9] = "wxyz";
}
void f(string& digits, int start, string& contri, vector<string>& res){

    //Base Case
    if(start == digits.size()){
        res.push_back(contri);
        return;
    }

    // recursion
    for(int i=0; i< digitToAlphabet[digits[start] -'0'].size(); i++){
        contri.push_back(digitToAlphabet[digits[start] - '0'][i]);
        f(digits,start+1,contri,res);
        contri.pop_back();
    }
}
vector<string> letterCombinations(string digits) {
    if(digits.size() == 0) return vector<string>();
    vector<string> res;
    string contri;
    initializeMap();
    f(digits,0,contri,res);
    return res;
}
````


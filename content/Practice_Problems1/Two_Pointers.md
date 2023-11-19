+++

title = "2 Pointers"
date = 2021-05-10T17:01:25+05:30
weight = 7

+++

##### Introduction

- Not a DS or Algorithm
- Is a technique to solve non-linear function ($O(n^2),O(n^3)$) and make it into a linear solution $O(n)$

- Works on linear data structure only : Arrays and Linked lists.

##### Approach

- Index based 2 pointers
- Windows Based Technique
- Slow-fast pointer

##### Sorted Array : n

Find a pair(a,b) -> sum to number $x$

Brute Force : $O(n^2)$

2 pointer approach.

Simple put two pointers, at start and end respectively. Now if sum > sum of numbers at pointers then do end-- otherwise start++ :D

##### [Remove Duplicated from a Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Two pointers

- P1 : keeps running 
- P2 : reserved pointer : captures all unique values.

````c++
int removeDuplicates(vector<int>& nums) {
    if(nums.size() == 0) return 0;
    int p1 = 0;
    int p2 = 1;
    while(p2 < nums.size()){
        if(nums[p1] == nums[p2]) p2++;
        else swap(nums[++p1],nums[p2++]);
    }
    return p1+1;
}
````

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Calculate a prefix and suffix array and take product of left and right elements :D

[Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

````c++
int maxArea(vector<int>& height) {
    int n = height.size();
    // begin
    int p1 = 0; 
    // end;
    int p2 = n-1;
    int maxVol = 0;
    while(p1<p2){
        maxVol = max(min(height[p1], height[p2])*(p2-p1),maxVol);
        if(height[p2] > height[p1]){
            int curr_ht = height[p1];
            p1++;
            while(p1<p2 && height[p2] <=curr_ht) p1++;
        } 
        else{
            int curr_ht = height[p2];
            p2--;
            while(p1<p2 && height[p2] <=curr_ht) p2--;
        }
    }
    return maxVol;
}
````

[3Sum](https://leetcode.com/problems/3sum/)

````c++
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> ans;
    for(int i = 0;i < nums.size(); i++){
        // Reaching the unique element
        if(i>0 && nums[i] == nums[i-1])
            continue;
        int l = i+1, r = nums.size()-1;
        while(l<r){
            int s = nums[i]+nums[l]+nums[r];
            if(s>0) r--;
            else if (s<0) l++;
            else {
                ans.push_back(vector<int> {nums[i],nums[l]
                    ,nums[r]});
                // updating l and r to get to next solution
                while(l+1 < r && nums[l] == nums[l+1]) l++;
                while(r-1 > l && nums[r] == nums[r-1]) r--; 
                l++; r--;
            }
        }

    }
    return ans;
}
````

<hr>
##### Sliding window

Text : S, Pattern : P

P : Co-relation criteria => $f(P_1, P_2)$

There is always a sliding windows state = $[P_1, P_2]$ which will correspond to the solution.

[Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

This is a very generic problem on sliding window.

Text : abcdeabcdab

Pattern : abc

we want to create a window but we can't list all probabilities and check if it is one of the anagram! Expensive Approach in terms of time complexity.

Create the frequency array of Pattern and keep count which will decide exit criteria.

a : 1 , b : 1 , c : 1 and count = 3

now we check and we find that c is part of map and we want to decrease it and count too.

a : 1 , b : 1 , c : 0 and count = 2

....

a : 0, b : 0 , c : 0 and count = 0 (matched condition and let's move to next window)

But remember to to increase frequency of elements again ! 

````c++
vector<int> findAnagrams(string s, string p) {
    // hash map
    vector<int> hash(256,0);
    // fill up freq table
    for(char c:p) hash[c]++;
    vector<int> res;
    int p1 = 0, p2 = 0, count = p.size();

    while(p2 < s.size()){
        // logic of adding element to window
        if(hash[s[p2]] > 0 ) count--;
        hash[s[p2]]--;
        p2++;

        //exit criteria for a window  -> count == 0
        if(count == 0) res.push_back(p1);

        //logic while removing the element
        if(p2-p1 == p.size()){
            if(hash[s[p1]] >= 0) count++;
            hash[s[p1]]++;
            p1++;
        }

    }
    return res;
}
````

[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

Same as previous question but here exit criteria changes.

````c++
string minWindow(string s, string t) {
    vector<int> hash(256,0);
    // populate the hash table
    for(char c:t) hash[c]++;
    int p1 = 0, p2 = 0;
    int count = 0;
    // Head will be the start of resulted substring
    // minlength as the length of resultant substring
    int head = 0, minLength = INT_MAX;
    while(p2 < s.size()){
        // adding logic
        if(hash[s[p2]] > 0) count++;
        hash[s[p2]]--;
        p2++;

        // exit criteria
        // find the minmum substring length
        while(count == t.size()){
            if(p2-p1 < minLength){
                minLength = p2-p1;
                head = p1;
            }
            // remove logic
            if(hash[s[p1]] == 0) count--;
            hash[s[p1]]++;
            p1++;
        }
    }
    if(minLength == INT_MAX) return "";
    else return s.substr(head,minLength);
}
````



[Longest Substring without Repeating](https://leetcode.com/problems/longest-substring-without-repeating-characters/)


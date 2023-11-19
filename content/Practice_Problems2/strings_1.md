+++

title = "Strings 1"
date = 2021-07-09T12:25:25+05:30
weight = 5

+++

Contents :

- Implementation
  - Encoding/Decoding
  - Compression
  - Palindromes
  - Parsing
- Tries
  - Design Problems
- String Matching Algorithms
  - KMP
  - Application (failure)

[Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

````c++
bool isAlphaNumeric(char c) {
    c = tolower(c);
    return ((c >= 'a' && c <= 'z') || (c >= '0' && c <= '9'));
}
bool isPalindrome(string s) {
    int start = 0, end = s.size()-1;        
    while(start < end) {
        if(!isAlphaNumeric(s[start])) start++; 
        else if(!isAlphaNumeric(s[end])) end--;
        else if(tolower(s[start]) != tolower(s[end])) return false;
        else {
            start++; end--;
        }
    }
    return true;
}
````

[Valid Palindrome-II](https://leetcode.com/problems/valid-palindrome-ii/)

Find a first mismatch and we can delete either one of the characters.

if we delete $i$ we need `isPalindrome(i+1, j)` or if we delete $j$ then check `isPalindrome(i,j-1)`

````c++
bool isPalindrome(string &s, int start, int end) {
    while(start <= end && s[start] == s[end]) {
        start++; end--;	}
    return (start > end);
}
bool validPalindrome(string s) {
    int n = s.size(), start = 0, end = n-1;
    while(start <= end && s[start] == s[end]) {
        start++; end--;	}
    if(start > end) return true;
    return (isPalindrome(s,start+1,end) || isPalindrome(s,start,end-1));
}
````

[Encode and decode Strings](https://leetcode.com/problems/encode-and-decode-strings/)

encode the array as ["hello","my", "name", "is","smk"] `5#hello2#my#4name2#is3#smk`

````c++
string encode(vector<string>& strs) {
    string enc = "";
    for(int i = 0; i < strs.size(); i++){
        enc += to_string(strs[i].size()) + '#' + strs[i];
    }
    return enc;
}
vector<string> decode(string s) {
    vector<string> dec;
	int i = 0;
    while(i < s.size()) {
        // Get the first occurence of #
        int pos = s.find('#', i);
        int count = stoi(s.subtr(i, pos-i));
        pos++;
        // push a string
        dec.push_back(s.substr(pos,count));
        
        // Update i
        i = pos+count;
    }
    return res;
}
````

[String Compression](https://leetcode.com/problems/string-compression/)

Run length compression. Try to use the same string you have been given.

````c++
int compress(vector<char>& chars) {
    int n = chars.size(), i, j, curr_char, curr_count;
    curr_char = chars[0];
    curr_count = 1;

    // j stores the first position where I can insert.
    j = 0;

    for(i = 1; i <= n; i++) {
        if(i < n && chars[i] == curr_char)
            curr_count++;
        else {
            // time to insert into the same strings
            // Insert the char
            chars[j] = curr_char;
            j++;
            // Insert the count
            if(curr_count > 1) {
                string count_str = to_string(curr_count);
                int len = 0;

                while(len <count_str.size()) {
                    chars[j] = count_str[len];
                    j++;
                    len++;
                }
            }
            // Update curr_char and curr_count for the next iteration.
            if(i < n) {
                curr_char = chars[i];
                curr_count = 1;
            }
        }
    }
    return j;
}
````

[Add Binary](https://leetcode.com/problems/add-binary/)

in adding we add and then take the number's modulo with base and put remainder there and carry the quotient over to next digit.

#### Pattern Matching

[Implementing strStr](https://leetcode.com/problems/implement-strstr/)

Find the needle in the haystack , a very well known historical problem.

A simple brute force solution is $O(mn)$ where we match needle for every character of haystack.

Can we avoid some computation. We will try to find longest prefix which is also a suffix for T[0...i-1]. KMP :D

We define LPS array (aka $\pi(x) function $) : `LPS[i]` denotes longest proper prefix which is also a proper suffix for pattern `[0...i]` .

We will then compare `T[i]` with the `P[LPS[j-1]]` and we can update `j = LPS[j-1]`

````c++
int strStr(string haystack, string needle) {
    int n = needle.size(), i, j;
    
    if(n == 0) return 0;
    
    vector<int> lps(n,0);
    // Filling the lps
    for(int i = 1; i < n; i++) {
        j = lps[j-1];
        
        while(j > 0 && needle[j] != needle[i]) {
            j = lps[j-1];
        }
        if(needle[j] == needle[i])
            lps[i] = j+1;
        else
            lps[i] = 0;
    }

    // Actual KMP
    // I have the lps function
    // lps[i] <-- longest proper prefix which is also a suffix for pattern[0...i]
    i = 0, j = 0;
    while(i < haystack.size()) {
        // Find first mismatch
        while(j < n && i < haystack.size() && haystack[i] == neddle[j]) {
            i++; j++;
        }
        // if pattern found
        if(j == n)
            return (i-n);
        // pattern not found
        // update j
        if(j == 0)
            i++;
        else
            j = lps[j-1];
    }
   
    return -1;
}
````

[Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)

We will add two strings of same type, and strip first and last characters of the resultant string and then search for `s` in that range again.

````c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string t = s + s;
        string tprime = t.substr(1,t.size()-2);        
        int idx = tprime.find(s);
        
        if(idx == string::npos)
            return false;
        return true;
    }
};
````

[shortest Palindrome](https://leetcode.com/problems/shortest-palindrome)

Concatenate string with its reverse.

and do lps array.

````c++
class Solution {
public:
    string shortestPalindrome(string s) {
        string srev = s;
        int i = 0, j = srev.size()-1;
        
        while(i < j) {
            swap(srev[i],srev[j]);
            i++;j--;
        }
        
        string t = s + "#" + srev;
        int n = t.size();
        
        // Find out the lps
        vector<int> lps(n,0);
        
        lps[0] = 0;
        for(i = 1; i < n; i++) {
            j = lps[i-1];
            while(j > 0 && t[j] != t[i])
                j = lps[j-1];
            
            if(t[j] != t[i])
                lps[i] = 0;
            else
                lps[i] = j+1;
        }
        
        string append = srev.substr(0,s.size()-lps[n-1]);
        return append + s;
    }
};
````

##### Main steps of KMP problems

1. Preprocessing : Calculating LPS (Longest Prefix Suffix) Array

2. Main Logic : 


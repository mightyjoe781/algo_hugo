+++

title = "Bit Manipulation"
date = 2021-07-07T07:02:25+05:30
weight = 4

+++

#### Number System

- Representation of Number.
- Numbers are set of finite entities/symbols used for quantification.
- Several prominent examples are Decimal, Roman, Binary, Hexadecimal and more.

[Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

There are some definite symbols that indicate fixed numbers. Note how we translate number 4 in Roman. $IV$ means 5-1, while if $VI$ it is 5+1.

Problem reduces to just traversing the symbol and adding and subtracting according the priority.

````c++
class Solution {
public:
    map<char,int> mp;
    void init() {
        // Can't be initialised in global scope.
        mp['I'] = 1     ;
        mp['V'] = 5     ;
        mp['X'] = 10    ;
        mp['L'] = 50    ;
        mp['C'] = 100   ;
        mp['D'] = 500   ;
        mp['M'] = 1000  ;
    }
    
    int romanToInt(string s) {
        init();	// C++ requires a type specifier for all declaration.
        int n = s.size();
        int res = 0;
        for (int i = 0; i < n-1; i++) {
            // Get the corresponding value of symbol.
            if(mp[s[i]] >= mp[s[i+1]])
                res += mp[s[i]];
            else
                res -= mp[s[i]];
        }
        res += mp[s[n-1]];
        return res;
    }
};
````

Convert other way now :)

[Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

Note : Take a look at the upper bound given in description (4k).

````c++
class symbolMap {
public:
    symbolMap() {
        m.resize(1001);
        
        m[1] = "I";
        m[4] = "IV";
        m[5] = "V";
        m[9] = "IX";
        m[10] = "X";
        m[40] = "XL";
        m[50] = "L";
        m[90] = "XC";
        m[100] = "C";
        m[400] = "CD";
        m[500] = "D";
        m[900] = "CM";
        m[1000] = "M";
    }
    string getSymbol (int val) {
        return m[val];
    }
    vector<int> getAllValues() {
        return {1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000};
    }
private:
    vector<string> m;
};
class Solution {
public:
    string intToRoman(int num) {
        symbolMap sm;
        vector<int> vals = sm.getAllValues();
        string res = "";
        
        // Divide in decreasing order
        int m = vals.size();
        for(int i = m-1; i >= 0; i--) {
            int q = num/vals[i];            
            while(q--) {
                res += sm.getSymbol(vals[i]);
            }            
            num = num%vals[i];
        }
        return res;
    }
};
````

Practice : AND, OR , XOR operations.

AD HOC : Next Permutation of a sorted sequence.

Bitwise operator :

binary operator `AND &`, `OR |`, `XOR ^` 

unary operator `left shift <<` , `right shift >>`

 [Single Number](https://leetcode.com/problems/single-number/)

Note :  $a \oplus a =0$ and $a\oplus 0 = a$ .

so if there is any number which doesn't repeat twice will be left after doing cumulative xor on entire array.

````c++
int singleNumber(vector<int>& nums) {
    int res = nums[0];
    int n = nums.size();
    for(int i = 1; i < n; i++) 
        res ^= nums[i];
    return res;
}
````

[XOR Queries of a Subarray](https://leetcode.com/problems/xor-queries-of-a-subarray/)

Similar to maintaining a prefix array sum.

Typically Brute force will be O(nq) : ~ $10^8$ easy TLE

but with precomputation O(1) , and reasonable because only involves lookup queries.

````c++
vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
    int n = arr.size();
    vector<int> pre(n+1,0);
    int run = 0;
    for(int i = 0; i < n; i++)
        run ^= arr[i];
        pre[i+1] ^= run;

    int m = queries.size();
    vector<int> ans(m);
    for(int i = 0; i < m; i++)
        ans[i] = pre[queries[i][1]+1] ^ pre[queries[i][0]];
    return ans;
}
````

[Number of Steps to Reduce a Number in Binary Representation to One](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-in-binary-representation-to-one/)

Notes 

- Parity ( Odd : LSB = 1, Even USB = 0)
- Right shift by 1 is equivalent to dividing by 2
- Left shift by 1 is equivalent to multiplying by 2
- when adding 1, all bits from the position to the position of the first 0 are flipped

Simulation is not preferred for solution. Operate on the boolean representation.

Remove all zeroes until u see a one ( from right to left ~ removing even) as we see 1 we keep inverting until we see new 0 (adding 1).

Keep doing this. Again we have pruned above simulation but original problem is counting simulation not simulate.

Note : each zero from rightmost costs 1 operation, first immediate 1 costs 2 operation, all subsequent 1 will cost 1. Now next set of zeroes costs 2.

Now we abstracted simulation to counting problem.

- All 0s until we se "1"  +1
- for that "1" +2
- all 0s we see after "1" +2
- all 1s after we see a "1" +1

stop when number becomes 1.

````c++
int numSteps(string s) {
    int n = s.size(), i;
    bool one_seen = false;
    int first_one_idx = -1;
    int res = 0;
    for(i = n-1; i >=0; i--) {
        // if not seen a one
        if(!one_seen) {
            if(s[i] =='0')
                res++;
            else {
                res+=2;
                one_seen = true;
                first_one_idx = i;
            }
        }
        else {
            if(s[i] == '1')
                res++;
            else
                res+=2;
        }
    }
    return first_one_idx == 0 ? res-2:res;
}
````

### BIT Manipulation Tricks

- Check if j-th bit (from right) is set ` T = (S & 1<<j)`
- To set/turn on the j-th item `S|=(1<<j)`
- To clear/turn off the j-th item of set `S &= ~(1<<j)`
- To toggle the j-th item `S^= (1<<j)` 
- Get the least significant bit that is one (from right) `T=(S & (-S))`
- turn on all bits in a set of size n, `S=(1<<n)-1`

**Additional functions**

The g++ compiler provides the following functions for counting bits:

- `__builtin_clz(x)` : # zeros at the beginning (count leading zeros)
- `__builtin_ctz(x)` : # zeros at the end (count trailing zeros)
- `__builtin_popcount(x)` : # ones in a number.
- `__builtin_parity(x)` : # the parity (even/odd) of the number of ones.



[Minimum flips to make a OR b Equal to c](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/)

a OR b = c 

Here are there are 2 cases for each bit

- a_ bit | b_ bit == c_bit : Don't do anything when OR of ith bit is same

- OR is different so we check c_bit

  - c_bit = 1 we ADD 1

  - c_bit = 0 then we see the table

    | c_bit | a_bit | b_bit | cost |
    | ----- | ----- | ----- | ---- |
    | 0     | 0     | 1     | 1    |
    | 0     | 1     | 0     | 1    |
    | 0     | 1     | 1     | 2    |
    | 1     | 0     | 0     | 1    |

  - Simply a_bit & b_bit == 1(this is wrong, remember what ai & bi is :) cost : 2 else cost 1.

````c++
int minFlips(int a, int b, int c) {
    // Iterate bit by bit
    int i, ai, bi, ci;
    int res = 0;
    for(i = 0; i < 32; i++) {
        ai = (1<<i)&a;
        bi = (1<<i)&b;
        ci = (1<<i)&c;

        if((ai | bi) == ci)
            continue;
        if((ai & bi) > 0) // hmm :)
            res += 2;
        else
            res++;
    }
    return res;
}
````

[Single Number II](https://leetcode.com/problems/single-number-ii/)

Represent the numbers in their bit representation

![image-20210709073008743](/BitManipulation.assets/image-20210709073008743.png) 

Number of 1s in the ith bit. so 1 bit will be of form 3k and its 3k + 1 (number than appears once)

Count divisible by 3 $c_i$ = 0, otherwise $c_i$ = 1

this way we can generate $c_i$ using the division on every ith bit :)

Bitwise trick :) set the ith bit.

````c++
int singleNumber(vector<int>& nums) {
    int c = 0, i, j, count, ci;

    // fix a position
    for(i = 0; i < 32; i++) {
        // get count of 1s in this position
        count = 0;
        for(j = 0; j < nums.size(); j++){
            // Test whether ith bit of nums[j] is set
            // increment count accordingly.
            count+= nums[j]&(1<<i) ? 1 : 0;
        }
        
        ci = count%3;
        if(ci)
            c = c | (1<<i);
    }
    return c;
}
````

Note : interesting write up [link](https://leetcode.com/problems/single-number-ii/discuss/43295/Detailed-explanation-and-generalization-of-the-bitwise-operation-method-for-single-numbers)

[Single Number III](https://leetcode.com/problems/single-number-iii/)

Note :

lets say b, f are the unique numbers

- b ^ f =  a ^ a ^ b ^ c ^ ....

- b and f are different numbers.

  Can we distinguish b and f on the basis of first different bit from the right. If we find such position we can separate and get that position

  - How will you use this information. ( i.e. i represents first different bit)

    b and f will be in different group that their ith bit will differ. say : b belongs to a group where ith bit is 0. then we can do xor the number where ith bit is zero we definitely say that the one of the number in group=0 will be b.

    Similarly we have same group-1 where ith bit is set 1.

````c++
vector<int> singleNumber(vector<int>& nums) {
    long long run = 0;
    for(int j = 0; j < nums.size(); j++) 
        run ^= nums[j];

    // run = b ^ f
    long long mask = (run) & (~(run-1)); // first differnt bit or first set bit in xor of b and f

    // Group-0/1 processing
    long long xorg1 = 0, xorg2 = 0;
    for(int i = 0; i < nums.size(); i++) {
        if((nums[i]&mask) == 0)
            xorg1 ^= nums[i];
        else
            xorg2 ^= nums[i];
    }
    return {(int)xorg1, (int)xorg2};
}
````

### Set Representation


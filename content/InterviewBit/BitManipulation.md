+++

Title = "Bit Manipulation"

weight = 5

+++

[Bit Manipulation](https://www.interviewbit.com/courses/programming/topics/bit-manipulation/) on Scalar.

XORs have 2 important property other than commutative and associativity.

- Identity element : $A \oplus 0 = A$
- Self-Inverse : $A \oplus A = 0$
- XOR is monotonic in absolute difference between numbers.

[All about XOR](https://accu.org/journals/overload/20/109/lewin_1915/)

#### Bit Play

**Number of 1 Bits**

- Standard loop thru digits and do `if(N & (1 << i)) cnt++;`

**Number of Zeroes**

- Keep counting zeroes from backwards and break when u encounter a 1.

**Reverse Bits**

- Use two pointers with `bitset<n> bt;` and then return `bt.to_long()`.

- traverse thru the bits and calculate `result = (result<<1) | (A >> i & 1);`

**Divide Integers** (Medium)

- To many corner cases.

**Different Bits Sum Pairwise**

- For each of the ith bit in all n numbers calculate number of 1s
- then answer is `ans += 2 * cnt * (A.size() - cnt)`

#### Bit Array

**Single Number**

- Take the XOR of entire array elements.
- Explanation : Each element appears twice so XOR of them is zero which is XOR'd with the element that appears once gives that element back.

**Single Number II**

- 

#### Bit Tricks

**Min XOR Value** (Medium)

- XOR is monotonic in absolute difference between numbers, its little obvious in binary.
- Sort the array and then `ans = min(ans, A[i] ^ A[i+1])` over `0 < i < A.size()-1`
- A further more efficient solution $O(n)$ uses Trie data structure.
- Try similar variant of this problem called as Max XOR Pair.

**Count Total Set Bits** (Hard)

- Any approach based on $O(n)$ or $O(n\log n)$ will fail.

- Solution :

  0 : 00000

  1 : 00001

  2 : 00010

  3 : 00011

  4 : 00100

  If you notice after every $2^i$ power bits are inverted. So instead of traversing thru A we will do as follows.

  

  ````c++
  int solve(int A) {
      if (A==0)
          return 0;
      long long int p=log(A)/log(2);
      long long int ans=0;
      ans+=p*pow(2,p)/2+(A-pow(2,p)+1)+solve(A-pow(2,p));
      return ans%1000000007 ;
  }
  ````

  

**Palindromic Binary Representation** (Hard)



**XOR-ing the Subarrays!** (Medium)

- given list [1,2,3, ... n] how do you generate all subarrays ? (let put partition at kth index)

- [ 1 | 2 | 3 | ... k-1 | k | k + 1| .... n] : we can see k is included in exactly $k(n-k+1)$ subarrays.

- In our case its enough to find whether this is even or odd `(i-1)*(n-i)`. If even numbers xor becomes zero or else the number itself.

- `if(((i+1) *(n-i))%2) res ^= A[i];` over $0 \le i < n$

  


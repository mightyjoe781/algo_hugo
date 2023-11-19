+++

Title = "Synopsis"
chapter = true

weight = 14

tags = ["summary"]

+++

### Data Structures

- Trie

  ````c++
  struct vertex {
      char alphabet;
      bool exist;
      vector<vertex*> child;
      vertex(char a): alphabet(a), exist(false) {child.assign(26,NULL);}
  };
  
  class Trie {
  private:
      vertex* root;
  public:
      Trie() {root = new vertex('!');}
      
      void insert(string word) {
          vertex* cur = root;
          for(int i = 0; i < (int)word.size()); ++i) {
              int alphaNum = word[i]-'A';
              if(cur->child[alphaNum] == NULL)
                  cur->child[alphaNum] = new vertex(word[i]);
              cur = cur->child[alphaNum];
          }
          cur->exists = true;
      }
      
      bool search(string word) {
          vertex* cur = root;
          for(int i = 0; i < (int) word.size(); ++i) {
              int alphaNum = word[i]-'A';
              if(cur->child[alphaNum] == NULL)
                  return false; // early termination
              cur = cur->child[alphaNum];
          }
          return cur->exists;
      }
      bool startsWith(string prefix) {
          vertex* cur = root;
          for(int i = 0; i < (int) prefix.size(); ++i) {
              int alphaNum = prefix[i]-'A';
              if(cur->child[alphaNum] == NUL)
                  return false;
              cur = cur->child[alphaNum];
          }
          
      }
  }
  ````

  

### Searching

- Linear Search

- **Binary Search**
  ````c++
  int main() {
      int A[] = {1,3,5,6,8,10,14};
      int l = 0, r = A.size()-1, mid;
      while(l < r) {
          mid = l + (r-l+1)/2; // in case of lower mid - we compensate
          if(predicate(x,A[mid]))
      		l = mid;
          else
              r = mid-1;
      }
      cout << l << endl;		// above primer ensures answer is at l
  }
  ````

### Sorting



### Graph

### Strings

- **String Hashing**

  ````c++
  long long compute_hash(string const& s) {
      const int p = 31;  // prime p = 51 if capital letters are used
      const int m = 1e9 + 9;
      long long hash_val = 0;
      long long p_pow = 1;
      for (char c : s) {
          hash_val = (hash_val + (c - 'a' + 1) * p_pow) % m;
          p_pow = (p_pow * p) % m;
      }
      return hash_val;
  }
  ````

  Hashing of substring of $s$ from $i$ to $j$.

  $hash(s[i...j]) . p^i = hash(s[0...j]) - hash(s[0...i-1])$

  Rabin-Karp : utilizes above strategy with precomputed power array.

- **Prefix Function Calculation (LPS)**

  ````c++
  vector<int> prefix_function(string s) {
      int n = (int) s.length();
      vector<int> pi(n);
      for (int i = 1; i < n; i++) {
          int j = pi[i-1];
          while(j > 0 && s[i] != s[j])
              j--;
          if(s[i] == s[j])
              j++;
          pi[i] = j;
      }
      return pi;
  }
  ````

  LPS(i) : Longest prefix which also a suffix in the subarray $S([0...i])$

- **KMP**

  Using above LPS do this

  1. value to put in prefix function is needle + "$" + haystack.
  2. then find if `LPS(i) == needle.size()` , then return `i-2*needle.size()` as matched substring location.

- 
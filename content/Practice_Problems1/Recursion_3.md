+++

title = "Recursion 3"
date = 2021-05-15T23:01:25+05:30
weight = 6

+++

##### N-Queens Problem (Very Famous Problem)

[Link](https://leetcode.com/problems/n-queens/)

Enumeration Problem

Problem can be restated as putting 1 queen into each row such that they don't conflict.

Split into chunks

- n queens can be put in n rows
- for each row we have n possible columns

$c_1$ : List of valid configuration s.t. first cell is [0,0] and rest of solution is `Mat[1..n-1][0..n-1]`

$c_2$ : List of valid configuration s.t. first cell is [0,1]

$c_3$ : List of valid configuration s.t. first cell is [0,2]

Subproblem instance

$c_1$ : take (0,0) as occupied and then find out all valid configuration for n-1 queens and append (0,0) to it.

Representation of problem

`f(nxn Matrix, n-1)`

````c++
bool existsInCol(vector<string>& mat, int col, int n){
    for(int i = 0; i <n ; i++){
        if(mat[i][col] == 'Q')
            return true;
    }
    return false;
}
bool existsInDiag(vector<string>& mat, int curr_row, int curr_col, int type, int n){
    //type == 1 principal diagonal (positive slope)
    //type == 2 secondary diagonal (negative slope)

    //Go up
    int factor = (type == 1)? 1 : -1;

    int i = curr_row, j = curr_col;
    while( i>=0 && j<n && j >= 0){
        if(mat[i][j] == 'Q')
            return true;
        i--;
        j+=factor;
    }
    //Go down
    return false;
}
void f(vector<string>& mat, int  rem, vector<string>& contri, vector<vector<string>>& res,int n){

    // Base step
    if(rem == 0)
    { res.push_back(contri); return;}
    //recursive step
    //c1 to c_N
    //try n-rem row
    int i;
    for(i = 0; i < n; i++){
        //check if this is a valid position
        // check if possible in curr col
        if(!existsInCol(mat,i,n) &&
           !existsInDiag(mat,n-rem,i,1,n)&&
           !existsInDiag(mat,n-rem,i,2,n)){
            mat[n-rem][i] = 'Q';
            contri[n-rem][i] = 'Q';
            f(mat,rem-1,contri,res,n);
            mat[n-rem][i] = '.';
            contri[n-rem][i] ='.';

        }
    }

}
vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> res;
    vector<string> mat(n, string(n,'.'));
    vector<string> contri(n,string(n,'.'));
    f(mat,n,contri,res,n);
    return res;

}
````

[Word Search](https://leetcode.com/problems/word-search/) Add Hoc (Recursion)

dfs problem : )

````c++
vector<vector<int>> dirs = {{1,0}, {-1,0} , {0,1} , {0,-1}};
bool f(vector<vector<char>>& board, int start_i, int start_j,vector<vector<bool>>& visited, string& word, int pos){
    // 4 possibilities
    if(pos == word.size())
        return true;
    int m = board.size() , n =board[0].size();
    bool res = false;
    for(int i = 0; i < dirs.size(); i++){
        int x = start_i + dirs[i][0];
        int y = start_j + dirs[i][1];

        //check if its valid
        if(x<0 || x>=m || y <0  || y>=n) continue;
        if(visited[x][y] || board[x][y] != word[pos]) continue;
        visited[x][y] = true;
        res = res||f(board, x, y, visited, word, pos+1);
        visited[x][y] = false;

    }
    return res;
}
bool exist(vector<vector<char>>& board, string word) {
    int m = board.size(), n = board[0].size();
    bool res = false;
    vector<vector<bool>> visited(m,vector<bool>(n,false));
    for(int i = 0; i<m; i++)
        for(int j = 0; j < n ; j++)
            if(board[i][j] == word[0]){
                visited[i][j] = true;
                res = res || f(board,i,j,visited, word, 1);
                visited[i][j] = false;
            }

    return res;
}
````

<hr>

##### Mathematical Problem : Combinatorial

Time Complexity : Visual Methods

[Count All Valid Pickup and Delivery Options](https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options/)

(P1,D1) (P2,D2) ... (Pn,Dn)

P1 P2 D1 P3 D2 D3

for putting (P4,D4) we have 7+6+5+... +1 possibilities

say P1 to Pn and D1 to Dn is already put and we want to add ($P_{n+1}, D_{n+1}$)

i.e. $\frac{(2n+1)(2n+1+1)}{2} = (n+1)(2n+1)$

````c++
long mod = 1e9+7;
int countOrders(int n) {
    long ans = 1; 
    for(long int i = 1; i < n; i++)
        ans = ((ans *(i+1))%mod * (2*i+1)%mod)%mod; 

    return ans;
}
````

**Note : **

$ans = a_1 + a_2 + a_3 .... + a_n$

$ans\% mod = (((a_1 \%mod + a_2 \%mod) \%mod +a_3\%mod) \%mod .... + a_n) \%mod$

$(a-b) \% m = (a\%m - b\%m) + m$

[Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

We can't list all of the permutation :D TLE

we will need to do dfs

take 1, 2, 3

possibilities : (123, 132,213, 231, 312,321) and we have to give kth solution!

There are 3 chunks , chunks including 1, 2 , 3 as first number respectively!

Now say its a ordered listing then we can easily say chunk size and the chunk in which this k will lie.

chunk_id = $\frac{k-1}{(n-1)!}$

then calculate offset how far this item is away from that chunk_id

hmmm can we do that recursively ! yes :D

````c++
int fact(int n){
    int i, res = 1;
    for(i = 1; i<= n; i++)
        res = res * i;
    return res;
}
//Return the pos and unvisited number
int getNumber(vector<int>& visited, int pos){
    int count = 0;
    for(int i = 1; i<= 9; i++){
        if(visited[i]) continue;
        count++;
        if(count == pos)
            return i;
    }
    //NF-won't reach here
    return -1;
}
string getPermutation(int n, int k) {
    int i,chunk_id, pos, curr_digit;
    int copy_n = n;
    vector<int> visited(10,0); 
    string res = "";
    for( i = 1; i <= copy_n; i++){
        chunk_id = (k-1)/fact(n-1); 
        // get corresponding digit
        pos = chunk_id +1;
        curr_digit = getNumber(visited, pos);
        res = res + to_string(curr_digit);

        // mark visited
        visited[curr_digit] = 1;
        // update k and n;
        k = k - (chunk_id*fact(n-1));
        n--;
    }
    return res;
}
````

##### Mathematical Analysis on Time Complexity of Recursion

- Back Substitution Method (write recurrence)

$$
\begin{equation}
f(n) = f(n-1)+ f(n-2) \\
f(n) = f(n-2)+ f(n-3)+f(n-3)+f(n-4) \\
f(n) = f(n-2)+ 2f(n-3) + f(n-4)\\
....
f(n) = f(1) + O(2^n)
\end{equation}
$$

- Tree Method
  - Imagine the tree
  - Sum up the work at each level

- Subsets Problem : $O(2^n)$
- Combination Problem : $O(2^{max(n,k)}) = O(2^n)$


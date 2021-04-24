+++

title = "Trees-1"
date = 2021-04-16T09:01:25+05:30
weight = 10

+++



### Implementation Problems

Mostly we try to solve same subproblems for both children and use the results to make decision for root node.

[Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) :

longest path either comes from left node or right node we can take just take max of whatever our recursive function returns.

`maxDepth(root)=max(1+maxDepth(root->left),1+maxDepth(root->right))`

````c++
int maxDepth(TreeNode* root) {
	if(!root) return 0;
    return 1+max(maxDepth(root->left),maxDepth(root->right));
}
````

By reference based recursion method : good for revision ;0

````c++
void maxDepthHelper(TreeNode* root, int contri,int& res){
    // base case 
    if(!root) return;
    // update contri only at a leaf
    if(!root->left && !root->right){
        res = max(res,1+contri);
        return;
    }
     
    // Recursion
    maxDepthHelper(root->left,contri+1, res);
    maxDepthHelper(root->right,contri+1, res);
}
int maxDepth(TreeNode* root){
    int res = 0;
    maxDepthHelper(root,0,res);
    return res;
}
````

[Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/) : 

Can't implement it like previous question as there is issue with how we dealing with base case here. So we separately count answer for each child.

````c++
int minDepth(TreeNode* root) {
    if(!root) return 0; 
    int left_ans = INT_MAX, right_ans = INT_MAX;
    if(root->left)
        left_ans = minDepth(root->left);
    if(root->right)
        right_ans = minDepth(root->right);
    if(left_ans == INT_MAX && right_ans ==INT_MAX)
        return 1;
    return 1+min(left_ans,right_ans);
} 
````

[Path Sum](https://leetcode.com/problems/path-sum/) :

Top Down Approach

````c++
void flagger(TreeNode* root,int contri, int targetSum, bool& res){
    if(!root)
        return;
    // validate at the leaf node
    if(!root->left && !root->right){
        res = res || contri+root->val == targetSum;
        return;
    }
    //recursive step
    flagger(root->left, contri+root->val,targetSum,res);
    flagger(root->right, contri+root->val,targetSum,res);
}

bool hasPathSum(TreeNode* root, int sum) {
    bool res = false;
    flagger(root,0,sum,res);
    return res;
}
````



[Same Tree](https://leetcode.com/problems/same-tree/) : 

traverse and compare at each recursion

````c++
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(!p && !q) return true;
    if(!p || !q) return false;

    return p->val == q->val && isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
}
````

[Symmetric Tree](https://leetcode.com/problems/symmetric-tree/) : 

````c++
bool areSymmetric(TreeNode* t1, TreeNode* t2){
    // base cases
    if(!t1 && !t2) return true;
    if(!t1 || !t2) return false;

    // recursive cae
    return t1->val == t2->val &&
        areSymmetric(t1->left, t2->right) &&
        areSymmetric(t1->right, t2->left);
}
bool isSymmetric(TreeNode* root) {
    return areSymmetric(root->left,root->right); }
````



### Problems

[Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/) : 

Simple solution is taking difference of left and right subtree. Brute Force as we are calculating height at every node.

2 observation

- for a node location to be balanced , its right and left subtree must be balanced

- L-> balanced , R->balanced, Root should be balanced

So here we make the problem more broader so we add more constraint of root should be balanced.

Most of the times problems on binary trees is all about understanding the problem statement.

````c++
// returns tries if subtree is balanced and return the height of tree
bool isBalancedHelper(TreeNode* root, int &height){
    //base case
    if(!root){
        height = 0;
        return true;     }
    //recursive step
    bool ans_l, ans_r;
    int height_l, height_r;

    // get the height and the answer for the left subtree
    ans_l = isBalancedHelper(root->left,height_l);
    // get the height and the answer for the right subtree
    ans_r = isBalancedHelper(root->right,height_r);
    // compute
    height = 1 + max(height_l,height_r);
    
    return  ans_l && ans_r && abs(height_l-height_r) <= 1;
}
bool isBalanced(TreeNode* root) {
    int height = 0;
    return isBalancedHelper(root,height); }
````



[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) 

Again we need multiple values returned , checking validity along with minimum and maximum element to check validity

````c++
bool isValidBSTHelper(TreeNode* root, long& min_elt, long& max_elt){
    // base case
    if(!root){
        min_elt = LONG_MAX;
        max_elt = LONG_MIN;
        return true;
    }
    // recursive step
    bool ans_l,ans_r;
    long min_left, max_left, min_right,max_right;

    // get for the left and right subtree
    ans_l = isValidBSTHelper(root->left,min_left,max_left);
    ans_r = isValidBSTHelper(root->right,min_right,max_right);

    // combine
    min_elt = min((long)root->val,min(min_left,min_right));
    max_elt = max((long)root->val,max(max_left,max_right));

    return ans_l && ans_r &&
        (root->val > max_left && root->val < min_right);  }
bool isValidBST(TreeNode* root) {
    long min_elt = LONG_MAX, max_elt = LONG_MIN;
    return isValidBSTHelper(root, min_elt, max_elt);
}
````

[Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) : 

Problem here is valid maximum path can be anywhere in the tree.

returns the max sum path (as defined in ques) of the tree rooted at "root" .It also returns the max sum of a path that start at "root" returned in max_sum

````c++
int maxPathSumHelper(TreeNode* root, int& max_sum_rooted){
    if(!root){
        max_sum_rooted = 0;
        return -1005;
    }
    int max_sum_left,max_sum_root_left;
    int max_sum_right,max_sum_root_right;
    // get for left and right
    max_sum_left  = maxPathSumHelper(root->left, max_sum_root_left);
    max_sum_right = maxPathSumHelper(root->right, max_sum_root_right);
    // combine
    max_sum_rooted = root->val + 
        max(0,max(max_sum_root_left,max_sum_root_right));

    int max_sum = max(max_sum_left,max_sum_right);
    max_sum = max(max_sum, max(0,max_sum_root_left)+ 
                  root->val + max(0,max_sum_root_right));
    return max_sum;
}
int maxPathSum(TreeNode* root) {
    int max_sum_rooted;
    return maxPathSumHelper(root,max_sum_rooted); }
````


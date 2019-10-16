# Binary trees

Definition for a binary tree node:
```c++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
``` 
## Symmetric Tree
https://leetcode.com/problems/symmetric-tree/
```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root)
            return true;
        else
            return recursiveThing(root->left, root->right);
    }
private:
    bool recursiveThing(TreeNode* leftBranch, TreeNode* rightBranch) {
        if (!leftBranch && rightBranch || !rightBranch && leftBranch)
            return false;
        else {
            if (!leftBranch)
                return true;
            else
                return leftBranch->val == rightBranch->val && 
                recursiveThing(leftBranch->left, rightBranch->right) &&
                recursiveThing(leftBranch->right, rightBranch->left);
        }
    }
};
```

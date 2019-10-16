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

## Maximum Depth of Binary Tree
https://leetcode.com/problems/maximum-depth-of-binary-tree/
```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root)
            return 0;
        return max(recursiveThing(root->left, 1), recursiveThing(root->right, 1));
    }
private:
    int recursiveThing(TreeNode* leaf, int depth) {
        if (!leaf)
            return depth;
        depth++;
        int left = leaf->left ? recursiveThing(leaf->left, depth) : depth;
        int right = leaf->right ? recursiveThing(leaf->right, depth) : depth;
        return max(left, right); 
    }
};
```

## Same Tree
https://leetcode.com/problems/same-tree/
```c++
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        return recursiveThing(p, q);
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
                recursiveThing(leftBranch->left, rightBranch->left) &&
                recursiveThing(leftBranch->right, rightBranch->right);
        }
    }
};
```

## Invert Binary Tree
https://leetcode.com/problems/invert-binary-tree/
```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        recursiveThing(root);
        return root;
    }
private:
    void recursiveThing(TreeNode* branch) {
        if (!branch)
            return;
        recursiveThing(branch->left);
        recursiveThing(branch->right);
        swap(branch->left, branch->right);
    }
};
```

## Path Sum
https://leetcode.com/problems/path-sum/
```c++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root)
            return false;
        sum -= root->val;
        if (!(root->left || root->right) && sum == 0)
            return true;
        return hasPathSum(root->left, sum) || hasPathSum(root->right, sum); 
    }
};
```

## Binary Tree Level Order Traversal
https://leetcode.com/problems/binary-tree-level-order-traversal/
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector <vector <int>> result;
        recursiveThing(root, result, 0);
        return result;
    }
private:
    void recursiveThing(TreeNode* branch, vector <vector <int>>& tree, int level) {
        if (!branch)
            return;
        if (level >= tree.size())
            tree.push_back(vector <int> {branch->val});
        else
            tree[level].push_back(branch->val);
        recursiveThing(branch->left, tree, level + 1);
        recursiveThing(branch->right, tree, level + 1);
    }
};
```

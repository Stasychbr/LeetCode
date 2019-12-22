# Binary trees

+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)
+ [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
+ [Same Tree](#same-tree)
+ [Invert Binary Tree](#invert-binary-tree)
+ [Path Sum](#path-sum)
+ [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)
+ [Subtree of Another Tree](#subtree-of-another-tree)
+ [Kth Smallest Element in a BST](#kth-smallest-element-in-a-bst)
+ [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
+ [Lowest Common Ancestor of a Binary Search Tree](#lowest-common-ancestor-of-a-binary-search-tree)
+ [Inorder Successor in BST](#inorder-successor-in-bst)
+ [Validate Binary Search Tree](#validate-binary-search-tree)
+ [Binary Search Tree Iterator](#binary-search-tree-iterator)

Definition for a binary tree node:
```c++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
``` 

## Binary Tree Inorder Traversal
https://leetcode.com/problems/binary-tree-inorder-traversal/
### Recursion:
```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector <int> result;
        inOrder(root, result);
        return result;
    }
private:
    void inOrder(TreeNode* root, vector <int>& elements) {
        if (!root) {
            return;
        }   
        else {
            inOrder(root->left, elements);
            elements.push_back(root->val);
            inOrder(root->right, elements);
        }
        
    } 
};
```
### Iterative:
```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector <int> result;
        stack <TreeNode*> treeStack;
        TreeNode* current = root;
        while (current || !treeStack.empty()) {
            while (current) {
                treeStack.push(current);
                current = current->left;
            }
            current = treeStack.top();
            treeStack.pop();
            result.push_back(current->val);
            current = current->right;
        }
        return result;
    }
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
        if (!leftBranch && !rightBranch)
            return true;
        else {
            if (!leftBranch || !rightBranch)
                return false;
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
        return recursiveThing(root, 0);
    }
private:
    int recursiveThing(TreeNode* leaf, int depth) {
        if (!leaf)
            return depth;
        depth++;
        int left = recursiveThing(leaf->left, depth);
        int right = recursiveThing(leaf->right, depth);
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
        if (!leftBranch && !rightBranch)
            return true;
        else {
            if (!leftBranch || !rightBranch)
                return false;
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

### Recursive:
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

### Iterative:
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector <vector <int>> result;
        queue <TreeNode*> branches;
        if (!root)
            return result;
        branches.push(root);
        while (!branches.empty()) {
            vector <int> level;
            for (int i = branches.size(); i > 0; i--) {
                level.push_back(branches.front()->val);
                if (branches.front()->left)
                    branches.push(branches.front()->left);
                if (branches.front()->right)
                    branches.push(branches.front()->right);
                branches.pop();
            }
            result.push_back(level);
        }
        return result;
    }
};
```

## Subtree of Another Tree
https://leetcode.com/problems/subtree-of-another-tree/
```c++
class Solution {
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if (!s)
            return false;
        if (isSame(s, t))
            return true;
        return isSubtree(s->left, t) || isSubtree(s->right, t);
    }
private:
    bool isSame(TreeNode* leftBranch, TreeNode* rightBranch) {
        if (!leftBranch && rightBranch || !rightBranch && leftBranch)
            return false;
        else {
            if (!leftBranch)
                return true;
            else
                return leftBranch->val == rightBranch->val && 
                isSame(leftBranch->left, rightBranch->left) &&
                isSame(leftBranch->right, rightBranch->right);
        }
    }
};
```

## Kth Smallest Element in a BST
https://leetcode.com/problems/kth-smallest-element-in-a-bst/

### Recursive
T∈O(n)
```c++
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        vector <int> elements;
        inOrder(root, elements);
        return elements[k - 1];
    }
private:
    void inOrder(TreeNode* root, vector <int>& elements) {
        if (!root) {
            return;
        }   
        else {
            inOrder(root->left, elements);
            elements.push_back(root->val);
            inOrder(root->right, elements);
        }
        
    } 
};
```

### Iterative
T∈O(h + k)
```c++
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        stack <TreeNode*> roots;
        TreeNode* Kth;
        while (k > 0) { 
            while (root) {
                roots.push(root);
                root = root->left;
            }
            Kth = roots.top();
            roots.pop();
            k--;
            root = Kth->right;
        }
        return Kth->val;
    }
};
```

## Lowest Common Ancestor of a Binary Tree
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* result;
        recursiveThing(root, p, q, result);
        return result;
    }
private:
    bool recursiveThing(TreeNode* branch, TreeNode* p, TreeNode* q, TreeNode*& result) {
        if (!branch)
            return false;
        int core = (int)(branch == q or branch == p);
        int left = (int)recursiveThing(branch->left, p, q, result);
        int right = (int)recursiveThing(branch->right, p, q, result);
        if (core + left + right >= 2)
            result = branch;
        return core or left or right;
    }
};
```

## Lowest Common Ancestor of a Binary Search Tree
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while (root) {
            if (p->val < root->val && q->val < root->val)
                root = root->left;
            else if (p->val > root->val && q->val > root->val)
                root = root->right;
            else
                return root;
        }
        return root;
    }
};
```

## Validate Binary Search Tree
https://leetcode.com/problems/validate-binary-search-tree/
### Recursive
```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (root) {
            vector <int> inOrderTr;
            inOrder(root, inOrderTr);
            for (auto i = inOrderTr.end() - 1; i > inOrderTr.begin(); i--) {
                if (*i <= *(i - 1))
                    return false;
            }
        }
        return true;
    }
```
### Iterative
```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        int lastElem = INT_MIN;
        int flag = 0;
        stack <TreeNode*> treeStack;
        TreeNode* current = root;
        while (current || !treeStack.empty()) {
            while (current) {
                treeStack.push(current);
                current = current->left;
            }
            current = treeStack.top();
            treeStack.pop();
            if (flag && current->val <= lastElem) {
                return false;
            }
            lastElem = current->val;
            current = current->right;
            flag = 1;
        }
        return true;
    }
};
```
## Binary Search Tree Iterator
https://leetcode.com/problems/binary-search-tree-iterator/
```c++
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        inOrder(root, inOrderTr);
    }
    
    /** @return the next smallest number */
    int next() {
        int result = inOrderTr.front();
        inOrderTr.pop();
        return result;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !inOrderTr.empty();
    }
private:
    queue <int> inOrderTr;
    void inOrder(TreeNode* root, queue <int>& elements) {
        if (!root) {
            return;
        }   
        else {
            inOrder(root->left, elements);
            elements.push(root->val);
            inOrder(root->right, elements);
        }
        
    } 
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
 ```
 
 ## Inorder Successor in BST
 https://www.lintcode.com/problem/inorder-successor-in-bst/
 ```c++
 class Solution {
public:
    /*
     * @param root: The root of the BST.
     * @param p: You need find the successor node of p.
     * @return: Successor of p.
     */
    TreeNode * inorderSuccessor(TreeNode * root, TreeNode * p) {
        TreeNode* result = NULL;
        if (!root)
            return NULL;
        if (p->right) {
            result = p->right;
            for (; result->left; result = result->left);
            return result;
        }
        else {
            while (root != p) {
                if (root->val > p->val) {
                    result = root;
                    root = root->left;
                }
                else {
                    root = root->right;
                }
            }
            return result;
                
        }
    }
};
```

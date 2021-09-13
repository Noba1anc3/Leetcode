Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

```
Example 1:

Input: root = [3,9,20,null,null,15,7]
Output: true

Example 2:

Input: root = [1,2,2,3,3,null,null,4,4]
Output: false

Example 3:

Input: root = []
Output: true
```

## Solution

```c++
class Solution{
public:
    bool isAVL(TreeNode* root){
        if (root == nullptr) return true;
        if (isAVL(root->left) && isAVL(root->right) &&
           abs(height(root->left) - height(root->right)) <= 1)
            return true;
        return false;
    }
    int height(TreeNode* root){
        if (root == nullptr) return 0;
        return max(height(root->left), height(root->right)) + 1;
    }
}
```

Optimize:

```c++
class Solution{
public:
    bool rsp = true;

    bool isBalanced(TreeNode* root){
        height(root);
        return rsp;
    }

    int height(TreeNode* root){
        if (root == nullptr) return 0;
        int l_height = height(root->left);
        int r_height = height(root->right);
        if (abs(l_height - r_height) > 1) rsp = false;
        return max(l_height, r_height) + 1;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了71.41%的用户

内存消耗：20.5 MB, 在所有 C++ 提交中击败了33.95%的用户

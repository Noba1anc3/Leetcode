Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.



 **Example:**

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]

Return the following binary tree:

    3
   / \
  9  20
    /  \
   15   7
```

## Solution
### c++

```c++
class Solution {
private:
    unordered_map<int, int> index;

public:
    TreeNode* myBuildTree(vector<int>& preorder, vector<int>& inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
        if (preorder_left > preorder_right)
            return nullptr;

        int preorder_root = preorder_left;
        int inorder_root = index[preorder[preorder_root]];
        
        TreeNode* root = new TreeNode(preorder[preorder_root]);

        int size_left_subtree = inorder_root - inorder_left;

        root->left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        root->right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();

        for (int i = 0; i < n; i++) 
            index[inorder[i]] = i;

        return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了96.93%的用户

内存消耗：24.8 MB, 在所有 C++ 提交中击败了52.51%的用户

Attention:

- ```c++
  if (preorder_left > preorder_right)
  	return nullptr;
  ```

- ```c++
  TreeNode* root = new TreeNode(preorder[preorder_root]);
  ```


输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。 



Examples:

```
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
```

## Solution

### Recursive

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
 
class Solution {
public:
    int maxDepth(TreeNode* T) {
        if (T == NULL) 
            return 0;     
        return max(maxDepth(T->left), maxDepth(T->right)) + 1;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了75.31%的用户

内存消耗：18.4 MB, 在所有 C++ 提交中击败了91.05%的用户

### Non-recursive Queue

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* T) {
        if (T == NULL) return 0;
        
        int height = 1;
        queue<pair<TreeNode*, int>> Q;
        pair<TreeNode*, int> node_height(T, height);

        Q.push(node_height);
        
        while (!Q.empty()){
            TreeNode* node = Q.front().first;
            int curHeight = Q.front().second;
            Q.pop();
                        
            if (curHeight > height)
                height = curHeight;
            
            if (node->left != NULL){
                TreeNode* lchild = node->left;
                pair<TreeNode*, int> lchild_height(lchild, curHeight+1);
                Q.push(lchild_height);
            }
            if (node->right != NULL){
                TreeNode* rchild = node->right;
                pair<TreeNode*, int> rchild_height(rchild, curHeight+1);
                Q.push(rchild_height);
            }
        }
        
        return height;
    }
};
```

执行用时：12 ms, 在所有 C++ 提交中击败了75.31%的用户

内存消耗：18.5 MB, 在所有 C++ 提交中击败了82.96%的用户

### Non-recursive Vector

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
 
class Solution {
public:
    int maxDepth(TreeNode* T) {
        if (T == NULL) return 0;
        
        int height = 0;
        vector<TreeNode*> nodes;
        nodes.push_back(T);
        
        while (nodes.size() != 0){
            vector<TreeNode*> tmp;

            for (TreeNode* node : nodes){
                if (node->left) tmp.push_back(node->left);
                if (node->right) tmp.push_back(node->right);
            }
            
            nodes = tmp;
            height++;
        }
        
        return height;
    }
};
```

执行用时：16 ms, 在所有 C++ 提交中击败了46.69%的用户

内存消耗：19 MB, 在所有 C++ 提交中击败了48.79%的用户
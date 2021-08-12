Given the root of a binary tree, determine if it is a complete binary tree.

In a complete binary tree, every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

 
```
Example 1:

Input: root = [1,2,3,4,5,6]
Output: true
Explanation: Every level before the last is full (ie. levels with node-values {1} and {2, 3}), and all nodes in the last level ({4, 5, 6}) are as far left as possible.

Example 2:

Input: root = [1,2,3,4,5,null,7]
Output: false
Explanation: The node with value 7 isn't as far left as possible.
```

## Solution

```c++
class Solution {
public:
    bool isCompleteTree(TreeNode* root) {
        long node = 1, maxNode = 1;
        queue<pair<TreeNode*, int>> Q;
        Q.push(make_pair(root, 1));

        while (!Q.empty()){
            pair<TreeNode*, long> curNode = Q.front(); Q.pop();

            if (curNode.first->left){
                node++;
                maxNode = curNode.second*2;
                Q.push(make_pair(curNode.first->left, curNode.second*2));
            }
            if (curNode.first->right){
                node++;
                maxNode = curNode.second*2+1;
                Q.push(make_pair(curNode.first->right, curNode.second*2+1));
            }
        }

        return node == maxNode;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了80.84%的用户

内存消耗：10 MB, 在所有 C++ 提交中击败了83.74%的用户

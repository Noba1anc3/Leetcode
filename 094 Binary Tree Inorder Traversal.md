Given the root of a binary tree, return the inorder traversal of its nodes' values.

## Solution

### 递归

C++
```c++
class Solution {
public:
    void inorder(TreeNode* root, vector<int>& InOrder) {
        if (!root) {
            return;
        }
        inorder(root->left, InOrder);
        InOrder.push_back(root->val);
        inorder(root->right, InOrder);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> InOrder;
        inorder(root, InOrder);
        return InOrder;
    }
};

```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户  
内存消耗：8.3 MB, 在所有 C++ 提交中击败了11.30%的用户

### 迭代

c++
```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root != nullptr || !stk.empty()) {
            while (root != nullptr) {
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            res.push_back(root->val);
            root = root->right;
        }
        return res;
    }
};

```

执行用时：4 ms, 在所有 C++ 提交中击败了46.28%的用户  
内存消耗：8.5 MB, 在所有 C++ 提交中击败了8.21%的用户

### 颜色标记法
其核心思想如下：

- 使用颜色标记节点的状态，新节点为白色，已访问的节点为灰色。
- 如果遇到的节点为白色，则将其标记为灰色，然后将其右子节点、自身、左子节点依次入栈。
- 如果遇到的节点为灰色，则将节点的值输出。

python
```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        WHITE, GRAY = 0, 1
        res = []
        stack = [(WHITE, root)]
        while stack:
            color, node = stack.pop()
            if node is None: continue
            if color == WHITE:
                stack.append((WHITE, node.right))
                stack.append((GRAY, node))
                stack.append((WHITE, node.left))
            else:
                res.append(node.val)
        return res
```
执行用时：32 ms, 在所有 Python3 提交中击败了96.75%的用户  
内存消耗：13.4 MB, 在所有 Python3 提交中击败了14.15%的用户

Given the root of a binary tree, return the inorder traversal of its nodes' values.

## Solution

### 递归

C++
```c++
class Solution {
private:
    vector<int> inOrder;

public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (root){
            if (root->left) inorderTraversal(root->left);
            inOrder.push_back(root->val);
            if (root->right) inorderTraversal(root->right);
        }
        return inOrder;
    }
};
```
执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户  
内存消耗：9.3 MB, 在所有 C++ 提交中击败了7.30%的用户

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

c++
```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1;
    stack<pair<TreeNode*, int>> inOrder;
    vector<int> rsp;

public:
    vector<int> inorderTraversal(TreeNode* root) {
        inOrder.push(pair<TreeNode*, int>(root, WHITE));
        while (!inOrder.empty()){
            pair<TreeNode*, int> stackTop = inOrder.top();
            inOrder.pop();
            if (!stackTop.first) continue;
            if (stackTop.second == WHITE){
                inOrder.push(pair<TreeNode*, int>(stackTop.first->right, WHITE));
                inOrder.push(pair<TreeNode*, int>(stackTop.first, GRAY));
                inOrder.push(pair<TreeNode*, int>(stackTop.first->left, WHITE));
            }
            else{
                rsp.push_back(stackTop.first->val);
            }
        }
        return rsp;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了41.13%的用户

内存消耗：7.9 MB, 在所有 C++ 提交中击败了97.18%的用户

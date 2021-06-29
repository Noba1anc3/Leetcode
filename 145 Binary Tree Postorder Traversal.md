Given the root of a binary tree, return the postorder traversal of its nodes' values.

## Solution

### 颜色标记法
其核心思想如下：

- 使用颜色标记节点的状态，新节点为白色，已访问的节点为灰色。
- 如果遇到的节点为白色，则将其标记为灰色，然后将其自身、右子节点、左子节点依次入栈。
- 如果遇到的节点为灰色，则将节点的值输出。

python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        WHITE, GRAY = 0, 1
        res = []
        stack = [(WHITE, root)]
        while stack:
            color, node = stack.pop()
            if node is None: continue
            if color == WHITE:
                stack.append((GRAY, node))
                stack.append((WHITE, node.right))
                stack.append((WHITE, node.left))
            else:
                res.append(node.val)
        return res
```

执行用时：32 ms, 在所有 Python3 提交中击败了96.83%的用户  
内存消耗：13.4 MB, 在所有 Python3 提交中击败了29.35%的用户

c++
```c++
class Solution {
private:
    int WHITE = 0, GRAY = 1;
    stack<pair<TreeNode*, int>> inOrder;
    vector<int> rsp;

public:
    vector<int> postorderTraversal(TreeNode* root) {
        inOrder.push(pair<TreeNode*, int>(root, WHITE));
        while (!inOrder.empty()){
            pair<TreeNode*, int> stackTop = inOrder.top();
            inOrder.pop();
            if (!stackTop.first) continue;
            if (stackTop.second == WHITE){
                inOrder.push(pair<TreeNode*, int>(stackTop.first, GRAY));
                inOrder.push(pair<TreeNode*, int>(stackTop.first->right, WHITE));
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

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：8.2 MB, 在所有 C++ 提交中击败了63.26%的用户

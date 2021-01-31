Given a binary tree, write a function to get the maximum width of the given tree. The maximum width of a tree is the maximum width among all levels.

The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the null nodes between the end-nodes are also counted into the length calculation.

It is guaranteed that the answer will in the range of 32-bit signed integer.



**Example:**
```
Example 1:

Input: 

           1
         /   \
        3     2
       / \     \  
      5   3     9 

Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).


Example 2:

Input: 

          1
         /  
        3    
       / \       
      5   3     

Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).


Example 3:

Input: 

          1
         / \
        3   2 
       /        
      5      

Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).


Example 4:

Input: 

          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
Output: 8
Explanation:The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
```

## Solution

### C++

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int widthOfBinaryTree(TreeNode* root) {
        unsigned long long ans = 0;
        unsigned long long left = 0;
        int curLevel = 0;

        pair<int, unsigned long long> level_id(0, 0);
        pair<TreeNode*, pair<int, unsigned long long>> ROOT(root, level_id);
        queue<pair<TreeNode*, pair<int, unsigned long long>>> Q;
        Q.push(ROOT);

        while (!Q.empty()){
            TreeNode* T = Q.front().first;
            int level = Q.front().second.first;
            unsigned long long id = Q.front().second.second;
            Q.pop();

            if (T->left){
                pair<int, unsigned long long> lchild_level_id (level + 1, id * 2);
                Q.push(pair<TreeNode*, 
                       pair<int, unsigned long long>> (T->left, lchild_level_id));
            }
            if (T->right){
                pair<int, unsigned long long> rchild_level_id (level + 1, id * 2 + 1);
                Q.push(pair<TreeNode*, 
                       pair<int, unsigned long long>> (T->right, rchild_level_id));
            }
            if (curLevel != level){
                curLevel = level;
                left = id;
            }
            ans = max(ans, id - left + 1);
        }

        return ans;
    }
};
```

执行用时：4 ms, 在所有 C++ 提交中击败了98.17%的用户

内存消耗：16 MB, 在所有 C++ 提交中击败了88.70%的用户

Attention

- ```java
  Q.front()
  ```

- ```java
  T->left
  ```

- ```python
  unsigned long long
  ```

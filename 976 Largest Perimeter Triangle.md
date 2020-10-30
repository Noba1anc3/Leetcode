Given an array `A` of positive lengths, return the largest perimeter of a triangle with **non-zero area**, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return `0`.

Examples:

```
Example 1:

Input: [2,1,2]
Output: 5
Example 2:

Input: [1,2,1]
Output: 0
Example 3:

Input: [3,2,3,4]
Output: 10
Example 4:

Input: [3,6,2,3]
Output: 8
```

## Solution

c++
```python
#include <algorithm>

class Solution {
public:

    static bool compare(int a, int b){
        return a > b;
    }

    int largestPerimeter(vector<int>& A) {
        sort(A.begin(), A.end(), compare);
        for(int i = 0; i < A.size() - 2; i++){
            if (A[i] < A[i+1] + A[i+2]){
                return A[i] + A[i+1] + A[i+2];
            }
        }
        return 0;
    }
};
```

执行用时：108 ms, 在所有 C++ 提交中击败了81.30%的用户  
内存消耗：20.6 MB, 在所有 C++ 提交中击败了5.14%的用户

Attention:
- vector<int>&
- vector.size()
- sort(A.begin(), A.end(), compare);
- static bool compare()

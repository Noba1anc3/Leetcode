Given a list of daily temperatures `T`, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.

For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

Note: The length of `temperatures` will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.

## Solution
可以维护一个存储下标的单调栈，从栈底到栈顶的下标对应的温度列表中的温度依次递减。如果一个下标在单调栈里，则表示尚未找到下一次温度更高的下标。

正向遍历温度列表。对于温度列表中的每个元素 `T[i]`，如果栈为空，则直接将 `i` 进栈，如果栈不为空，则比较栈顶元素 `prevIndex` 对应的温度 `T[prevIndex]` 和当前温度 `T[i]`，如果 `T[i] > T[prevIndex]`，则将 `prevIndex` 移除，并将 `prevIndex` 对应的等待天数赋为 `i - prevIndex`，重复上述操作直到栈为空或者栈顶元素对应的温度大于等于当前温度，然后将 `i` 进栈。

由于单调栈满足从栈底到栈顶元素对应的温度递减，因此每次有元素进栈时，会将温度更低的元素全部移除，并更新出栈元素对应的等待天数，这样可以确保等待天数一定是最小的。

以下用一个具体的例子帮助读者理解单调栈。对于温度列表 `[73,74,75,71,69,72,76,73]`，单调栈 `stack` 的初始状态为空，答案 `ans` 的初始状态是 `[0,0,0,0,0,0,0,0]`，按照以下步骤更新单调栈和答案，其中单调栈内的元素都是下标，括号内的数字表示下标在温度列表中对应的温度。

- 当 i=0 时，单调栈为空，因此将 0 进栈。

  - stack=[0(73)]

  - ans=[0,0,0,0,0,0,0,0]

- 当 i=1 时，由于 74 大于 73，因此移除栈顶元素 0，赋值 ans[0]:=1-0，将 1 进栈。

  - stack=[1(74)]

  - ans=[1,0,0,0,0,0,0,0]

- 当 i=2 时，由于 75 大于 74，因此移除栈顶元素 1，赋值 ans[1]:=2-1，将 2 进栈。

  - stack=[2(75)]

  - ans=[1,1,0,0,0,0,0,0]

- 当 i=3 时，由于 71 小于 75，因此将 3 进栈。

  - stack=[2(75),3(71)]

  - ans=[1,1,0,0,0,0,0,0]

- 当 i=4 时，由于 69 小于 71，因此将 4 进栈。

  - stack=[2(75),3(71),4(69)]

  - ans=[1,1,0,0,0,0,0,0]

- 当 i=5 时，由于 72 大于 69 和 71，因此依次移除栈顶元素 4 和 3，赋值 ans[4]:=5−4 和 ans[3]:=5−3，将 5 进栈。

  - stack=[2(75),5(72)]

  - ans=[1,1,0,2,1,0,0,0]

- 当 i=6 时，由于 76 大于 72 和 75，因此依次移除栈顶元素 5 和 2，赋值 ans[5]:=6−5 和 ans[2]:=6−2，将 6 进栈。

  - stack=[6(76)]

  - ans=[1,1,4,2,1,1,0,0]

- 当 i=7 时，由于 73 小于 76，因此将 7 进栈。

  - stack=[6(76),7(73)]

  - ans=[1,1,4,2,1,1,0,0]

python
```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        length = len(T)
        ans = [0] * length
        stack = []
        for i in range(length):
            temperature = T[i]
            while stack and temperature > T[stack[-1]]:
                prev_index = stack.pop()
                ans[prev_index] = i - prev_index
            stack.append(i)
        return ans
```

执行用时：588 ms, 在所有 Python3 提交中击败了35.97%的用户  
内存消耗：17.2 MB, 在所有 Python3 提交中击败了19.27%的用户

c++
```c
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> S;
        vector<int> rsp(temperatures.size(), 0);

        for(int i = 0; i < temperatures.size(); i++){
            while (!S.empty() && temperatures[S.top()] < temperatures[i]){
                rsp[S.top()] = i - S.top();
                S.pop();
            }
            S.push(i);
        }

        return rsp;
    }
};
```

执行用时：188 ms, 在所有 C++ 提交中击败了15.52%的用户

内存消耗：86.7 MB, 在所有 C++ 提交中击败了21.12%的用户

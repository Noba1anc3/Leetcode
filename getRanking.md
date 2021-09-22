已知有n个人组成数组nums，nums中的每一项为他前面有几个人比他矮，返回一个数组，数组每一项为身高从小到大排序的编号。

Input : 0 1 1 0  
Output : 2 4 3 1

## Solution

双栈法，如果为0则直接压入栈顶，如果非0则弹出该项大小的元素，将该项下标压入栈后，再将弹出的元素依次填回去（相当于抄底）

```c++
void getRanking(vector<int>& nums){
    stack<int> S, tmp;
    for (int i = 0; i < nums.size(); i++){
        if (nums[i] == 0)
            S.push(i);
        else{
            int num = nums[i];
            while(num--){
                tmp.push(S.top());
                S.pop();
            }
            S.push(i);
            while(!tmp.empty()){
                S.push(tmp.top());
                tmp.pop();
            }
        }
    }

    int idx = 1;
    while (!S.empty()){
        nums[S.top()] = idx++;
        S.pop();
    }
}
```

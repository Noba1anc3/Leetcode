飞机选座问题

要求选座的乘客之间最少间隔一个人，如果能顺利选座，则返回成功，并输出座位号。否则，返回失败。

Example
```
Input:
2 2
***| |***
***| |.*.

Output:
SUCCESS
2D
2F

Input:
2 3
***| |***
***| |.*.

Output:
FAILED

Input:
2 2
***| |*.*
***| |.*.

Output:
FAILED

Input:
2 3
**.| |***
***| |.*.

Output:
SUCCESS
1C
2D
2F
```

## Solution

```c++
class Solution{
public:
    pair<int, int> tmp;
    vector<pair<int, int>> rsp;

    bool can_insert(vector<vector<int>> nums, int i, int j){
        if (nums[i-1][j] == 2 || nums[i-1][j-1] == 2 || nums[i-1][j+1] == 2 ||
            nums[i][j-1] == 2 || nums[i][j+1] == 2 || nums[i+1][j] == 2 ||
            nums[i+1][j+1] == 2 || nums[i+1][j-1] == 2){
            return false;
        }
        return true;
    }

    void solution(int C, vector<vector<int>>& nums){
        if (C == 0){
            rsp.push_back(tmp);
            return;
        }
        for (int i = 1; i < nums.size() - 1; i++){
            for (int j = 1; j < 8; j++){
                if (nums[i][j] == 0 && can_insert(nums, i, j)){
                    nums[i][j] = 2;
                    tmp = make_pair(i, j - 1);
                    solution(C - 1, nums);
                    nums[i][j] = 0;
                }
            }
        }
    }
};



int main(){
    unordered_map<int, char> dict = {{1, 'A'}, {2, 'B'}, {3, 'C'}, {4, 'D'}, {5, 'E'}, {6, 'F'}};
    int N, C;
    cin >> N >> C;
    string line;
    getline(cin, line);
    vector<vector<int>> nums(N+2, vector<int>(9, 1));
    
    for (int i = 0; i < N; i++){
        getline(cin, line);
        for (int j = 0; j < 3; j++){
            if (line[j] == '.')
                nums[i+1][j+1] = 0;
        }
        for (int j = 6; j < 9; j++){
            if (line[j] == '.')
                nums[i+1][j-1] = 0;
        }
    }

    Solution s = Solution();
    s.solution(C, nums);
    bool fail = s.rsp.empty();
    if (fail)
        cout<<"FAILED";
    else{
        cout<<"SUCCESS"<<"\n";
        for(const pair<int, int>& p : s.rsp){
            cout << p.first << dict[p.second] << '\n';
        }
    }
    return 0;
}
```

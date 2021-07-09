```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;


class Solution{
private:
    vector<int> dp, p;

public:
    static bool cmp(vector<int>& act1, vector<int>& act2){
        return act1[1] < act2[1];
    }

    void set_p_vector(vector<vector<int>>& Activities){
        for (int i = 1; i < Activities.size(); i++){
            int cur_start_time = Activities[i][0];
            for (int j = i - 1; j >= 0; j--){
                int bef_end_time = Activities[j][1];
                if (bef_end_time <= cur_start_time){
                    p[i + 1] = j + 1;
                    break;
                }
            }
        }
    }

    int Weighted_Activity_Selection(vector<vector<int>>& Activities)
    {
        int n = Activities.size();
        dp.resize(n+1), p.resize(n+1);

        sort(Activities.begin(), Activities.end(), cmp);

        set_p_vector(Activities);

        for (int i = 1; i <= n; i++)
            dp[i] = max(dp[i-1], Activities[i-1][2] + dp[p[i]]);

        return dp[n];

    }
};


int main()
{
    vector<vector<int>> Activities = {{1,4,5},{3,5,8},{0,6,2},{5,7,4},{3,9,9},{5,9,2},
                                      {6,10,5},{8,11,8},{8,12,3},{2,14,6},{12,16,1}};

    Solution Slt = Solution();
    cout<<Slt.Weighted_Activity_Selection(Activities);;

    return 0;
}
```

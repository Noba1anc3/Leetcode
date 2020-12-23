```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int Weighted_Activity_Selection(vector<vector<int>> Activities)
{
    int n = Activities.size();
    vector<int> dp(n+1, 0), p(n+1, 0);

    sort(Activities.begin(), Activities.end(), [](const vector<int> act1, const vector<int> act2){
         return act1[1] < act2[1];
    });

    for (int i = 2; i <= n; i++){
        vector<int> activity_current = Activities[i-1];
        for (int j = i-1; j > 0; j--){
            vector<int> activity_before = Activities[j-1];
            if (activity_before[1] <= activity_current[0]){
                p[i] = j;
                break;
            }
        }
    }

    for (int i = 1; i <= n; i++){
        dp[i] = max(dp[i-1], Activities[i-1][2] + dp[p[i]]);
    }

    return dp[n];

}

int main()
{
    vector<vector<int>> Activities = {{1,4,5},{3,5,8},{0,6,2},{5,7,4},{3,9,9},{5,9,2},
                                      {6,10,5},{8,11,8},{8,12,3},{2,14,6},{12,16,1}};

    int total = Weighted_Activity_Selection(Activities);
    cout<<total;

    return 0;
}
```

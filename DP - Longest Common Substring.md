```c++
#include <iostream>
#include <vector>
using namespace std;

void Longest_Common_Substring(string a, string b){
    int m = a.size(), n = b.size();
    int lMax = 0, pMax;

    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));

    for (int i = 1; i <= m; i++){
        for (int j = 1; j <= n; j++){
            char charA = a[i-1];
            char charB = b[j-1];
            if (charA != charB){
                dp[i][j] = 0;
            }
            else{
                dp[i][j] = dp[i-1][j-1] + 1;
                if (dp[i][j] > lMax){
                    lMax = dp[i][j];
                    pMax = i;
                }
            }
        }
    }

    if (lMax == 0)
        return;
    else
        for (int i = pMax - lMax + 1; i <= pMax; i++)
            cout<<a[i-1];
}


int main()
{
    string a = "DEADBEE", b = "EATBEE";
    Longest_Common_Substring(a, b);

    return 0;
}
```

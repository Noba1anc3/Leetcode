```c++
#include <iostream>
#include <vector>

using namespace std;

int n = 4;
vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
vector<vector<int>> split(n+1, vector<int>(n+1, 0));

void print(int i, int j)
{
    if(i == j){
        cout<<"A["<<i<<"]";
        return;
    }
    cout<<"(";
    print(i, split[i][j]);
    print(split[i][j]+1, j);
    cout<<")";
}

int Matrix_Multiplication(vector<int> p, int n){
    for (int l = 1; l <= n - 1; l++){
        for (int i = 1; i <= n - l; i++){
            int j = i + l;
            dp[i][j] = INT_MAX;
            for (int k = i; k < j; k++){
                int q = dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j];
                if (q < dp[i][j]){
                    dp[i][j] = q;
                    split[i][j] = k;
                }
            }
        }
    }

    print(1, n);

    return dp[1][n];
}


int main()
{
    vector<int> p = {5, 4, 6, 2, 7};
    int optimal = Matrix_Multiplication(p, n);
    cout<<'\n'<<optimal;
    return 0;
}
```

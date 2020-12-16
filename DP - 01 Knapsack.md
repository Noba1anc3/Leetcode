```c++
#include <iostream>
#include <vector>
using namespace std;

int Knapsack(int W, vector<int> w, vector<int> v){
    int n = w.size();
	int V[n+1][W+1], keep[n+1][W+1];

    for (int i = 0; i <= W; i++)
        V[0][i] = 0;

    for (int i = 1; i <= n; i++){
        for (int j = 0; j <= W; j++){
            int weight = w[i];
            int value = v[i];
            if (weight <= j && value + V[i-1][j-w[i]] > V[i-1][j]){
                V[i][j] = value + V[i-1][j-w[i]];
                keep[i][j] = 1;
            }
            else{
                V[i][j] = V[i-1][j];
                keep[i][j] = 0;
            }
        }
    }

    int K = W;
    for (int i = n; i >= 1; i--){
        if (keep[i][K]){
            cout<<i<<' ';
            K -= w[i];
        }
    }

    return V[n][W];
}


int main()
{
    int W = 10;
    vector<int> w = {0, 5, 4, 6, 3};
    vector<int> v = {0, 10, 40, 30, 50};

    Knapsack(W, w, v);

    return 0;
}
```


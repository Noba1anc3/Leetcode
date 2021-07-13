```c++
#include <iostream>
#include <vector>

using namespace std;

const int n = 5;

double p[n+1] = {-1, 0.15, 0.1, 0.05, 0.1, 0.2};
double q[n+1] = {0.05, 0.1, 0.05, 0.05, 0.05, 0.1};

int root[n+1][n+1];
double w[n+2][n+2];
double e[n+2][n+2];

double optimalBST(int n, double *p, double *q){
    for (int i = 1; i <= n + 1; i++)
        w[i][i - 1] = e[i][i - 1] = q[i - 1];

    for (int len = 0; len < n; len++){
        for (int i = 1; i <= n; i++){
            int j = i + len;
            e[i][j] = INT_MAX;
            w[i][j] = w[i][j - 1] + p[j] + q[j];

            for (int r = i; r <= j; r++){
                double temp = e[i][r - 1] + e[r + 1][j] + w[i][j];
                if (temp < e[i][j]){
                    e[i][j] = temp;
                    root[i][j] = k;
                }
            }
        }
    }

    return e[1][n];
}
```

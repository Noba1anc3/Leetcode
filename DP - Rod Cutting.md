```c++
#include <iostream>
#include <vector>
using namespace std;

int Rod_Cutting(vector<int> p, int n){
	vector<int> r(n+1, 0), cut(n+1, 0);
    for (int i = 1; i <= n; i++){
        int revenue = -1;
        for (int j = 1; j <= i; j++){
            if (revenue < p[j-1] + r[i-j]){
                revenue = p[j-1] + r[i-j];
                cut[i] = j;
            }
        }
        r[i] = revenue;
    }

    int N = n;
    while (n > 0){
        cout<<cut[n]<<' ';
        n -= cut[n];
    }
    cout<<'\n';

    return r[N];
}


int main()
{
	vector<int> p = {1, 5, 8, 9, 10, 17, 17, 20, 24, 26};
    int n = 10;
    int maxRevenue = Rod_Cutting(p, n);
    cout<<maxRevenue;
    return 0;
}
```


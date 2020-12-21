```c++
#include <iostream>
#include <algorithm>

using namespace std;

struct Item{
    int value;
    int weight;
    double fraction;
};

bool cmp(Item a, Item b){
    return a.value/a.weight > b.value/b.weight;
}

double Fraction_Knapsack(int n, int *V, int *W, int K)
{
    double total = 0;
    struct Item items[n];
    for (int i = 0; i < n; i++){
        items[i].value = V[i];
        items[i].weight = W[i];
        items[i].fraction = 0;
    }

    sort(items, items + n, cmp);

    while (K > 0){
        for (int i = 0; i < n; i++){
            auto item = items[i];
            if (K >= item.weight){
                items[i].fraction = 1;
                K -= item.weight;
                total += item.value;
            }
            else{
                items[i].fraction = (double)K/item.weight;
                total += items[i].fraction * item.value;
                break;
            }
        }
        break;
    }

    return total;

}
```

Attention

- Struct
- cmp
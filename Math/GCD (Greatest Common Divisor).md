# GCD (Greatest Common Divisor)

```cpp
#include <bits/stdc++.h>
using namespace std;

void naive_gcd()
{
    int a, b;
    cin >> a >> b;
    int size = max(a, b);
    int ans = 1;
    for (int i = 1; i <= size; i++)
    {
        if ((a % i == 0) && (b % i == 0))
        {
            ans = max(ans, i);
        }
    }

    cout << ans;
}

void gcd(int a, int b)
{
    if (a == 0)
    {
        cout << b;
        return;
    }
    else
    {
        return gcd(b % a, a);
    }
}

int main()
{
    int a, b;
    cin >> a >> b;
    if (a > b)
    {
        swap(a, b);
    }
    gcd(a, b);
}

```
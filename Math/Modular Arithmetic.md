# Modular Arithmetic

(a + b)%m = ( (a%m) + (b%m) ) % m;
(a * b)%m = ( (a%m) * (b%m) ) % m;
(a - b)%m = ( (a%m) - (b%m) + m) % m;
(a/b)%m = ( (a%m) * (b^-1) % m) % m;

### Printing answer in modulo 10^9 + 7 helps prevent strong negative numbers.

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
	ll n;
	cin >> n;
	ll a = 1;
	// ll m = 10 ^ 9 + 7;
	// cout << ans % 1000000007;
	for (int i = 1; i <= n; i++){
		a = ((a % 1000000007) * (2 % 1000000007)) % 1000000007;
		// cout << a << " ";
	}
	cout << a;
}

```
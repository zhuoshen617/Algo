O(knlogn)

<details>
  <summary>Click for more Hints!</summary>
  
  有一点数论知识，dp[i][j]表示长度为i，最后一个是数字j的数列个数。
  关键在转移的时候，看j后面能接几，比如3后面能接3，6，9，.. N。这样枚举是调和级数，复杂度是nlogn。

</details>

```
#include "bits/stdc++.h"
using namespace std;
typedef long long LL;
#define INF 0x3f3f3f3f
const int MOD = 1e9+7;
typedef pair<int, int> PII;

const int N = 2005;
const int K = 2005;

int n, k;
int dp[K][N];

int main() {
   
    #ifndef ONLINE_JUDGE
       freopen("input.txt", "r", stdin);
       freopen("output.txt", "w", stdout);
    #endif

    cin >> n >> k;

    for (int j = 1; j <= n; j++) 
        dp[1][j] = 1;
    
    for (int i = 1; i < k; i++) {
        for (int j = 1; j <= n; j++) {
            for (int l = j; l <= n; l += j) {
                dp[i+1][l] = (dp[i+1][l] + dp[i][j]) % MOD;
            }
        }
    }


    int ans = 0;
    for (int j = 1; j <= n; j++) 
        ans = (ans + dp[k][j]) % MOD;

    cout << ans;
    
    return 0;
}
```
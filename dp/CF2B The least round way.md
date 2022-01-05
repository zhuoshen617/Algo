CF的题目十分考察思维。

先统计每一个数字拥有2和5这两个因子的数量。然后如果你把2和5放在一起考虑就走远了。
比如下面这个例子。
3
5 1 5
1 2 1
5 1 4

正确的思路是单独考虑，2和5。找到一条含有2最少路径，再找一条含有5最少路径。
注意在找含有2最少路径时完全不用考虑5的情况。因为最后不管这条2最少的路有几个5，我们看的是2的数量不是5。
你如果担心2最少的路上5更少怎么办，这条路一定会被找一条含有5最少路径是找到。

坑点：有0。如果一条路上有0，则最多1个0。特判一下。

我现在的能力还不足以处理CF题目的思维。加油。

```
#include "bits/stdc++.h"

using namespace std;
typedef long long LL;
#define INF 0x3f3f3f3f
const int MOD = 1e9+7;
typedef pair<int, int> PII;

const int N = 1005;
int n;
int grid[N][N][2], dp2[N][N][2], dp5[N][N][2];

void print0(int x, int y) {
    int i = 1, j = 1;
    while (i < x) {
        cout << "D";
        i++;
    }

    while (j < n) {
        cout << "R";
        j++;
    }

    while (i < n) {
        cout << "D";
        i++;
    }
}

void print(int dp[N][N][2]) {
    int i = n, j = n;

    string path;
    while (i != 1 || j != 1) {
        if (dp[i][j][1] == 0) 
            path += "D", i--;
        else
            path += "R", j--;
    }

    reverse(path.begin(), path.end());
    cout << path;
}

int countNum(int n, int f) {
    int res = 0;
    if (n == 0)
        return res;

    while (n % f == 0) {
        res++;

        n /= f;
    }

    return res;
}

int main() {

    #ifndef ONLINE_JUDGE
       freopen("input.txt", "r", stdin);
       freopen("output.txt", "w", stdout);
    #endif

    memset(dp2, INF, sizeof dp2);
    memset(dp5, INF, sizeof dp5);

    cin >> n;
    bool has0 = false;
    int x0, y0;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            int x;
            cin >> x;

            if (x == 0) {
                has0 = true;
                x0 = i;
                y0 = j;
            }

            grid[i][j][0] = countNum(x, 2);
            grid[i][j][1] = countNum(x, 5);
        }
    }

    dp2[1][1][0] = grid[1][1][0];
    dp5[1][1][0] = grid[1][1][1];

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i == 1 && j == 1)
                continue;

            if (dp2[i-1][j][0] <= dp2[i][j-1][0]) {
                dp2[i][j][0] = dp2[i-1][j][0] + grid[i][j][0];
                dp2[i][j][1] = 0;
            } else {
                dp2[i][j][0] = dp2[i][j-1][0] + grid[i][j][0];
                dp2[i][j][1] = 1;
            }
            
            if (dp5[i-1][j][0] <= dp5[i][j-1][0]) {
                dp5[i][j][0] = dp5[i-1][j][0] + grid[i][j][1];
                dp5[i][j][1] = 0;
            } else {
                dp5[i][j][0] = dp5[i][j-1][0] + grid[i][j][1];
                dp5[i][j][1] = 1;
            }
        }
    }

    if (dp2[n][n][0] > 1 && dp5[n][n][0] > 1 && has0) {
        cout << 1 << endl;
        print0(x0, y0);
        return 0;
    }

    cout << min(dp2[n][n][0], dp5[n][n][0]) << endl;
    if (dp2[n][n][0] <= dp5[n][n][0]) {
        print(dp2);
    } else {
        print(dp5);
    }
 
    return 0;
}
```
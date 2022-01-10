一个贪心的思路。

先对有向边的点拓扑排序，然后给每一个点编号。然后对于每一条无向边，按照编号顺序链接。

这题就是给一个DAG，加上一些边，怎么加才能不产生环。
 

```
#include "bits/stdc++.h"
 
using namespace std;
typedef long long LL;
#define INF 0x3f3f3f3f
const int MOD = 1e9+7;
typedef pair<int, int> PII;
 
const int N = 200010;
const int M = 200010;
 
struct Edges{
    int a, b;
} edges[M];
 
int adjList[N], ne[M], e[M], idx;
int din[N], id[N], topSort[N];
int n, m;
queue<int> q;
 
void add(int a, int b) {
    e[idx] = b;
    ne[idx] = adjList[a];
    adjList[a] = idx++;
}
 
int main() {
 
    #ifndef ONLINE_JUDGE
       freopen("input.txt", "r", stdin);
       freopen("output.txt", "w", stdout);
    #endif
    
    int t;
    cin >> t;
    while (t--) {
        memset(adjList, -1, sizeof adjList);
        memset(din, 0, sizeof din);
        idx = 0;
       
        cin >> n >> m;
 
        for (int i = 0; i < m; i++) {
            int a, b, c;
            cin >> a >> b >> c;
            edges[i].a = b;
            edges[i].b = c;
 
            if (a == 1) {
                add(b, c);
                din[c]++;
            } 
        }
 
        int cnt = 0;
        for (int i = 1; i <= n; i++) {
            if (din[i] == 0) {
                q.push(i);
                topSort[cnt++] = i;
            }
        }  
 
        while (q.size()) {
            int u = q.front();
            q.pop();
 
            for (int i = adjList[u]; ~i; i = ne[i]) {
                int v = e[i];
            
                din[v]--;
                if (din[v] == 0) {
                    q.push(v);
                    topSort[cnt++] = v;
                }
            }
        }
        
        if (cnt != n) {
            puts("NO");
            continue;
        }
 
        puts("YES");
        for (int k = 0; k < n; k++) {
            id[topSort[k]] = k;
        }
       
        for (int i = 0; i < m; i++) {
            int u = edges[i].a;
            int v = edges[i].b;
 
            if (id[u] < id[v])
                cout << u << " " << v << endl;
            else
                cout << v << " " << u << endl;
        }
    }
 
    return 0;
}
```
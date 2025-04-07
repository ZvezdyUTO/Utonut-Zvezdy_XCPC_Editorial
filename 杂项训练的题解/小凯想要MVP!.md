# 小凯想要MVP!

所有者: Zvezdy
标签: 组合数学, 鸽巢原理
创建时间: 2025年4月7日 17:11

组合数学和鸽巢原理的结合，其实本质上是两种增长模式的对冲。已知一种选法和一个和对应，但是选发是组合数，成指数型增长，但是总和只是线性增长，所以当n大到一定程度的时候，选法的数量会远远大于总和，因此这就变成了一个鸽巢原理：一定会有一个溢出，所以就会有两个重复。这也许是一种经典的技巧：当看到需要判断是否有重复元素的时候，就可能使用鸽巢原理，尤其是这种存在一一对应关系和增长速度对冲的。

```cpp
/*
 *  ██╗   ██╗████████╗ ██████╗ ███╗   ██╗██╗   ██╗████████╗
 *  ██║   ██║╚══██╔══╝██╔═══██╗████╗  ██║██║   ██║╚══██╔══╝
 *  ██║   ██║   ██║   ██║   ██║██╔██╗ ██║██║   ██║   ██║
 *  ██║   ██║   ██║   ██║   ██║██║╚██╗██║██║   ██║   ██║
 *  ╚██████╔╝   ██║   ╚██████╔╝██║ ╚████║╚██████╔╝   ██║
 *   ╚═════╝    ╚═╝    ╚═════╝ ╚═╝  ╚═══╝ ╚═════╝    ╚═╝
 *
 *  ███████╗██╗   ██╗███████╗███████╗██████╗ ██╗   ██╗
 *  ╚══███╔╝██║   ██║██╔════╝╚══███╔╝██╔══██╗╚██╗ ██╔╝
 *    ███╔╝ ██║   ██║█████╗    ███╔╝ ██║  ██║ ╚████╔╝
 *   ███╔╝  ╚██╗ ██╔╝██╔══╝   ███╔╝  ██║  ██║  ╚██╔╝
 *  ███████╗ ╚████╔╝ ███████╗███████╗██████╔╝   ██║
 *  ╚══════╝  ╚═══╝  ╚══════╝╚══════╝╚═════╝    ╚═╝
 */
// #pragma GCC optimize(2)
// #pragma GCC optimize(3,"Ofast","inline")
#include <bits/stdc++.h>
using ll = long long;
#define debug(x) std::cout << #x << " = " << x << '\n'

const int N = 2e5 + 5;

int n, m, arr[N];
int vis[3 * N];

int sum = 0, cnt;
bool flag;

void dfs(int i, int cur) {
    if (flag) return;
    if (cur == cnt) {
        if (vis[sum] != 0) flag = true;
        ++vis[sum];
        return;
    }

    for (int it = i; it <= n; ++it) {
        sum += arr[it];
        dfs(it + 1, cur + 1);
        sum -= arr[it];
    }
}

void Main_work() {
    std::cin >> n >> m;
    for (int i = 1; i <= n; ++i) std::cin >> arr[i];
    if (n >= 24) {
        std::cout << "YES\n";
        return;
    }

    sum = 0;
    flag = false;
    for (int i = 1; !flag && i <= n / 2; ++i) {
    std::memset(vis, 0, sizeof(vis));
        cnt = i;
        dfs(1, 0);
    }
    if (flag) {
        std::cout << "YES\n";
    } else {
        std::cout << "NO\n";
    }
}

void init() {
}

signed main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(0), std::cout.tie(0);
    // freopen("test.in", "r", stdin);
    // freopen("test.out", "w", stdout);
    init();
    int Zvezdy = 1;
    std::cin >> Zvezdy;
    while (Zvezdy--) {
        Main_work();
    }
    return 0;
}
```
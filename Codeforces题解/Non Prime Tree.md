# Non Prime Tree

所有者: Zvezdy
标签: 图论, 数学, 构造
创建时间: 2025年4月2日 16:02

要求构造这树上任意相邻两点差值绝对值为合数，题目给出了一个最重要的条件：我们使用的必须是1~2n的数，那完全可以考虑构造偶数差值。独特的构造方案就是把树按层进行分段，然后不同的层使用不同的构造方式，比如奇数层就是2 4 6… 偶数层就是2n 2n-2 2n-4…这样可保证每层的差值都是偶数并且差值是最大化的，但有特殊情况就是最后一个点差值可能为2，那么就需要特判将它改为差值是1。

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
#define int ll
#define debug(x) std::cout << #x << " = " << x << '\n'

void Main_work() {
    int n;
    std::cin >> n;
    std::vector<std::vector<int>> edge(n + 1);

    for (int i = 1, u, v; i < n; ++i) {
        std::cin >> u >> v;
        edge[u].push_back(v);
        edge[v].push_back(u);
    }

    std::vector<bool> vis(n + 1, false);
    std::vector<std::vector<int>> save(2);
    std::queue<std::array<int, 2>> que;
    que.push({1, 1}), vis[1] = true;

    int max = 0;
    while (que.size()) {
        auto [now, deep] = que.front();
        max = std::max(max, deep);
        que.pop();
        save[deep & 1].push_back(now);
        for (auto to : edge[now]) {
            if (!vis[to]) {
                vis[to] = true;
                que.push({to, deep + 1});
            }
        }
    }

    std::vector<int> ans(n + 1);
    for (int i = 0, j = 2 * n; i < save[0].size(); ++i, j -= 2) ans[save[0][i]] = j;
    for (int i = 0, j = 2; i < save[1].size(); ++i, j += 2) ans[save[1][i]] = j;

    int end = max & 1;
    auto& goal = *save[end].rbegin();
    if (std::abs(ans[goal] - ans[edge[goal][0]]) == 2) {
        if (ans[goal] > ans[edge[goal][0]]) {
            --ans[goal];
        } else {
            ++ans[goal];
        }
    }

    for (int i = 1; i <= n; ++i) std::cout << ans[i] << ' ';
    std::cout << '\n';
}

void init() {}

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
# UVA10054 The Necklace

所有者: Zvezdy
标签: 图论, 欧拉回路

珠子是被一个个串起来的，并且一串珠子代表两两个点之间只有一条边，这就是欧拉回路的模板题。

欧拉回路的判断方式是：图连通+所有点都为偶点。这里给出的证明是：每个点都要被进入一次和出去一次，所以度数一定必须是偶数。

在这一题中，我们需要把珠子建成边，而颜色和颜色就是点了，因为珠子可以通过颜色串通起来，所以这里应该是一个点边互换的小操作。在建完图以后发现是有重点和重边的，可以直接不用管。直接跑打印就好，每次删除自己走过的边。

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

    int deg[51]{0}, edge[51][51]{0};

    int col;

    for (int i = 1, l, r; i <= n; ++i) {
        std::cin >> l >> r;
        ++edge[l][r], ++edge[r][l];
        ++deg[l], ++deg[r];
        col = l;
    }

    for (int i = 1; i <= 51; ++i) {
        if (deg[i] & 1) return std::cout << "some beads may be lost\n", void();
    }

    // 现在需要打印一个欧拉回路
    auto print = [&](auto&& self, int now) -> void {
        for (int to = 1; to <= 50; ++to) {
            if (edge[now][to] > 0) {
                --edge[now][to], --edge[to][now];
                std::cout << now << ' ' << to << '\n';
                self(self, to);
            }
        }
    };
    print(print, col);
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
    int cnt = 0;
    while (Zvezdy--) {
        ++cnt;
        std::cout << "Case #" << cnt << '\n';
        Main_work();
    }
    return 0;
}
```
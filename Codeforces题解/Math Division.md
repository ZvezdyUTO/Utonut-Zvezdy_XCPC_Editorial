# Math Division

所有者: Zvezdy
标签: 位掩码, 动态规划, 概率论
创建时间: 2025年4月2日 16:35

概率DP，经过观察其实可以发现，我们的操作次数最多只有n次或者n-1次，这是因为如果我们永远不进位，那么n-1次之后必定得到1，如果发生进位就只会有两种可能：进位在中间，会被0补掉，进位到最后一位，让最后一位进1。因此我们的期望应该是n-1+最后一位进一的概率*1。而当前这一位是否进位的概率只会被它前一位影响，因此简单推导出第i位进一的概率然后进行转移即可。

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

const int MODE = 1e9 + 7;
const int inv = 500000004;

void Main_work() {
    int n;
    std::string s;
    std::cin >> n >> s;

    if (n == 1) {
        std::cout << "0\n";
        return;
    }

    std::vector<int> f(n + 1, 0);

    for (int i = n - 1; i >= 0; --i) {
        if (s[i] == '1') {
            int case1 = (1 - f[i + 1] + MODE) % MODE * inv % MODE;
            int case2 = f[i + 1];
            f[i] = (case1 + case2) % MODE;
        } else {
            f[i] = inv * f[i + 1] % MODE;
        }
    }

    std::cout << (n - 1 + f[1]) % MODE << '\n';
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
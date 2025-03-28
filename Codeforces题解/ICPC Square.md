# ICPC Square

所有者: Zvezdy
标签: 思维, 数学
创建时间: 2025年3月17日 21:48

数学思维，先从它的移动方式开始理解，它只能移动到它的倍数单位，意味着每次移动都是对层数做乘法，根据唯一分解，乘法可以看作往一个质因子集合里面添加新的质因子，那么我们之后到达的楼层一定是当前楼层的倍数，所以对于这么一个集合，可以直接把集合中全部元素除掉共同的因子简化讨论。这样我们就变成了从1层开始。其次对于单次跳跃的限制，可以推断出最高楼层要么是2*D，要么是n，如果最高层是偶数，那么根据不等式，一定可以从max/2处跳跃过来，如果是奇数则需要判断，我们需要找到中间的跳板，这个跳板可以是根据唯一分解打出，一点一点往上累加质因子，或者根据最大值不超过2*D的性质直接找中间一个因子进行双重验证。

技巧还是消除共同因子减少讨论，以及列好不等式，对于中间的情况做出枚举，此外从极端情况入手推导有利条件也非常重要。

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
    int n, d, s;
    std::cin >> n >> d >> s;
    auto check = [](int n, int d) {
        int max = std::min(2ll * d, n);
        if (max % 2) {
            for (int i = 1; i * i <= max; ++i) {
                if (max % i == 0) {
                    int j = max / i;
                    if (std::max(i, max - i) <= d || std::max(j, max - j) <= d) return max;
                }
            }
            return max - 1;
        } else {
            return max;
        }
    };
    std::cout << std::max(s, check(n / s, d / s) * s) << '\n';
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
    // std::cin >> Zvezdy;
    while (Zvezdy--) {
        Main_work();
    }
    return 0;
}
```
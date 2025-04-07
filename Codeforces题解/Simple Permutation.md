# Simple Permutation

所有者: Zvezdy
标签: 数学, 构造
创建时间: 2025年3月27日 16:01

题目需要构造一个排列，并且要让它在一个奇奇怪怪的等式中使得一个其中计算出的素数达标，首先被注意到的就是那个素数的数量，可以说我完全不能从那个奇怪的数量表达式里面看出任何东西，但有一个事情是肯定的，就是我们一定需要构造出足够多的质数满足这个该死的条件。仔细观察题目运算的方式，实际上在经过题目的处理过后，这个数组的第i个元素就变成了前i项元素的平均值，那如果我们让这个平均值等于某个质数，那绝对可以在让质数的数量最多，并且满足我们的条件。这里需要用到一个简单的数学结论——伯特利-切比雪夫定理，对于一个x，[x,2x]的范围上一定存在至少一个质数，因此，我们需要找的就是这个排列里面最中间的那个质数，越中间越好，这样有利于我们用交替的方式构造平均值等于。找到它以后我们就按x, x-1, x+1, x-2, x+2的形式构造，因为向上取整嘛。对于平均值构造这种交替构造的方式可能也是经典了。同时也许这也可能是“构造最直观简单方案”的一种体现。

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

bool isPrime(int x) {
    if (x <= 1) return false;
    for (int i = 2; i * i <= x; ++i) {
        if (x % i == 0) return false;
    }
    return true;
}

void Main_work() {
    int n;
    std::cin >> n;
    std::vector<int> ans;

    auto build = [&](int p) {
        ans.push_back(p);
        for (int i = 1; i <= n; ++i) {
            if (p - i > 0) ans.push_back(p - i);
            if (p + i <= n) ans.push_back(p + i);
        }
    };

    for (int x = 0;; ++x) {
        if (isPrime(n / 2 - x)) {
            build(n / 2 - x);
            break;
        }
        if (isPrime(n / 2 + x)) {
            build(n / 2 + x);
            break;
        }
    }
    for (int i = 0; i < n; ++i) std::cout << ans[i] << ' ';
    std::cout << '\n';
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
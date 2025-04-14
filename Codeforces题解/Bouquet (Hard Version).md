# Bouquet (Hard Version)

所有者: Zvezdy
标签: 数学
创建时间: 2025年4月14日 18:39

最后卡死在了x和x+1分讨那里，据说遇到这种这种看起来像二元线性规划的题目不是分讨调整就是不等式，看来这里是分讨调整了。调整是有策略的，因为x会比x+1塞得更满，所以优先考虑先把x塞了再塞x+1，然后根据余数来推个式子调整，已知一个x变x+1可以填上一个空缺的余数，替换的上限是已塞入的x的数量和剩余的x+1的数量，根据这些打个式子判断还能塞多少就行。

问题在于居然没看出来这种分讨，可能缺乏经验吧。也可能是想的复杂了，因为可能遇到x塞完以后可能还剩很多给x+1塞的位置，这种替换调整固定套路就是先塞小后补大再看换几个小的能平衡吧，大概。

```cpp
void Main_work() {
    int n, m;
    std::cin >> n >> m;
    std::vector<std::array<int, 2>> arr(n);
    for (int i = 0; i < n; ++i) std::cin >> arr[i][0];
    for (int i = 0; i < n; ++i) std::cin >> arr[i][1];
    std::sort(arr.begin(), arr.end());

    // 做法依旧是考虑每两种相邻的花瓣，但是需要讨论
    // 可以拆开讨论，只用一种花瓣或者，用两种花瓣的情况

    int ans = 0;

    for (auto [num, cnt] : arr) {
        int max_cnt = m / num;
        int get = std::min(max_cnt, cnt) * num;
        ans = std::max(ans, get);
    }

    // 大胆的猜测吗？
    // ax+by=m?
    for (int i = 1; i < n; ++i) {
        if (arr[i][0] - arr[i - 1][0] > 1) continue;
        if (arr[i][0] * arr[i][1] + arr[i - 1][0] * arr[i - 1][1] <= m) {
            ans = std::max(ans, arr[i][0] * arr[i][1] + arr[i - 1][0] * arr[i - 1][1]);
        } else {
            // 在背包中，x比x+1能占据更多的格子
            int sum0 = std::min(arr[i - 1][1], m / arr[i - 1][0]);
            int lst = m - arr[i - 1][0] * sum0;
            int sum1 = std::min(arr[i][1], lst / arr[i][0]);

            lst = m - sum0 * arr[i - 1][0] - sum1 * arr[i][0];
            // lst可以用x+1补上
            int more = std::min({sum0, lst, arr[i][1] - sum1});
            ans = std::max(ans, m - lst + more);
        }
    }

    std::cout << ans << '\n';
}
```
# B - Iris and the Tree

所有者: Zvezdy
标签: dfn序, 图论
创建时间: 2025年4月13日 09:20

思路很简单，主要是赛时居然没仔细读题，没发现这棵树的编号是按dfn序编的，导致出了不少问题。既然是按dfn序编的，那按题目给的路径走法，每条边最多会被经过两次，现在来思考一下如何计算这里的贡献。先从单个路径做独立分析，如果一条路没有被固定完，那么它所能捞到的最大资源就是外部未被固定总资源量+自己已经被固定的资源量。如果一条边已经被固定住，那它也就可以被退休了。

最重要的应该是如何去实现它，这里有很多技巧。我们每次需要修改某条边，那我们就非常有必要知道每条边的影响范围，在这里我们需要找到如何表达一条路和一条边，对于边来说用题目所给出的方案标记即可，但是对于路，可以选择去标记终点。因为当我们从上一条路末尾走到当前路末尾的过程，我们可以一路上自然地将经过的路收集在动态数组中，最后到终点时再取出来录入影响集合并清空缓存。

有了每条边能影响的路径映射，我们就可以考虑开始处理询问了，我们可以把我们要算的和拆成几部分来计算，之前说了对于一条边，如果它没被固定完，就是外部资源+自己固定，所以对于全局来说，我们可以设出目前哪些被固定了，哪些没被固定，以及还有多少条路径是没被固定的，直接求解即可。

```cpp
void Main_work() {
    int n, w;
    std::cin >> n >> w;

    std::vector<std::vector<int>> edge(n + 1);
    for (int v = 2, u; v <= n; ++v) {
        std::cin >> u;
        edge[u].push_back(v);
    }

    std::vector<std::vector<int>> etor(n + 1), g(+1);
    std::vector<int> path, cnt(n + 1), sum(n + 1);

    auto dfs = [&](auto&& self, int now) -> void {
        for (auto i : path) {
            etor[i].push_back(now);
            ++cnt[now];
        }
        path.clear();

        for (auto to : edge[now]) {
            path.push_back(to);
            self(self, to);
            path.push_back(to);
        }
    };
    dfs(dfs, 1);
    for (auto i : path) {
        etor[i].push_back(1);
        ++cnt[1];
    }

    int ans = 0, tmp = 0, tot = n, s = 0;
    for (int i = 2; i <= n; ++i) {
        int x, y;
        std::cin >> x >> y;
        w -= y;

        for (auto now : etor[x]) {
            tmp -= sum[now];
            sum[now] += y;

            if (--cnt[now] == 0) {
                ans += sum[now];
                --tot;
            } else {
                tmp += sum[now];
            }
        }

        std::cout << ans + tot * w + tmp << ' ';
    }
    std::cout << '\n';
}
```
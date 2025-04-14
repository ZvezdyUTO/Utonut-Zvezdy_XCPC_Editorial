# Gleb and Boating

所有者: Zvezdy
标签: 思维, 数学
创建时间: 2025年4月13日 11:26

对于一道题，通常可能需要配合一些经验加上手玩得出结论，手玩的过程其实也就是分类讨论的过程，对于分讨来说，最重要的就是当我们模拟到某一步的时候，清楚自己下一步能做什么，同时为了防止情况过于复杂，还需要打下草稿来记录我们的过程，最好是一种树形的草稿来记录。

对于此题，最重要的是发现回退一次后的实际影响，不能发现掉头后，我们可以选择走一步或者走多步再掉头回来，而我们每往后走x步再折返x步，就等于在原处回退了x步，发现这个以后就可以大胆猜测，最极限情况下，n%k==k-1，所以只要n≥k*k，那么答案一定就是k-2或者k或者1。因此可以讨论n≤k^2以内的情况，大概可以k^3求解这种暴力覆盖问题，然后拿bitset卡一下常，就美美结束，而且bitset天生适合这种可达问题。

重点在于重新发现手玩对解题的重要性，以及题目如果过于复杂的时候，记录草稿对分析的优化。还有就是可达问题使用bitset优化，其实非常简单就是了。

```cpp
void Main_work() {
    int n, k;
    std::cin >> n >> k;
    if (n % k == 0) return std::cout << k << '\n', void();
    if (k * k <= n) return std::cout << std::max(1ll, k - 2) << '\n', void();

    std::vector<std::tr2::dynamic_bitset<>> f(k + 1);
    for (int i = 0; i <= k; ++i) f[i].resize(n + 1);

    for (int i = 0; i <= n; i += k) f[k][i] = true;

    for (int i = k, dir = 1; i >= 2; --i, dir ^= 1) {
        if (f[i][n]) return std::cout << i << '\n', void();

        if (dir) {
            for (int j = 0; j <= n; j += i - 1) f[i - 1] |= (f[i] >>= i - 1);
        } else {
            for (int j = 0; j <= n; j += i - 1) f[i - 1] |= (f[i] <<= i - 1);
        }
    }

    std::cout << "1\n";
}
```
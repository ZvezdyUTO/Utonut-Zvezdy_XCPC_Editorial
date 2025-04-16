# Zebra-like Numbers

所有者: Zvezdy
标签: 数学, 记忆化搜索
创建时间: 2025年4月13日 17:24

如果遇到这种从单位元素可以被递归构造上来的序列，那一般从最大数开始进行分解一定是最优的，考虑到这点以后，这一题就完全可以从最大数那里开始分解。显然这些斑马数是不能严格变为某种进制的，但我们可以使用从最大开始分解这个条件逆向构造出所有的斑马数，重点在于选最大值。我们的目标是选出limit以内的，斑马值为cnt的数，并且当前考虑的最高位为i，因为它严格按照最高位一直分解，所以我们是有几种选择的：1.如果数依旧足够大，我们考虑继续把cnt给一位给最高位。2.如果不够大或者不想给最高位了，可以放弃掉第i位的斑马数，转而构造最大斑马数是i-1的情况，此时需要注意limit应该减少至idx[i]-1,因为不可能大于等于下一位斑马值。就只是从这两种情况进行转移，然后每次都让cnt-1，可以保证，因为最低位为1，所以如果递归到i和cnt为0时，证明我们构造出了一种合法方案，直接返回即可，非法返回0。另外注意到这一题中有一个状态是非常大的，但考虑到斑马数只有30个，不太可能会爆，所以可以用std::map来记录状态。

```cpp
void Main_work() {
    int l, r, k;
    std::cin >> l >> r >> k;
    if (k >= 120) return std::cout << "0\n", void();

    std::vector<int> idx(1, 1);  // max=3e17
    while (true) {
        int next = *idx.rbegin() * 4ll + 1;
        if (next > 1e18) break;
        idx.push_back(next);
    }
    int p = idx.size();

    std::array<std::array<std::map<int, int>, 125>, 35> save;

    auto f = [&](auto&& self, int i, int cnt, int num) -> int {
        if (cnt < 0 || num < 0) return 0ll;
        if (i == -1) return (num == 0 && cnt == 0);
        if (save[i][cnt].count(num)) return save[i][cnt][num];

        int res = 0, tmp_num = num, tmp_cnt = cnt;

        while (tmp_num >= idx[i]) {
            res += self(self, i - 1, tmp_cnt, idx[i] - 1);
            tmp_cnt -= 1;
            tmp_num -= idx[i];
        }
        res += self(self, i - 1, tmp_cnt, tmp_num);

        return save[i][cnt][num] = res;
    };

    int up = f(f, p - 1, k, r);

    for (int i = 0; i <= 30; ++i) {
        for (int j = 0; j <= 120; ++j) {
            save[i][j].clear();
        }
    }

    int down = f(f, p - 1, k, l - 1);

    std::cout << up - down << '\n';
}
```
# Squaring

所有者: Zvezdy
标签: 动态规划, 数学
创建时间: 2025年4月14日 18:39

只要相邻两个数满足条件，整体一定就满足条件。因为增长速度可能会很大，所以使用f[i]来存储每个数实际上被平方了几次。这里有一个小结论：如果a<b并且a^2>b，那么无论它们同时被平方几次，都会有A^2>B，因为每次a^2后一定比b^2中的一个因子大，以此得证。另外我们需要而的一个显而易见的结论就是如果a<b，那么a^2<b^2。

因此来考虑如何计算相邻的代价，选择分类讨论，如果是a[i-1]<a[i]，那么就先把a[i]开平方到之前所提的范围内，记录被开平方了cnt次，那f[i]=f[i-1]-cnt，因为就当做是前面那个少平方cnt次。如果是a[i-1]>a[i]，那也如法炮制，让a[i]平方cnt次直到大于前面的数，接着f[i]=f[i-1]+cnt。

重点就是推导出关于平方的那个数学结论吧，没啥好说的，因为那个数学结论的可逆性，所以就算那个a和b被开方，也符合那个条件，因此我们才能使用开方操作逆向这个过程，把这个过程看成一条完整的操作链就行。

```cpp
void Main_work() {
    int n;
    std::cin >> n;
    std::vector<int> arr(n);
    for (int i = 0; i < n; ++i) std::cin >> arr[i];

    for (int i = 0, max = 0; i < n; ++i) {
        max = std::max(max, arr[i]);
        if (arr[i] < max && arr[i] == 1) return std::cout << "-1\n", void();
    }

    std::vector<int> f(n);
    f[0] = 0;

    for (int i = 1; i < n; ++i) {
        int tmp = arr[i], cnt = 0;

        if (tmp > arr[i - 1]) {
            while (arr[i - 1] != 1 && std::sqrt(tmp) >= arr[i - 1]) {
                tmp = std::sqrt(tmp);
                --cnt;
            }
        } else {
            while (tmp < arr[i - 1]) {
                tmp *= tmp;
                ++cnt;
            }
        }

        f[i] = std::max(0ll, f[i - 1] + cnt);
    }

    std::cout << std::accumulate(f.begin(), f.end(), 0ll) << '\n';
}
```
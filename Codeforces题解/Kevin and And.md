# Kevin and And

所有者: Zvezdy
标签: 数学, 状态压缩
创建时间: 2025年3月12日 10:42

数学题。如果正常分析的话，首先会想到每个数与上下面的候选列表，是单调递减的，并且根据我们选数顺序的不同，原数递减的速度也会有差异。因为m很小只有10，所以我们暴力枚举都能枚举出第i个数操作j次后所能变成的最小值。那有了这些条件以后，其实我们如果需要拿到f[i][j]，就必须拿f[i][j-1]，变成了有依赖性的树形背包。不过时间复杂度太大不优。

再来考虑这题的数学情况，首先研究可以发现，因为按位与的二进制性质，如果我们每次都遵循优先消除最大化的话，那么最高位一定是被优先消除的，这意味着我们的递减函数其实是一个凸函数，也就是说求导后函数递减。这意味着我们每执行一步新的操作，消除掉的值一定比上一次要少。那我们完全可以拿这些新消除的值进行排序，并且优先选大的，因为保证每一位操作往后都是单调递减，所以选到后面的操作之前一定已经选了前面的操作。

因此，在以后的操作中，需要留意性质有：我们的操作带来的变化，可否使用函数图像进行表示？如果使用函数图像，图像能为我们带来什么性质？比如极大值极小值，以及求导后观察变化趋势。如果是单调函数可以观察凹凸性，一般来说有凹凸性的函数就可以使用贪心策略。另外，当操作顺序的依赖可以使用数学性质隐式保证的时候，直接排序贪心即可。另外就是观察操作的本质，此操作为二进制消除，按位与消除的是二进制的某一位，所以如果我们每次取最优，一定是消除最高位，进制计数下最高位严格压所有低位一头，因此可以发现这个函数的求导函数递减。
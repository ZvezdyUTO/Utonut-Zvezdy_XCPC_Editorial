# How Does the Rook Move?

所有者: Zvezdy
标签: 动态规划, 数学
创建时间: 2024年4月22日 00:24

可以只看横坐标不关心纵坐标，因为电脑会映射玩家的步法。但考虑一种特殊的情况就是玩家把棋放在对角线，电脑没办法跟随。按这个思路，如果我们不放在对角线那么每次都会损失两个横行的格子，如果放在对角线则损失一个横行的格子。

其实在预处理中，不管我们的每一步棋放在哪或者顺序如何都不影响接下来的操作，比如在12345格子中23格被占了，其实我们可以把145缩成123，因为我们依旧可以放（1,4）等位置，并且只考虑横行所以如此缩进完全合理。

最后可以发现，无论题目如何操作，我们都可以事先打表预处理好数据表示当前剩余n个横行所有的情况，考虑排列组合所带来的可能，这题最方便的就是开一个DP数组递归调用我们求解出的答案，推导过程如下：

给n个数，每次从中抽取两个不同的组队，问有多少种组合方法。

每多出一行，那么我们有一些选择：一是把对角线补满，那么就直接加上dp[i-1]就好，二是拿新的那个格子和之前的某个格子组合，那就得弄个空格子出来，于是是dp[i-2]*(i-1)，这里有一点排列组合的运算方式在里面。考虑黑白棋可互换位置，要乘2。

**最后得出状态转移方程：dp[i]=dp[i-1]+2*(i-1)*dp[i-2]; 这回狠狠被数学宰了**

```cpp
/* ★ _____                           _         ★*/
/* ★|__  / __   __   ___   ____   __| |  _   _ ★*/
/* ★  / /  \ \ / /  / _ \ |_  /  / _  | | | | |★*/
/* ★ / /_   \ V /  |  __/  / /  | (_| | | |_| |★*/
/* ★/____|   \_/    \___| /___|  \__._|  \__, |★*/
/* ★                                     |___/ ★*/
//#pragma GCC optimize(2)
//#pragma GCC optimize(3,"Ofast","inline")
#include<bits/stdc++.h>
using namespace std;
#define int long long
#define ld long double
#define ll long long
#define fi first
#define se second
#define maxint 0x7fffffff
#define maxll 9223372036854775807
#define all(v) v.begin(), v.end()
#define debug(x) cout<<#x<<"="<<x; endll
#define save(x) std::cout << std::fixed << std::setprecision(x)
#define FOR(word,begin,endd) for(auto word=begin;word<=endd;++word)
#define ROF(word,begin,endd) for(auto word=begin;word>=endd;--word)
#define cmp(what_type) function<bool(what_type,what_type)>
#define r(x) cin>>x
#define s(x) cout<<x
#define cint(x) int x;cin>>x
#define cchar(x) char x;cin>>x
#define cstring(x) string x;cin>>x
#define cll(x) ll x; cin>>x
#define cld(x) ld x; cin>>x
#define pque priority_queue
#define umap unordered_map
#define uset unordered_set
#define endll cout<<endl
#define __ cout<<" "
#define dot pair<int,int>
ll dp[300005];
void solve(){
    cint(n); cint(k);
    while(k--){
        cint(a); cint(b);
        if(a==b) --n;
        else n-=2ll;
    }
    s(dp[n]);endll;
}
signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
//    freopen("test.in", "r", stdin);
//    freopen("test.out", "w", stdout);
    int TTT; cin>>TTT;
//    int TTT=1;
    dp[0]=1ll; dp[1]=1ll; dp[2]=3ll;
        FOR(i,3,300000){ 
            dp[i]=dp[i-1]+2*(i-1)*dp[i-2];
            dp[i]%=1000000007ll;
    }
    while(TTT--){solve();}
    return 0;
}
```
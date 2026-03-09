---
title: 算法Learing
date: 2026-03-03T10:00:00+08:00
draft: false
summary: 蓝桥杯C/C++软件赛40天备赛汇总
tags:
  - 大三下
  - 算法
  - 软件
categories:
  - 笔记
Toc: true
math: true
---
# 贪心

# 枚举

## 区间枚举

- [p8600]连号区间：寻找隐藏等式关系（**区间内最大值 - 最小值 = 区间长度 - 1**）避免排序。
# 模拟

- [p8598]断号、重号/比较水的模拟：建立哈希表确定出现次数
# 搜索

## 全排列-带分数[p8599]

> 遇到全排列时，可以通过**DFS**和**next_permutaion函数**生成全排列
### next_permutation函数

`next_permutation` 是 C++ STL `<algorithm>` 头文件中的函数，**作用是将数组 / 容器的元素修改为 “字典序下的下一个更大排列”**，同时返回**布尔值**表示是否生成成功。

```
template <class BidirectionalIterator>
bool next_permutation (BidirectionalIterator first, BidirectionalIterator last);
```

- **第一个参数 `first`**：要处理的序列的**起始迭代器 / 指针**（对应数组的起始位置）。
- **第二个参数 `last`**：要处理的序列的**结束迭代器 / 指针**（对应数组的 “尾后位置”，即最后一个元素的下一个位置）。

如果处理的是数组 `a`，且要处理的元素是 `a[1]~a[9]`（比如之前的全排列题），那么：

- `first` 就是 `a + 1`（指向第一个要处理的元素）；
- `last` 就是 `a + 10`（指向第 9 个元素的下一个位置，即 `a[10]`）。
- 当函数**原地修改**到最大排列时，返回**False**
## 记忆化搜索

### 网格DFS-地宫寻宝[p8612]
>💡网格DFS和树的DFS核心原理相同：**边界条件+分支状态+终止条件**
>     本题的区别在于分支由**子节点->向左向下两个方向**、**多了拿/不拿两个子状态**、**终止条件为统计符合条件的路径数量**
>    这必然会导致重复统计会超时（**时间复杂度为$2^{m+n}$**），故引入记忆化搜索（**核心就是多了个memo来储存之前走过的路径**）
- 暴力DFS
```cpp
void DFS(x, y, max, num)
{
  //边界条件
  //边界1
  if(x == m + 1 || y == n + 1) return;
  //边界2
  if(x == m && y == n)
  {
    if(num == k || ( num == k - 1 && cost[m][n] > max)
    {
    cnt++;
    }
    return;
  }
  
  //分支1：向右
  //子状态1：不拿
  DFS(x+1, y, max, n);
  //子状态2：拿
  if(cost[x][y] > max)
  DFS(x+1, y, cost[x][y], n+1);
    
  //分支2：向下
  //子状态1：不拿
  DFS(x, y+1, max, n);
  //子状态2：拿
  if(cost[x][y] > max)
  DFS(x, y+1, cost[x][y], n+1);
}
```
### 记忆化搜索(缓冲数组)
>💡由于每次递归都要回到起点会超时，故我们需要增加一个缓冲四维数组$dp[x][y][max][num]$任意位置下，到达终点满足条件的线路数量->**这样使得每一次不用再回到原点**



# 树

## 树的直径[p8602]

>求树的直径的方法是：**两次DFS/BFS**

### BFS（中心扩散）
- 基本逻辑：不断**进入退回**，记录所有分岔路的信息（利用队列，先进先出）
- 板子：（BFS 是迭代实现，适合节点数多的场景，避免 **DFS 递归栈溢出**问题）

```cpp
// 邻接表：adj[u] 存储 {v, w}，v是邻接点，w是边权
vector<pair<int, int>> adj[100];
int dist[100]; // 存储每个节点到起点的距离
int n; // 节点总数

// BFS求起点start的最远节点和对应距离，返回值：{最远节点编号, 最远距离} 
pair<int, ll> bfs(int start) 
{   // 步骤1：初始化距离数组和队列（核心：标记是否访问过 + 记录距离） 
    memset(dist, -1, sizeof(dist)); // -1 = 未访问，0 = 起点距离 
    queue<int> q; // BFS的核心容器：队列（先进先出） 
    q.push(start); // 第一步：把起点放进队列 
    dist[start] = 0; // 起点到自己的距离是0 

    // 步骤2：初始化结果变量（记录最远节点和距离） 
    int far_node = start; // 初始最远节点是起点 
    ll max_dist = 0; // 初始最远距离是0 

   // 步骤3：核心循环（队列不空就继续扩散） 
   while (!q.empty()) 
   { 
    // 步骤3.1：取出队列头部的节点（当前扩散的中心）
    int u = q.front(); 
    q.pop(); // 取出后从队列移除
   // 步骤3.2：遍历当前节点的所有邻边（扩散到邻居） 
   for (auto &edge : adj[u]) 
   { 
    int v = edge.first; // 邻居节点编号 
    ll w = edge.second; // 从u到v的边权（距离） 
    // 步骤3.3：只处理「未访问过」的节点（避免重复/走回头路） 
    if (dist[v] == -1) 
    { // 关键：更新v的距离 = u的距离 + 边权 
     dist[v] = dist[u] + w; 
     // 步骤3.4：更新最远节点和距离（核心！） 
    if (dist[v] > max_dist) { max_dist = dist[v]; // 刷新最远距离 far_node = v; // 刷新最远节点 } 
    // 步骤3.5：把v放进队列，作为下一轮扩散的中心 q.push(v); } } } // 步骤4：返回最终结果      return {far_node, max_dist}; }

// 求树的直径主函数
ll get_tree_diameter() {
    // 第一步：找任意节点（如1）的最远节点u
    int u = bfs(1).first;
    // 第二步：找u的最远节点v，u-v的距离就是直径
    ll diameter = bfs(u).second;
    return diameter;
}

// 测试用例（适配之前的路费问题）
int main() {
    cin >> n;
    // 建图：输入n-1条边（p, q, d），无向边
    for (int i = 0; i < n - 1; ++i) {
        int p, q;
        ll d;
        cin >> p >> q >> d;
        adj[p].emplace_back(q, d);
        adj[q].emplace_back(p, d);
    }
    
    ll diameter = get_tree_diameter();
    // 路费计算：(L² + 21L)/2
    ll cost = (diameter * diameter + 21 * diameter) / 2;
    cout << "树的直径：" << diameter << endl;
    cout << "最大路费：" << cost << endl;
    
    return 0;
}
```
>   💡**adj[p].emplace_back(q, d);adj[q].emplace_back(p, d);** 是邻接表的构建

### DFS（递归+终止）
- 基本逻辑：一条路走到底（**递归**），走不通往回退走另一条（**回溯**）（利用栈，先进后出）
- 板子
```cpp
// DFS递归函数：遍历u的所有邻边，更新距离 
void dfs(int u, int parent) 
{ // parent避免走回父节点 
  for (auto &edge : adj[u]) 
  { 
  int v = edge.first; 
  ll w = edge.second; 
  if (v != parent) 
  { // 排除父节点，避免环 
  dist[v] = dist[u] + w; 
  dfs(v, u); 
  } 
  } 
} 
// DFS求起点start的最远节点和距离
pair<int, ll> dfs_get_far(int start) 
{ memset(dist, 0, sizeof(dist)); 
  dfs(start, -1); // 父节点设为-1（无父节点） 
  int far_node = start; 
  ll max_dist = 0; 
  for (int i = 1; i <= n; ++i) 
  { 
    if (dist[i] > max_dist) 
     { 
      max_dist = dist[i]; 
      far_node = i; 
      } 
  } 
  return {far_node, max_dist}; 
  }
```
### 树的直径的思路
- 从任意节点（如节点 1）出发，进行一次 BFS 或 DFS，找到距离它最远的节点 `u`。
- 再从节点 `u` 出发，进行一次 BFS 或 DFS，找到距离它最远的节点 `v`。
- `u` 和 `v` 之间的路径就是树的直径，其长度就是 `L_max`。

# 逆序对-小朋友排队[p8613]

>💡对于身高排序（交换相邻的元素）问题，实质上就是在**消灭逆序对**。
>如(3,1,2,4)，存在的逆序对有(3,1),(3,2)。而**每个元素交换的次数=该元素参与的逆序对数，总排序次数=总逆序对数****。
>计算逆序对数的方法有两个：**树状数组**和**归并排序**（如果使用冒泡排序时间复杂度为$O(n^2)$，而归并/树状数组时间复杂度为$O(nlogn)$

## 树状数组

树状数组实质上利用了二进制分解构建的数组$tree[x]$，有**两大法宝**：前缀和查询和单点增加。

树状数组中每一个元素值$tree[x]$和我的序列$num[x]$的关系是：

$$tree[x]=\sum_{i=x-lowbit(x)+1}^{x}num(i)$$
**其中$lowbit$是一个数二进制分解的最小次幂且其数值等于对于树状数组单元的区间长度**。如$$tree(4)=\sum[1,4]$$
$$length[tree(4)]=4=lowbit(4)$$
- 查询前缀和：实质上是在不断**减去lowbit**，直到求和覆盖所有不重叠子区间。
```cpp
int ask(int x)
{
    int ans = 0;
    for(;x;x-=x&-x) ans+=tree[x];
    return ans;
}
```
- 单点增加：实质上是不断**加上**lowbit，直到每个含有这个元素的tree都能加上这个值。
```cpp
void add(int x,int y)
{
    for(;x<=N;x+=x&-x) tree[x]+=y;
}
```
>如$7 = 2^2 +2 +1$
>分成$[1,4],[5,6],[7,7]$三个区间且$lowbit(4) =4,lowbit(6)=2,lowbit(7)=1$
- 典型应用就是求逆序对的数量：
  1. 离散化排名处理（避免序列数值跳跃过大，空间浪费）
>⭐sort函数
>⭐lower_bound函数 $lower_bound(起始地址, 结束地址, 目标值)$;**寻找升序排列的下标**

  2. 逆序遍历时查询前缀和（**记录右侧已遍历元素中比当前元素小的数量**）和单点增加（**每个逆序对的两个元素统计数目加1**）
```cpp
for(int i = n; i; i--)
{
    ans += ask(num[i]-1);
    add(a[i],1);
}
```
# 归并排序

# 动态规划DP

- DP的主要**三步**：
1. **定义状态**：明确 `dp[i]`（或 `dp[i][j]`）代表什么含义
2. **确定初始条件**：给出最基础的、无需推导的状态值（比如 `dp[0] = 0`）；
3. **推导状态转移方程**：找到 `dp[i]` 和前面的状态（如 `dp[i-1]`、`dp[i-2]`）之间的数学关系。

>最直观的例子是斐波那契数列：
> **状态定义**：`dp[i]` 表示第 i 个斐波那契数；
> **初始条件**：`dp[1] = 1`，`dp[2] = 1`；
   **状态转移方程**：`dp[i] = dp[i-1] + dp[i-2]`（核心！用前两个状态推当前状态）。

## 波动数列[p8614]

此题为比较经典容易（很容易推导出状态转移方程）的DP题目，对于每一步选择+a或者-b寻找满足总和为S的所有状态，**初始状态和定义**、**状态转移方程**和**终止条件**为：
$$dp[0][0]=0 （dp[i][j] 表示第i步时的加权和）$$
$$dp[i][j]=dp[i-1][j-a·i]+dp[i-1][j+b·i]$$
$$s\%n=j\%n$$
```cpp
#include <bits/stdc++.h>
using namespace std;
#define mod 100000007
typedef long long ll;
int n, s, a, b;
int dp[1007][1007];
inline int c(int x) {
	return (x % n + n) % n;//⭐求余数时同时能保证其为正数
}
int main() {
	cin >> n >> s >> a >> b;
	dp[0][0] = 1;
	for(int i = 1; i < n; i++)
		for(int j = 0; j < n; j++)
			dp[i][j] = (dp[i - 1][c(j - a * i)] + dp[i - 1][c(j + b * i)]) % mod;
	cout << dp[n - 1][c(s)];
	return 0;
}

```


## 垒骰子+矩阵乘法/数（矩阵）的快速幂[p8624]


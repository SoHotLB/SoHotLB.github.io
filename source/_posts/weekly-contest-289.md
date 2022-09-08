---
title: LeetCode第289场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第289场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-289/

### [一、计算字符串的数字和](https://leetcode-cn.com/problems/calculate-digit-sum-of-a-string/)

#### 题解：模拟

不断迭代字符串，如果长度超过k则进行划分然后按照题意求和相加，直到最终字符串长度小于k，结束循环。

注意：最后一组可能长度小于k，所以需要和字符串长度比较选择最小值（防止越界）。

**C++代码：**

```cpp
class Solution {
public:
    string digitSum(string s, int k) {
        int n=s.size();
        while(n>k){
            string str="";
            for(int i=0;i<n;i+=k){
                int num=0;
                for(int j=i;j<min(i+k,n);j++){
                    num+=(s[j]-'0');
                }
                str+=to_string(num);
            }
            s=str;n=s.size();
        }
        return s;
    }
};
```

### [二、完成所有任务需要的最少轮数](https://leetcode-cn.com/problems/minimum-rounds-to-complete-all-tasks/)

#### 题解：贪心+哈希

由于每次可以完成2个或者3个相同难度级别的任务，于是可以知道：假设难度级别为i的任务有n个，

- 如果n==1，则始终无法完成任务
- n\==2 或n==3，需要一次便可完成
- n>=3时，完成次数以3为周期（即：4,5,6需要2次，7,8,9需要3次...）

于是对于n>=3的情况，只需要向上取整即可（为方便处理，只需要(n+2)/3即可）

同时使用哈希表存储每个难度级别对应的任务数量。

**C++代码：**

```cpp
class Solution {
public:
    int minimumRounds(vector<int>& tasks) {
        int n=tasks.size();
        map<int,int>mp;
        for(int i=0;i<n;i++){
            mp[tasks[i]]++;
        }
        int ans=0;
        for(auto it:mp){
            if(it.second==1) return -1;
            ans+=(it.second+2)/3;
        }
        return ans;
    }
};
```

### [三、转角路径的乘积中最多能有几个尾随零](https://leetcode-cn.com/problems/maximum-trailing-zeros-in-a-cornered-path/)

#### 题解：前缀和+枚举

> 参考链接：https://leetcode-cn.com/problems/maximum-trailing-zeros-in-a-cornered-path/solution/by-tsreaper-ukq5/

需要求乘积中最多尾随零的个数，只需要转化为每个数的因子对应的 2和5的和的最小值即可。

做乘法运算时，随着乘数的增加，尾随零的个数只会增加不会减少，于是可以从二维数组的其中一个角出发，到另一个角结束（期间只拐一次弯，不走已经走过的单元格）

可以先用前缀和存储每一行和每一列因子2和因子5的个数，再枚举拐点计算答案。

> 还是比较复杂....

**C++代码：**

```cpp
class Solution {
public:
    int maxTrailingZeros(vector<vector<int>>& grid) {
        int n=grid.size();int m=grid[0].size();
        //每一行、每一列2和5的数量的前缀和
        vector<vector<int> > r2(n+1),c2(n+1),r5(n+1),c5(n+1);
        for(int i=0;i<=n;i++){
            r2[i]=c2[i]=r5[i]=c5[i]=vector<int>(m+1);
        }
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                int cnt2=0,cnt5=0;
                int now=grid[i-1][j-1];
                while(now%5==0) cnt2++,now/=2;
                while(now%5==0) cnt5++,now/=5;
                r2[i][j]=r2[i][j-1]+cnt2;
                c2[i][j]=c2[i-1][j]+cnt2;
                r5[i][j]=r5[i][j-1]+cnt5;
                c5[i][j]=c5[i-1][j]+cnt5;
            }
        }
        int ans=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                //左--->上方结束
                ans=max(ans,min(r2[i][j]+c2[i-1][j],r5[i][j]+c5[i-1][j]));
                //左--->下方结束
                ans=max(ans,min(r2[i][j]+c2[n][j]-c2[i-1][j],r5[i][j]+c5[n][j]-c5[i][j]));
                //右--->	上方结束
                ans=max(ans,min(r2[i][m]-r2[i][j]+c2[i][j],r5[i][m]-r5[i][j]+c5[i][j]));
                //右--->下方结束
                ans=max(ans,min(r2[i][m]-r2[i][j]+c2[n][j]-c2[i-1][j],r5[i][m]-r5[i][j]+c5[n][j]-c5[i-1][j]));
            }
        }
        return ans;
    }
};
```

### [四、相邻字符不同的最长路径](https://leetcode-cn.com/problems/longest-path-with-different-adjacent-characters/)

#### 题解：树的直径（树形dp）

题目需要求任意相邻结点不取相同字符的最长路径，便可转化为：对于每一个结点，以该结点为根节点，在满足题意的情况下求子树的直径。然后不断回溯，从而求出整棵树在满足题意情况下的最长直径。

通过dfs遍历即可。

注意：需要将如：字符串、存储图（树）的二维数组等比较大的变量作为全局变量~~（别问怎么知道的...）~~

具体见代码注释。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> edge[100010];
    int dis[100010];     //dis[i]：表示以结点i为根结点的最大深度
    string str;
    int ans=0;
    void dfs(int u){   //找出以当前结点为根节点的最大深度和次大深度
        int second_dis=0;    //次大深度
        dis[u]=0;
        for(int i=0;i<edge[u].size();i++){
            int v=edge[u][i];
            dfs(v);
            if(str[u]==str[v]) continue;
            if(dis[v]>dis[u]){   //求以u结点为根结点的最大深度
                second_dis=dis[u];
                dis[u]=dis[v];
            }else{
                second_dis=max(second_dis,dis[v]);   //更新次大深度
            }
        }
        dis[u]++;   //在上述操作中，并未考虑自身结点也在路径中
        ans=max(ans,dis[u]+second_dis);
    }
    int longestPath(vector<int>& parent, string s) {
        int n=parent.size();str=s;
        for(int i=1;i<n;i++){
            edge[parent[i]].push_back(i);
        }
        dfs(0);
        
        return ans;
    }
};
```


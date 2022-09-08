---
title: LeetCode第280场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第280场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-280/

### [一、得到 0 的操作数](https://leetcode-cn.com/problems/count-operations-to-obtain-zero/)

#### 题意

给你两个 非负 整数 num1 和 num2 。

每一步 操作 中，如果 num1 >= num2 ，你必须用 num1 减 num2 ；否则，你必须用 num2 减 num1 。

例如，num1 = 5 且 num2 = 4 ，应该用 num1 减 num2 ，因此，得到 num1 = 1 和 num2 = 4 。然而，如果 num1 = 4且 num2 = 5 ，一步操作后，得到 num1 = 4 和 num2 = 1 。
返回使 num1 = 0 或 num2 = 0 的 操作数 。

**示例1：**

> 输入：num1 = 2, num2 = 3
> 输出：3
> 解释：
> - 操作 1 ：num1 = 2 ，num2 = 3 。由于 num1 < num2 ，num2 减 num1 得到 num1 = 2 ，num2 = 3 - 2 = 1 。
> - 操作 2 ：num1 = 2 ，num2 = 1 。由于 num1 > num2 ，num1 减 num2 。
> - 操作 3 ：num1 = 1 ，num2 = 1 。由于 num1 == num2 ，num1 减 num2 。
> 此时 num1 = 0 ，num2 = 1 。由于 num1 == 0 ，不需要再执行任何操作。
> 所以总操作数是 3 。
>

**示例2：**

> 输入：num1 = 10, num2 = 10
> 输出：1
> 解释：
> - 操作 1 ：num1 = 10 ，num2 = 10 。由于 num1 == num2 ，num1 减 num2 得到 num1 = 10 - 10 = 0 。
> 此时 num1 = 0 ，num2 = 10 。由于 num1 == 0 ，不需要再执行任何操作。
> 所以总操作数是 1 。
>

**提示：**

- $0 <= num1, num2 <= 10^5$

#### 题解

直接按照题意模拟即可。

**C++代码：**

```cpp
class Solution {
public:
    int countOperations(int num1, int num2) {
        int ans=0;
        while(num1!=0&&num2!=0){
            if(num1>=num2) num1-=num2;
            else num2-=num1;
            ans++;
        }
        return ans;
    }
};
```

### [二、使数组变成交替数组的最少操作数](https://leetcode-cn.com/problems/minimum-operations-to-make-the-array-alternating/)

#### 题意

给你一个下标从 0 开始的数组 nums ，该数组由 n 个正整数组成。

如果满足下述条件，则数组 nums 是一个 交替数组 ：

nums[i - 2] == nums[i] ，其中 2 <= i <= n - 1 。
nums[i - 1] != nums[i] ，其中 1 <= i <= n - 1 。
在一步 操作 中，你可以选择下标 i 并将 nums[i] 更改 为 任一 正整数。

返回使数组变成交替数组的 最少操作数 。

**示例1：**

> 输入：nums = [3,1,3,2,4,3]
> 输出：3
> 解释：
> 使数组变成交替数组的方法之一是将该数组转换为 [3,1,3,1,3,1] 。
> 在这种情况下，操作数为 3 。
> 可以证明，操作数少于 3 的情况下，无法使数组变成交替数组。
>

**示例2：**

> 输入：nums = [1,2,2,2,2]
> 输出：2
> 解释：
> 使数组变成交替数组的方法之一是将该数组转换为 [1,2,1,2,1].
> 在这种情况下，操作数为 2 。
> 注意，数组不能转换成 [2,2,2,2,2] 。因为在这种情况下，nums[0] == nums[1]，不满足交替数组的条件。
>

**提示：**

- $1 <= nums.length <= 10^5$
- $1 <= nums[i] <= 10^5$

#### 题解

题目要求将数组转化为交替数组（即奇、偶数下标所有数值相同，但两者相互不同）。要求操作次数最小值，转化为求两个不相同数值集合的个数最大值即可，具体见代码。

**C++代码：**

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& a) {
        map<int,int> mp1,mp2;   //奇 偶下标集合
        int n=a.size();
        if(n==1){
            return 0;
        }
        for(int i=0;i<n;i++){
            if(i&1) mp1[a[i]]++;
            else mp2[a[i]]++;
        }
        int num1=0,num2=0;   //偶数下标出现最多和次多的数字
        int cnt1=0,cnt2=0;   //对应的次数
        for(auto &[k,v]:mp1){
            if(v>=cnt1){
                num2=num1;cnt2=cnt1;
                num1=k,cnt1=v;
            }else if(v>=cnt2){
                cnt2=v;num2=k;
            }
        }
        int ans=0;
        for(auto &[k,v]:mp2){
            if(k==num1) ans=max(ans,cnt2+v);
            else ans=max(ans,cnt1+v);
        }
        return n-ans;
    }
};
```

### [三、拿出最少数目的魔法豆](https://leetcode-cn.com/problems/removing-minimum-number-of-magic-beans/)

#### 题意

给你一个 正 整数数组 beans ，其中每个整数表示一个袋子里装的魔法豆的数目。

请你从每个袋子中 拿出 一些豆子（也可以 不拿出），使得剩下的 非空 袋子中（即 至少 还有 一颗 魔法豆的袋子）魔法豆的数目 相等 。一旦魔法豆从袋子中取出，你不能将它放到任何其他的袋子中。

请你返回你需要拿出魔法豆的 最少数目。

**示例1：**

> 输入：beans = [4,1,6,5]
> 输出：4
> 解释：
> - 我们从有 1 个魔法豆的袋子中拿出 1 颗魔法豆。
>   剩下袋子中魔法豆的数目为：[4,0,6,5]
> - 然后我们从有 6 个魔法豆的袋子中拿出 2 个魔法豆。
>   剩下袋子中魔法豆的数目为：[4,0,4,5]
> - 然后我们从有 5 个魔法豆的袋子中拿出 1 个魔法豆。
>   剩下袋子中魔法豆的数目为：[4,0,4,4]
>   总共拿出了 1 + 2 + 1 = 4 个魔法豆，剩下非空袋子中魔法豆的数目相等。
>   没有比取出 4 个魔法豆更少的方案。
>

**示例2：**

> 输入：beans = [2,10,3,2]
> 输出：7
> 解释：
> - 我们从有 2 个魔法豆的其中一个袋子中拿出 2 个魔法豆。
>   剩下袋子中魔法豆的数目为：[0,10,3,2]
> - 然后我们从另一个有 2 个魔法豆的袋子中拿出 2 个魔法豆。
>   剩下袋子中魔法豆的数目为：[0,10,3,0]
> - 然后我们从有 3 个魔法豆的袋子中拿出 3 个魔法豆。
>   剩下袋子中魔法豆的数目为：[0,10,0,0]
>   总共拿出了 2 + 2 + 3 = 7 个魔法豆，剩下非空袋子中魔法豆的数目相等。
>   没有比取出 7 个魔法豆更少的方案。
>

**提示：**

- $1 <= beans.length <= 10^5$
- $1 <= beans[i] <= 10^5$

#### 题解

先将数组从小到大排序，然后枚举每一袋，假设当前袋beans[i]的数量为最终结果的数量，则前面所有的袋子都要为0，后面的袋子需要减少至beans[i]。

如果通过二重遍历删除显然时间不够，于是引入**前缀和**实现。具体见代码

```cpp
class Solution {
public:
    long long minimumRemoval(vector<int>& beans) {
        sort(beans.begin(),beans.end());
        int n=beans.size();
        long long sum[100010];sum[0]=beans[0];
        for(int i=1;i<n;i++) sum[i]=sum[i-1]+beans[i];
        long long ans=(sum[n-1]-beans[0])-(long long)(n-1)*beans[0];
        for(int i=1;i<n;i++){
            ans=min(ans,sum[i-1]+(sum[n-1]-sum[i])-(long long)(n-i-1)*beans[i]);
        }
        return ans;
    }
};
```


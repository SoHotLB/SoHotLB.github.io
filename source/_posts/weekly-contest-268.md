---
title: LeetCode第268场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第268场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-268/

### [一、两栋颜色不同且距离最远的房子](https://leetcode-cn.com/problems/two-furthest-houses-with-different-colors/)

#### 题意

街上有 n 栋房子整齐地排成一列，每栋房子都粉刷上了漂亮的颜色。给你一个下标从 0 开始且长度为 n 的整数数组 colors ，其中 colors[i] 表示第  i 栋房子的颜色。

返回 两栋 颜色 不同 房子之间的 最大 距离。

第 i 栋房子和第 j 栋房子之间的距离是 abs(i - j) ，其中 abs(x) 是 x 的绝对值。

**提示：**

- n == colors.length
- 2 <= n <= 100
- 0 <= colors[i] <= 100
- 生成的测试数据满足 至少 存在 2 栋颜色不同的房子

#### 示例

**示例1：**

> ![img](https://assets.leetcode.com/uploads/2021/10/31/eg1.png)
>
> 输入：colors = [1,1,1,6,1,1,1]
> 输出：3
> 解释：上图中，颜色 1 标识成蓝色，颜色 6 标识成红色。
> 两栋颜色不同且距离最远的房子是房子 0 和房子 3 。
> 房子 0 的颜色是颜色 1 ，房子 3 的颜色是颜色 6 。两栋房子之间的距离是 abs(0 - 3) = 3 。
> 注意，房子 3 和房子 6 也可以产生最佳答案。
>

**示例2：**

> ![img](https://assets.leetcode.com/uploads/2021/10/31/eg2.png)
>
> 输入：colors = [1,8,3,8,3]
> 输出：4
> 解释：上图中，颜色 1 标识成蓝色，颜色 8 标识成黄色，颜色 3 标识成绿色。
> 两栋颜色不同且距离最远的房子是房子 0 和房子 4 。
> 房子 0 的颜色是颜色 1 ，房子 4 的颜色是颜色 3 。两栋房子之间的距离是 abs(0 - 4) = 4 。
>

**示例3：**

> 输入：colors = [0,1]
> 输出：1
> 解释：两栋颜色不同且距离最远的房子是房子 0 和房子 1 。
> 房子 0 的颜色是颜色 0 ，房子 1 的颜色是颜色 1 。两栋房子之间的距离是 abs(0 - 1) = 1 。

#### 题解

数据量才100，直接两重循环随便搞搞

```cpp
class Solution {
public:
    int maxDistance(vector<int>& colors) {
        int n=colors.size();
        int ans=0;
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                if(colors[i]!=colors[j]) ans=max(ans,j-i);
            }
        }
        return ans;
    }
};
```

### [二、给植物浇水](https://leetcode-cn.com/problems/watering-plants/)

#### 题意

你打算用一个水罐给花园里的 n 株植物浇水。植物排成一行，从左到右进行标记，编号从 0 到 n - 1 。其中，第 i 株植物的位置是 x = i 。x = -1 处有一条河，你可以在那里重新灌满你的水罐。

每一株植物都需要浇特定量的水。你将会按下面描述的方式完成浇水：

按从左到右的顺序给植物浇水。
在给当前植物浇完水之后，如果你没有足够的水 完全 浇灌下一株植物，那么你就需要返回河边重新装满水罐。
你 不能 提前重新灌满水罐。
最初，你在河边（也就是，x = -1），在 x 轴上每移动 一个单位 都需要 一步 。

给你一个下标从 0 开始的整数数组 plants ，数组由 n 个整数组成。其中，plants[i] 为第 i 株植物需要的水量。另有一个整数 capacity 表示水罐的容量，返回浇灌所有植物需要的 步数 。

**提示：**

- $n == plants.length$
- $1 <= n <= 1000$
- $1 <= plants[i] <= 10^6$
- $max(plants[i]) <= capacity <= 10^9$

#### 示例

**示例1：**

> 输入：plants = [2,2,3,3], capacity = 5
> 输出：14
> 解释：从河边开始，此时水罐是装满的：
> - 走到植物 0 (1 步) ，浇水。水罐中还有 3 单位的水。
> - 走到植物 1 (1 步) ，浇水。水罐中还有 1 单位的水。
> - 由于不能完全浇灌植物 2 ，回到河边取水 (2 步)。
> - 走到植物 2 (3 步) ，浇水。水罐中还有 2 单位的水。
> - 由于不能完全浇灌植物 3 ，回到河边取水 (3 步)。
> - 走到植物 3 (4 步) ，浇水。
> 需要的步数是 = 1 + 1 + 2 + 3 + 3 + 4 = 14 。
>

**示例2：**

> 输入：plants = [1,1,1,4,2,3], capacity = 4
> 输出：30
> 解释：从河边开始，此时水罐是装满的：
> - 走到植物 0，1，2 (3 步) ，浇水。回到河边取水 (3 步)。
> - 走到植物 3 (4 步) ，浇水。回到河边取水 (4 步)。
> - 走到植物 4 (5 步) ，浇水。回到河边取水 (5 步)。
> - 走到植物 5 (6 步) ，浇水。
> 需要的步数是 = 3 + 3 + 4 + 4 + 5 + 5 + 6 = 30 。
>

**示例3：**

> 输入：plants = [7,7,7,7,7,7,7], capacity = 8
> 输出：49
> 解释：每次浇水都需要重新灌满水罐。
> 需要的步数是 = 1 + 1 + 2 + 2 + 3 + 3 + 4 + 4 + 5 + 5 + 6 + 6 + 7 = 49 。

#### 题解

也不难，直接按照题意模拟即可。

```cpp
class Solution {
public:
    int wateringPlants(vector<int>& plants, int capacity) {
        int n=plants.size();
        int ans=1,num=capacity-plants[0];
        for(int i=1;i<n;i++){
            if(num<plants[i]){
                ans+=(2*i+1);num=capacity-plants[i];
            }else{
                ans++;num-=plants[i];
            }
        }
        return ans;
    }
};
```

### [三、区间内查询数字的频率](https://leetcode-cn.com/problems/range-frequency-queries/)

#### 题意

请你设计一个数据结构，它能求出给定子数组内一个给定值的 频率 。

子数组中一个值的 频率 指的是这个子数组中这个值的出现次数。

请你实现 RangeFreqQuery 类：

RangeFreqQuery(int[] arr) 用下标从 0 开始的整数数组 arr 构造一个类的实例。
int query(int left, int right, int value) 返回子数组 arr[left...right] 中 value 的 频率 。
一个 子数组 指的是数组中一段连续的元素。arr[left...right] 指的是 nums 中包含下标 left 和 right 在内 的中间一段连续元素。

**提示：**

- $1 <= arr.length <= 10^5$
- $1 <= arr[i],value <= 10^4$
- $0<= left <= right < arr.length$
- 调用 query 不超过 $10^5$ 次。

#### 示例

> 输入：
> ["RangeFreqQuery", "query", "query"]
> [[[12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]], [1, 2, 4], [0, 11, 33]]
> 输出：
> [null, 1, 2]
>
> 解释：
> RangeFreqQuery rangeFreqQuery = new RangeFreqQuery([12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]);
> rangeFreqQuery.query(1, 2, 4); // 返回 1 。4 在子数组 [33, 4] 中出现 1 次。
> rangeFreqQuery.query(0, 11, 33); // 返回 2 。33 在整个子数组中出现 2 次。
>

#### 题解

使用map存储每个数出现的所有位置，key表示数字，value则表示所有的位置。遍历数组存储每个数字出现的位置，显然位置数组是单调递增的，于是可以直接使用二分查找当前数字从0到r有多少个 减去 从0到l-1有多少个即可。C++有自带二分函数，Java相对来讲就麻烦些。

**C++代码：**

```cpp
class RangeFreqQuery {
public:
    map<int,vector<int> >mp;
    RangeFreqQuery(vector<int>& arr) {
        for(int i=0;i<arr.size();i++){
            mp[arr[i]].push_back(i);
        }
    }
    
    int query(int left, int right, int value) {
        if(!mp.count(value)) return 0;
        auto &v=mp[value];
        auto r=upper_bound(v.begin(),v.end(),right);
        auto l=upper_bound(v.begin(),v.end(),left-1);
        return r-l;
    }
};
```


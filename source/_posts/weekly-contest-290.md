---
title: LeetCode第290场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第290场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-290/

### [一、多个数组求交集](https://leetcode-cn.com/problems/intersection-of-multiple-arrays/)

#### 题解：哈希表

由于题目要求每个元素在nums所有数组中都出现过，并返回它们。于是不妨将所有元素存入与一个哈希表中：key表示nums所有数组中的各个元素，value对应它们在几个数组中出现过

由于题目说明了nums中所有数组都是由不同正整数组成，所以对于value的处理更加简单了，只需要判断出现过多少次即可。

如果value等于nums数组的个数则满足题意。由于c++ map自动对key按照从小到大排序，进一步满足题目要求，只需遍历一遍map即可。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> intersection(vector<vector<int>>& nums) {
        map<int,int>mp;
        int n=nums.size();
        for(int i=0;i<n;i++){
            for(int j=0;j<nums[i].size();j++){
                mp[nums[i][j]]++;
            }
        }
        vector<int> ans;
        for(auto it:mp){
            if(it.second==n){
                ans.push_back(it.first);
            }
        }
        return ans;
    }
};
```

### [二、统计圆内格点数目](https://leetcode-cn.com/problems/count-lattice-points-inside-a-circle/)

#### 题意：简单数学

已知圆心坐标以及半径，只需判断当前点到圆心的距离与半径的大小关系即可：

- 到圆心的距离小于等于半径，则在圆内部
- 否则在圆外部

对于点的遍历，可以直接已圆心为中心点，计算它对应的正方形的所有坐标即可。

可以用map或set集合存储所有满足条件点的个数。

**C++代码：**

```cpp
class Solution {
public:
    int countLatticePoints(vector<vector<int>>& circles) {
        map<pair<int,int>,int> mp;
        int n=circles.size();
        int x,y,r;
        pair<int,int> now;
        for(int i=0;i<n;i++){
            x=circles[i][0];y=circles[i][1];r=circles[i][2];
            for(int xx=x-r;xx<=x+r;xx++){
                for(int yy=y-r;yy<=y+r;yy++){
                    if((xx-x)*(xx-x)+(yy-y)*(yy-y)>r*r) continue;
                    now.first=xx;now.second=yy;
                    mp[now]++;
                }
            }
        }
        return mp.size();
    }
};
```

### [三、统计包含每个点的矩形数目](https://leetcode-cn.com/problems/count-number-of-rectangles-containing-each-point/)

#### 题解：排序+二分

给定若干个矩形，矩形的坐标为：左下角均在二维坐标系原点位置，给定右上角的坐标。然后给出若干点的坐标，判断每个点被多少个矩形所包含，返回一个整数数组即可。

由于题目给定y坐标最大为100，于是考虑对y坐标进行存储，第二维则存储纵坐标相同情况下，所有的点横坐标的信息。

然后对横坐标进行排序。遍历所有的点，假设当前遍历的点的纵坐标为y，由于y最大为100，直接暴力枚举$j(y<=j<=100)$，再求出所有矩形右上角纵坐标坐标为j时，横坐标大于等于当前遍历的点的横坐标x的矩形个数（二分查找，直接暴力查找会超时）。具体见代码：

**C++代码：**

```cpp
class Solution {
public:
    vector<int> countRectangles(vector<vector<int>>& rectangles, vector<vector<int>>& points) {
        int n=points.size();
        //存储所有的坐标，纵坐标为第一维，横坐标为第二维，即存储纵坐标对应的对应的所有横坐标集合
        //y最大为100
        vector<vector<int> > a(110);
        for(auto arr:rectangles){
            a[arr[1]].push_back(arr[0]);
        }
        //对相同纵坐标的横坐标按照从小到大排序
        for(int i=1;i<=100;i++) sort(a[i].begin(),a[i].end());
        vector<int> ans(n);
        int x,y;
        for(int i=0;i<n;i++){
            x=points[i][0];y=points[i][1];
            //首先满足矩形的高度大于等于纵坐标
            for(int j=y;j<=100;j++){
                if(a[j].size()>0){
                    //二分查找第一个大于等于当前横坐标的索引
                    ans[i]+=a[j].size()-(lower_bound(a[j].begin(),a[j].end(),x)-a[j].begin());
                }
            }
        }
        return ans;
    }
};
```

### [四、花期内花的数目](https://leetcode-cn.com/problems/number-of-flowers-in-full-bloom/)

#### 题解：转换+排序

需要求每个人到达时能看到花的个数，直接转换为：花期开始时间小于等于当前时间花的个数  -  花期结束时间大于等于当前时间花的个数。

为了方便查找，直接存储所有花开始的时间和结束的时间，然后排序即可。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& persons) {
        int n=persons.size();
        int m=flowers.size();
        vector<int> ans(n);

        //记录每朵花开始的时间和结束的时间
        vector<int> stime;vector<int> etime;
        for(auto x:flowers){
            stime.push_back(x[0]);
            etime.push_back(x[1]);
        }
        sort(stime.begin(),stime.end());
        sort(etime.begin(),etime.end());
        for(int i=0;i<n;i++){
            //当前时间>=花期开始的时间的花的个数  - 当前时间<=花期结束的时间花的个数
            ans[i]=(upper_bound(stime.begin(),stime.end(),persons[i])-stime.begin())-(lower_bound(etime.begin(),etime.end(),persons[i])-etime.begin());
        }
        return ans;
    }
};
```


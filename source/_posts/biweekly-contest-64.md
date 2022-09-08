---
title: LeetCode第64场双周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
# mathjax: true
katex: true
---

# LeetCode第46场双周赛

## 一、数组中第K个独一无二的字符串

### 题意：

给定一个数组和一个整数`k`，需要求数组中第`k`个独一无二字符串。如果少于`k`个独一无二字符串则返回空字符串。

其中：

- $1 <= k <= arr.length <= 1000$
- $1 <= arr[i].length <= 5$
- $arr[i] 只包含小写英文字母$

### 示例：

**示例1：**

> 输入：arr = ["d","b","c","b","c","a"], k = 2
> 输出："a"
> 解释：
> arr 中独一无二字符串包括 "d" 和 "a" 。
> "d" 首先出现，所以它是第 1 个独一无二字符串。
> "a" 第二个出现，所以它是 2 个独一无二字符串。
> 由于 k == 2 ，返回 "a" 。

**示例2：**

> 输入：arr = ["a","b","a"], k = 3
> 输出：""
> 解释：
> 唯一一个独一无二字符串是 "b" 。由于少于 3 个独一无二字符串，我们返回空字符串 "" 。

### 题解：

遍历一遍数组，用哈希表map记录每个字符串出现的次数。然后第二遍遍历数组，用set去重，记录是否存在第k个独一无二的字符串，如果存在则保存并返回，否则返回空字符串

时间复杂度：**O(nlogn)**

```cpp
class Solution {
public:
    string kthDistinct(vector<string>& arr, int k) {
        map<string,int>mp;
        int n=arr.size();
        for(int i=0;i<n;i++){
            mp[arr[i]]++;
        }
        set<string>st;int cnt=0;
        for(int i=0;i<n;i++){
            if(mp[arr[i]]==1 && st.count(arr[i])==0){
                st.insert(arr[i]);cnt++;
                if(cnt==k){
                    return arr[i];
                }
            }
        }
        return "";
    }
};
```

## 二、两个最好的不重叠活动

### 题意：

给定一系列活动，每个活动有开始时间、结束时间和相应的价值。现你最多可以参加两个**时间不重叠**的时间，使得你获得的价值最大

其中：

- $2 <= events.length <= 10^5$
- $events[i].length == 3$
- $1 <= startTime_i <= endTime_i <= 10^9$
- $1 <= value_i <= 10^6$

### 题解：

感觉思路有点难想，或者是我太菜了....

1. 先按照开始时间对活动进行排序
2. 使用优先队列（小根堆），存储活动的结束时间和价值，按照**结束时间从小到大排序**
3. 遍历整个活动数组，当优先队列队首（即当前最小的结束时间）小于当前活动的开始时间，则更新最大值maxh，并弹出队首元素
4. 每次更新能够获得的最大价值

时间复杂度：**O(nlogn)**

```cpp
class Solution {
public:
    int maxTwoEvents(vector<vector<int>>& ev) {
        map<int,int> mp;
        int n=ev.size();
        sort(ev.begin(),ev.end());
        priority_queue<pair<int,int> ,vector<pair<int,int> >,greater<> > q;
        int res=0,maxh=0;
        for(int i=0;i<n;i++){
            int s=ev[i][0];int e=ev[i][1];int t=ev[i][2];
            while(!q.empty()&&q.top().first<s){
                maxh=max(maxh,q.top().second);
                q.pop();
            }
            res=max(res,maxh+t);
            q.emplace(e,t);
        }
        return res;
    }
};
```

## 三、蜡烛之间的盘子

### 题意：

给定一个字符串数组，含有`*`和`|`两个字符，分别表示盘子和蜡烛。现给出一个询问数组，每个元素给定一个区间`[l,r]`，需要求得该区间中两支蜡烛之间的最大盘子数量

其中：

- $3 <= s.length <= 10^5$
- $s 只包含字符 '*' 和 '|'$ 
- $1 <= queries.length <= 10^5$
- $queries[i].length == 2$
- $0 <= left_i <= right_i < s.length$

### 示例：

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/ex-1.png)

> 输入：s = `"**|**|***|"`, queries = [[2,5],[5,9]]
> 输出：[2,3]
> 解释：
>
> - queries[0] 有两个盘子在蜡烛之间。
>
> - queries[1] 有三个盘子在蜡烛之间。

**示例2：**

![ex-2](https://gitee.com/serendipity_LB/img/raw/master/ex-2.png)

> 输入：s = `"***|**|*****|**||**|*"` ，queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
> 输出：[9,0,0,0,0]
> 解释：
>
> - queries[0] 有 9 个盘子在蜡烛之间。
> - 另一个查询没有盘子在蜡烛之间。
>

### 题解：

1. 用一数组存储所有蜡烛出现的位置
2. 每次询问时，利用二分查找求得区间中第一次蜡烛出现的位置和最后一次蜡烛出现的位置
3. **$两个位置的距离差-区间中蜡烛的数量$**即为结果

时间复杂度：**O(nlogn)**

```cpp
class Solution {
public:
    vector<int> platesBetweenCandles(string s, vector<vector<int>>& q) {
        vector<int> p;vector<int> ans;
        for(int i=0;i<s.length();i++){
            if(s[i]=='|'){
                p.push_back(i);
            }
        }
        for(int i=0;i<q.size();i++){
            int l=lower_bound(p.begin(),p.end(),q[i][0])-p.begin();
            int r=upper_bound(p.begin(),p.end(),q[i][1])-p.begin();r--;
            if(r<=l){
                ans.push_back(0);continue;
            }
            ans.push_back(p[r]-p[l]-(r-l+1)+1);
        }
        return ans;
    }
};
```






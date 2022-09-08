---
title: LeetCode第291场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第291场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-291/

### [一、移除指定数字得到的最大结果](https://leetcode-cn.com/problems/remove-digit-from-number-to-maximize-result/)

#### 题解：暴力

比赛时想复杂了，代码又丑又长，还wa了两发...，这里便不给出比赛的思路了。

需要计算删除指定字符后，字符串表示的数字结果最大化。

直接遍历字符串，若当前字符等于需要删除的字符时，则记录若删除当前字符后，所能得到的数字，只要维护这个数字的最大值即可，具体见代码。

**C++代码：**

```cpp
class Solution {
public:
    string removeDigit(string number, char digit) {
        int n=number.size();
        string ans="",res;
        for(int i=0;i<n;i++){
            if(number[i]==digit){
                res=number.substr(0,i)+number.substr(i+1,n-i);
                if(res>ans){
                    ans=res;
                }
            }
        }
        return ans;
    }
};
```

### [二、必须拿起的最小连续卡牌数](https://leetcode-cn.com/problems/minimum-consecutive-cards-to-pick-up/)

#### 题解：哈希

计算必须最小拿起的连续卡牌数，使得拿起卡牌数中有一对相同数字的卡牌。

只需要记录每个数字第一次出现的下标，若当前遍历的数字为x：

- 如果前面已经出现过，则直接记录当前需要拿起的连续卡牌数量，并维护最小值。

- 如果前面没有出现过，则将当前数字以及下标存入到哈希表中。

具体见代码。

**C++代码：**

```cpp
class Solution {
public:
    int minimumCardPickup(vector<int>& cards) {
        int n=cards.size();
        map<int,int> mp;
        int ans=0x3f3f3f3f;
        for(int i=0;i<n;i++){
            if(mp.count(cards[i])){
                ans=min(ans,i-mp[cards[i]]+1);
            }
            mp[cards[i]]=i;
        }
        if(ans==0x3f3f3f3f) ans=-1;
        return ans;
    }
};
```

### [三、含最多 K 个可整除元素的子数组](https://leetcode-cn.com/problems/k-divisible-elements-subarrays/)

#### 题解：哈希

由于数组长度最大为200，所有子数组个数最大为：$$\Sigma_1^n  i$$（其中n为数组长度）

需要判断没有重复的子数组，可以将数字转化为字符，于是便将子数组转化为了字符串，存入到哈希表中判重。

假设当前遍历的下标为i，则直接判断以下边i开头的子数组有多少个满足题意：

- 如果超过k个可整除的元素，则直接结束循环
- 如果不超过k个，则判断当前子数组是否出现过，若没出现过则结果加1，同时存到到哈希表中

具体见代码。

**C++代码：**

```cpp
class Solution {
public:
    set<string> st;
    int countDistinct(vector<int>& nums, int k, int p) {
        int n=nums.size();
        int ans=0;
        for(int i=0;i<n;i++){
            string s="";int cnt=0;
            for(int j=i;j<n;j++){
                s+=nums[j]+'0';
                if(nums[j]%p==0) ++cnt;
                if(cnt>k) break;
                if(st.count(s)) continue;
                st.insert(s);++ans;
            }
        }
        return ans;
    }
};
```

### [四、字符串的总引力](https://leetcode-cn.com/problems/total-appeal-of-a-string/)

#### 题解：dp

需要计算所有子字符串的引力和。字符串的引力表示为：字符串中出现不同字符的个数。

由于字符串长度最大为$1e^5$，显然按照题三一样两重循环遍历所有子字符串显然不行，考虑dp

dp[i]表示以s[i]结尾的子字符串的所有引力之和。

于是最终结果ans为所有dp[i]的和。

状态转移方程为：

- 如果前面出现过s[i]，则：$dp[i]=dp[i-1]+(i-j-1)+1$，其中j表示s[i]在前面出现的下标，
- 如果没有出现过s[i]，则：$dp[i]=dp[i-1]+i+1$

具体见代码。

**C++代码：**

```cpp
class Solution {
public:
    typedef long long ll;
    ll dp[100010];
    long long appealSum(string s) {
        ll ans=1;
        int n=s.size();
        //记录每个字母出现的索引
        int pos[26];
        for(int i=0;i<26;i++) pos[i]=-1;
        pos[s[0]-'a']=0;dp[0]=1;
        for(int i=1;i<n;i++){
            if(pos[s[i]-'a']==-1){
                //没有出现过
                dp[i]=dp[i-1]+i+1;
            }else{
                //前面出现过
                dp[i]=dp[i-1]+(i-pos[s[i]-'a']);
            }
            //更新s[i]最新的下标
            pos[s[i]-'a']=i;
            ans+=dp[i];
        }
        return ans;
    }
};
```

当然，可以对空间进行优化，对于dp数组，只需要用一个变量sum存储即可。

```cpp
class Solution {
public:
    typedef long long ll;
    vector<int> ve;
    long long appealSum(string s) {
        int n=s.length();
        int pos[26];
        for(int i=0;i<26;i++) pos[i]=-1;
        ll ans=0,sum=0;
        for(int i=0;i<n;i++){
            if(pos[s[i]-'a']!=-1){
                //前面已经出现过s[i]
                int pre_pos=pos[s[i]-'a'];
                sum+=(i-pre_pos);
            }else{
                //前面没有出现过s[i]
                sum+=(i+1);
            }
            //更新s[i]最新的下标
            pos[s[i]-'a']=i;
            //ans累加
            ans+=sum;
        }
        return ans;
    }
};
```


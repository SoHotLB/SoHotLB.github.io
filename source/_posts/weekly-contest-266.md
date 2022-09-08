---
title: LeetCode第266场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## 一、统计字符串中元音子字符串

### 题意：

给定一个只含小写字母的字符串，需要求只包含元音字符的子字符串个数

其中：

- `1 <= word.length <= 100`
- `word` 仅由小写英文字母组成

### 示例：

**示例1：**

>输入：word = "aeiouu"
>输出：2
>解释：下面列出 word 中的元音子字符串（斜体加粗部分）：
>- "aeiouu"
>- "aeiouu"

**示例2：**

> 输入：word = "cuaieuouac"
> 输出：7
> 解释：下面列出 word 中的元音子字符串（斜体加粗部分）：
> - "cuaieuouac"
> - "cuaieuouac"
> - "cuaieuouac"
> - "cuaieuouac"
> - "cuaieuouac"
> - "cuaieuouac"
> - "cuaieuouac"
>

**示例3：**

>输入：word = "bbaeixoubb"
>输出：0
>解释：所有包含全部五种元音的子字符串都含有辅音，所以不存在元音子字符串。

### 题解：

由于字符串长度<=100，直接暴力即可，随便怎么暴力都行，这里由于时间因素，当时直接选择三重for循环

```cpp
class Solution {
public:
    int countVowelSubstrings(string s) {
        int len=s.length();
        set<char>st;
        int ans=0;
        for(int i=0;i<len;i++){
            for(int j=i;j<len;j++){
                st.clear();
                for(int k=i;k<=j;k++){
                    st.insert(s[k]);
                }
                if(st.size()==5&&st.count('a')&&st.count('e')&&st.count('i')&&st.count('o')&&st.count('u')){
                    ans++;
                }
            }
        }
        return ans;
    }
};
```

## 二、所有子字符串中的元音

### 题意：

给定一个字符串，返回所有字符串中元音的总数

其中：

- `1 <= word.length <= 105`
- `word` 由小写英文字母组成

### 示例：

**示例1：**

>输入：word = "aba"
>输出：6
>解释：
>所有子字符串是："a"、"ab"、"aba"、"b"、"ba" 和 "a" 。
>- "b" 中有 0 个元音
>- "a"、"ab"、"ba" 和 "a" 每个都有 1 个元音
>- "aba" 中有 2 个元音
>因此，元音总数 = 0 + 1 + 1 + 1 + 1 + 2 = 6 。
>

**示例2：**

> 输入：word = "abc"
> 输出：3
> 解释：
> 所有子字符串是："a"、"ab"、"abc"、"b"、"bc" 和 "c" 。
> - "a"、"ab" 和 "abc" 每个都有 1 个元音
> - "b"、"bc" 和 "c" 每个都有 0 个元音
> 因此，元音总数 = 1 + 1 + 1 + 0 + 0 + 0 = 3 。
>

**示例3：**

> 输入：word = "noosabasboosa"
> 输出：237
> 解释：所有子字符串中共有 237 个元音。

### 题解：

遍历字符串，若当前遍历元素$word[i]$为元音时，则若存在子字符串$word[l...r]$要包含字符$word[i]$，则

- $0 \le l \le i$
- $i \le r<n$（n为字符串word的长度）

于是代码如下

```cpp
class Solution {
public:
    long long countVowels(string word) {
        int n=word.length();
        long long ans=0;
        for(int i=0;i<n;i++){
            if(word[i]=='a'||word[i]=='e'||word[i]=='i'||word[i]=='o'||word[i]=='u'){
                ans+=(long long)(i+1)*(n-i);
            }
        }
        return ans;
    }
};
```

## 三、分配给商店的最多商品的最小值

### 题意：

给你一个整数 n ，表示有 n 间零售商店。总共有 m 种产品，每种产品的数目用一个下标从 0 开始的整数数组 quantities 表示，其中 quantities[i] 表示第 i 种商品的数目。

你需要将 所有商品 分配到零售商店，并遵守这些规则：

一间商店 至多 只能有 一种商品 ，但一间商店拥有的商品数目可以为 任意 件。
分配后，每间商店都会被分配一定数目的商品（可能为 0 件）。用 x 表示所有商店中分配商品数目的最大值，你希望 x 越小越好。也就是说，你想 最小化 分配给任意商店商品数目的 最大值 。
请你返回最小的可能的 x 。

### 示例：

**示例1：**

> 输入：n = 6, quantities = [11,6]
> 输出：3
> 解释： 一种最优方案为：
> - 11 件种类为 0 的商品被分配到前 4 间商店，分配数目分别为：2，3，3，3 。
> - 6 件种类为 1 的商品被分配到另外 2 间商店，分配数目分别为：3，3 。
> 分配给所有商店的最大商品数目为 max(2, 3, 3, 3, 3, 3) = 3 。
>

**示例2：**

> 输入：n = 7, quantities = [15,10,10]
> 输出：5
> 解释：一种最优方案为：
> - 15 件种类为 0 的商品被分配到前 3 间商店，分配数目为：5，5，5 。
> - 10 件种类为 1 的商品被分配到接下来 2 间商店，数目为：5，5 。
> - 10 件种类为 2 的商品被分配到最后 2 间商店，数目为：5，5 。
> 分配给所有商店的最大商品数目为 max(5, 5, 5, 5, 5, 5, 5) = 5 。
>

**示例3：**

> 输入：n = 1, quantities = [100000]
> 输出：100000
> 解释：唯一一种最优方案为：
> - 所有 100000 件商品 0 都分配到唯一的商店中。
> 分配给所有商店的最大商品数目为 max(100000) = 100000 。
>

### 题解：

直接二分答案即可。

```cpp
class Solution {
public:
    int minimizedMaximum(int n, vector<int>& quantities) {
        int m=quantities.size();
        int l=1,r=100000;
        while(l<=r){
            int mid=(l+r)>>1;
            int cnt=0;
            for(int i=0;i<m;i++){
                cnt+=quantities[i]/mid;
                if(quantities[i]%mid) cnt+=1;
            }
            if(cnt<=n){
                r=mid-1;
            }else{
                l=mid+1;
            }
        }
        return l;
    }
};
```




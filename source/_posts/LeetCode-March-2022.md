---
title: LeetCode2022年每日一题3月打卡汇总
categories: 'LeetCode每日一题'
tags: 
  - LeetCode
  - Algorithm
katex: true
---
## LeetCode2022年每日一题3月打卡汇总

### [3.1：Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

#### 题意

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

P   A   H   N
A P L S I I G
Y   I   R
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);

**示例1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

**示例2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

**示例3：**

```
输入：s = "A", numRows = 1
输出："A"
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 由英文字母（小写和大写）、`','` 和 `'.'` 组成
- `1 <= numRows <= 1000`

#### 题解一：二维数组模拟

直接按照题意模拟，将之字形排列输出到二维矩阵中，然后从左到右、从上到下遍历即可。

需要注意，只有一行的情况需要特判，直接输出s。

**C++代码：**

```cpp
class Solution {
public:
    char a[1010][1010];
    string convert(string s, int numRows) {
        if(numRows==1) return s;
        int x=0,y=1,cnt=0,t=0;
        for(int i=1;i<=1000;i++) for(int j=1;j<=1000;j++) a[i][j]=' ';
        int n=s.length();
        //先输出第一行
        while(cnt<numRows&&cnt<n) a[++x][y]=s[cnt++];
        while(cnt<n){
            //斜着输出
            t=0;
            while(cnt<n&&t<numRows-1){
                a[--x][++y]=s[cnt++];
                t++;
            }
            //输出一列
            t=0;
            while(cnt<n&&t<numRows-1) a[++x][y]=s[cnt++],t++;
        }
        string ans=s;cnt=0;
        for(int i=1;i<=numRows;i++){
            for(int j=1;j<=y;j++){
                if(a[i][j]==' ') continue;
                ans[cnt++]=a[i][j];
            }
        }
        return ans;
    }
};
```

#### 题解二：巧妙模拟

https://leetcode-cn.com/problems/zigzag-conversion/solution/zzi-xing-bian-huan-by-jyd/

**C++代码：**

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows==1) return s;
        vector<string> ve(numRows);
        int n=s.length();
        int x=0,flag=-1;
        for(int i=0;i<n;i++){
            ve[x]+=s[i];
            if(x==0||x==numRows-1) flag=-flag;
            x+=flag;
        }
        string ans;
        for(auto &str:ve) ans+=str;
        return ans;
    }
};
```

### [3.2：寻找最近的回文数](https://leetcode-cn.com/problems/find-the-closest-palindrome/)

#### 题意

给定一个表示整数的字符串 `n` ，返回与它最近的回文整数（不包括自身）。如果不止一个，返回较小的那个。

“最近的”定义为两个整数**差的绝对值**最小。

**示例1：**

```
输入: n = "123"
输出: "121"
```

**示例2：**

```
输入: n = "1"
输出: "0"
解释: 0 和 2是最近的回文，但我们返回最小的，也就是 0。
```

**提示：**

- `1 <= n.length <= 18`
- `n` 只由数字组成
- `n` 不含前导 0
- `n` 代表在 $[1, 10^{18} - 1]$ 范围内的整数

#### 题解：模拟

由于需要求回文串，如果我们从低位考虑，则对应的高位也需要改变，于是我们考虑从中间位置变化，然后低位便随着高位改变。

但是需要注意进位和减位的情况，例如：100-1,99+1。

**C++代码：**

```cpp
class Solution {
public:
    typedef long long ll;
    ll tonum(string s){
        ll res=0;
        for(int i=0;i<s.length();i++){
            res=res*10+(s[i]-'0');
        }
        return res;
    }
    ll getnum(ll x,bool tag){
        int n=0;
        ll res=x;
        string s=to_string(res);
        res=x;n=s.length();
        int pos=tag?n-1:n-2;
        while(pos>=0) res=res*10+(s[pos--]-'0');
        return res;
    }
    string nearestPalindromic(string s) {
        int n=s.length();
        ll now=tonum(s);
        //先考虑10^n+1,10^(n-1)-1
        set<ll> st;
        st.insert(pow(10,n)+1);st.insert(pow(10,n-1)-1);
        ll t=0;
        for(int i=0;i<(n+1)/2;i++) t=t*10+(s[i]-'0');
        for(ll i=t-1;i<=t+1;i++){
            ll tmp=-1;
            if(n%2==0) tmp=getnum(i,1);
            else tmp=getnum(i,0);
            if(tmp!=now) st.insert(tmp);
        }
        ll ans=-1;
        for(auto &i:st){
            // cout<<i<<endl;
            if(ans==-1) ans=i;
            else if(abs(i-now)<abs(ans-now)) ans=i;
            else if(abs(i-now)==abs(ans-now)&&i<ans) ans=i;
        }
        return to_string(ans);
    }
};
```

### [3.3：各位相加](https://leetcode-cn.com/problems/add-digits/)

#### 题意

给定一个非负整数 `num`，反复将各个位上的数字相加，直到结果为一位数。返回这个结果。

**示例1：**

```
输入: num = 38
输出: 2 
解释: 各位相加的过程为：
38 --> 3 + 8 --> 11
11 --> 1 + 1 --> 2
由于 2 是一位数，所以返回 2。
```

**示例2：**

```
输入: num = 0
输出: 0
```

**提示：**

- $0 <= num <= 2^{31} - 1$

#### 题解一：模拟

直接按照题意模拟即可。

**C++代码：**

```cpp
class Solution {
public:
    int addDigits(int num) {
        int x;
        while(num>=10){
            x=0;
            while(num){
                x+=num%10;num/=10;
            }
            num=x;
        }
        return num;
    }
};
```

**Java代码：**

```java
class Solution {
    public int addDigits(int num) {
        int sum=0;
        while(num>=10){
            sum=0;
            while(num>0){
                sum+=num%10;num/=10;
            }
            num=sum;
        }
        return num;
    }
}
```

#### 题解二：思维/数学

[巧妙题解](https://leetcode-cn.com/problems/add-digits/solution/java-o1jie-fa-de-ge-ren-li-jie-by-liveforexperienc/)

**C++代码：**

```cpp
class Solution {
public:
    int addDigits(int num) {
        return (num-1)%9+1;
    }
};
```

### [3.4：子数组范围和](https://leetcode-cn.com/problems/sum-of-subarray-ranges/)

#### 题意

给你一个整数数组 nums 。nums 中，子数组的 范围 是子数组中最大元素和最小元素的差值。

返回 nums 中 所有 子数组范围的 和 。

子数组是数组中一个连续 非空 的元素序列。

**示例1：**

```
输入：nums = [1,2,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0 
[2]，范围 = 2 - 2 = 0
[3]，范围 = 3 - 3 = 0
[1,2]，范围 = 2 - 1 = 1
[2,3]，范围 = 3 - 2 = 1
[1,2,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 1 + 1 + 2 = 4
```

**示例2：**

```
输入：nums = [1,3,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0
[3]，范围 = 3 - 3 = 0
[3]，范围 = 3 - 3 = 0
[1,3]，范围 = 3 - 1 = 2
[3,3]，范围 = 3 - 3 = 0
[1,3,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 2 + 0 + 2 = 4
```

**示例3：**

```
输入：nums = [4,-2,-3,4,1]
输出：59
解释：nums 中所有子数组范围的和是 59
```

**提示：**

- $1 <= nums.length <= 1000$
- $-10^9 <= nums[i] <= 10^9$

#### 题解一：模拟

遍历所有子数组，保存对应的最小值和最大值，依次求子数组最大值和最小值之差的和即可。

**C++代码：**

```cpp
class Solution {
public:
    typedef long long ll;
    int inf=0x3f3f3f3f;
    long long subArrayRanges(vector<int>& nums) {
        int n=nums.size();
        int minnum=inf,maxnum=-inf;
        ll ans=0;
        for(int i=0;i<n;i++){
            minnum=inf,maxnum=-inf;
            for(int j=i;j<n;j++){
                minnum=min(minnum,nums[j]);maxnum=max(maxnum,nums[j]);
                // cout<<i<<"--"<<j<<"--"<<minnum<<"---"<<maxnum<<endl;
                ans+=maxnum-minnum;
            }
        }
        return ans;
    }
};
```

#### 题解二：单调栈

我们定义：i<j，如果nums[i]==nums[j]，则逻辑上我们认为nums[i]<nums[j]（因为i<j）。进行此定义为了方便处理单调栈。

题目所求为：所有子数组范围（子数组最大值-最小值）和。可以转化为`<font color="#ff0000">`**所有子数组的最大值的和-所有子数组最小值和**`</font>`。

假设nums[j]左边第一个比它小的元素为nums[i]，nums[j]右边第一个比它小的元素为nums[k]。则所有子数组中以nums[i]为最小值的个数为$(j-i) * (k-j)$。如何获得nums[j]左边和右边第一个比它小的元素的下标呢， `<font color="#ff0000">`单调栈`</font>`刚好可以处理，预处理数组minL，minR。其中minL[i]表示nums[i]左边第一个比它小的元素的下标，minR[i]表示nums[i]右边第一个比它小的元素的下标。

> 单调栈的一个例子：
>
> ```
> // 对于数组 [..., 3, 5,6,7,4,1,2]
> // 
> // 要计算数字5的「右侧比5小的第一个数」的时候
> // 需要关注的只有 [6,4,1] 这三个数，也就是单调栈。
> // 由于6比5大，所以将6出栈，变成 [4,1]，于是找到了，4就是「比5小的右侧第一个数」
> // 然后将5入栈，变成 [5,4,1]
> //
> // 然后继续计算5左边的3的「右侧第一个更小的数」，此时需要考虑的栈是[5,4,1]
> // 依次将5, 4出栈，栈变成了[1]，终于比3小了，1就是比3小的右侧第一个数。然后将3入栈，变成[3, 1]，再继续往左。
> // 即，计算「右侧比nums[i]小的第一个数」的时候，要从右往左算。
> ```
>
> 于是我们以minL处理为例，从左到右遍历数组，当遍历到nums[i]时：
>
> - 执行出栈，直到栈为空 或者 nums[minstack.top()]逻辑上小于nums[i]，
> - 如果栈为空，则minL[i]=-1，否则minL[i]=minstack.top()
> - 然后将下标i入栈

于是所有子数组的最小值和为$minsum=\Sigma_{i=0}^{n-1}(minR[i]-i)(i-minL[i])*nums[i]$，同理求得maxsum。具体见代码：

**C++代码：**

```cpp
class Solution {
public:
    typedef long long ll;
    long long subArrayRanges(vector<int>& nums) {
        int n=nums.size();
        stack<int> minstack,maxstack;
        //minL[i]：表示nums[i]左边比它小的第一个元素的下标。其他同理
        vector<int> minL(n),minR(n),maxL(n),maxR(n);

        //处理minL[],maxL[]
        for(int i=0;i<n;i++){
            //如果当前栈顶元素大于nums[i]，则出栈
            while(!minstack.empty()&&nums[minstack.top()]>nums[i]) minstack.pop();
            minL[i]=minstack.empty()?-1:minstack.top();
            minstack.push(i);

            //如果当前栈顶元素小于等于nums[i]，则出栈
            //上述已定义了排序规则，若nums[maxstack.top()]==nums[i],则nums[maxstack.top()]<nums[i]（因为maxstack.top()<i）
            while(!maxstack.empty()&&nums[maxstack.top()]<=nums[i]) maxstack.pop();
            maxL[i]=maxstack.empty()?-1:maxstack.top();
            maxstack.push(i);
        }

        while(!minstack.empty()) minstack.pop();
        while(!maxstack.empty()) maxstack.pop();
        //处理minR[],maxR[]
        for(int i=n-1;i>=0;i--){
            //如果当前栈顶元素大于等于nums[i]，则出栈
            //若nums[maxstack.top()]==nums[i],则nums[maxstack.top()]>nums[i]（因为maxstack.top()>i）
            while(!minstack.empty()&&nums[minstack.top()]>=nums[i]) minstack.pop();
            minR[i]=minstack.empty()?n:minstack.top();
            minstack.push(i);

            //如果当前栈顶元素小于nums[i]，则出栈
            while(!maxstack.empty()&&nums[maxstack.top()]<nums[i]) maxstack.pop();
            maxR[i]=maxstack.empty()?n:maxstack.top();
            maxstack.push(i);
        }

        // for(int i=0;i<n;i++){
        //     cout<<minL[i]<<"---"<<minR[i]<<"---"<<maxL[i]<<"---"<<maxR[i]<<endl;
        // }

        ll maxsum=0,minsum=0;
        for(int i=0;i<n;i++){
            //分别以nums[i]为子数组的最大值或最小值的和，具体见题解描述
            maxsum+=(ll)(maxR[i]-i)*(i-maxL[i])*nums[i];
            minsum+=(ll)(minR[i]-i)*(i-minL[i])*nums[i];
        }
        return maxsum-minsum;
    }
};
```

**Java代码：**

```java
class Solution {
	public long subArrayRanges(int[] nums) {
		int n = nums.length;
		int minL[] = new int[n];
		int minR[] = new int[n];
		int maxL[] = new int[n];
		int maxR[] = new int[n];

		// Java一般中Deque代替Stack和Queue
		Deque<Integer> minstack = new ArrayDeque<>();
		Deque<Integer> maxstack = new ArrayDeque<>();
		for (int i = 0; i < n; i++) {
			while (!minstack.isEmpty() && nums[minstack.peek()] > nums[i])
				minstack.pop();
			minL[i] = minstack.isEmpty() ? -1 : minstack.peek();
			minstack.push(i);

			while (!maxstack.isEmpty() && nums[maxstack.peek()] <= nums[i])
				maxstack.pop();
			maxL[i] = maxstack.isEmpty() ? -1 : maxstack.peek();
			maxstack.push(i);
		}

		minstack.clear();
		maxstack.clear();
		for (int i = n - 1; i >= 0; i--) {
			while (!minstack.isEmpty() && nums[minstack.peek()] >= nums[i])
				minstack.pop();
			minR[i] = minstack.isEmpty() ? n : minstack.peek();
			minstack.push(i);

			while (!maxstack.isEmpty() && nums[maxstack.peek()] < nums[i])
				maxstack.pop();
			maxR[i] = maxstack.isEmpty() ? n : maxstack.peek();
			maxstack.push(i);
		}

		long minsum = 0, maxsum = 0;
		for (int i = 0; i < n; i++) {
			minsum += (long)(i - minL[i]) * (minR[i] - i) * nums[i];
			maxsum += (long)(i - maxL[i]) * (maxR[i] - i) * nums[i];
		}

		return maxsum - minsum;
	}
}
```

### [3.5：最长特殊序列 Ⅰ](https://leetcode-cn.com/problems/longest-uncommon-subsequence-i/)

#### 题意

给你两个字符串 a 和 b，请返回 这两个字符串中 最长的特殊序列  。如果不存在，则返回 -1 。

「最长特殊序列」 定义如下：该序列为 某字符串独有的最长子序列（即不能是其他字符串的子序列） 。

字符串 s 的子序列是在从 s 中删除任意数量的字符后可以获得的字符串。

例如，“abc” 是 “aebdc” 的子序列，因为您可以删除 “aebdc” 中的下划线字符来得到 “abc” 。 “aebdc” 的子序列还包括 “aebdc” 、 “aeb” 和 “” (空字符串)。

**示例1：**

```
输入: a = "aba", b = "cdc"
输出: 3
解释: 最长特殊序列可为 "aba" (或 "cdc")，两者均为自身的子序列且不是对方的子序列。
```

**示例2：**

```
输入：a = "aaa", b = "bbb"
输出：3
解释: 最长特殊序列是“aaa”和“bbb”。
```

**示例3：**

```
输入：a = "aaa", b = "aaa"
输出：-1
解释: 字符串a的每个子序列也是字符串b的每个子序列。同样，字符串b的每个子序列也是字符串a的子序列。
```

**提示：**

- `1 <= a.length, b.length <= 100`
- `a` 和 `b` 由小写英文字母组成

#### 题解：思维

- 如果两个字符串相同，则无论如何都找不到特殊序列，于是返回-1
- 如果两个字符串不同，则长度长的字符串肯定是最长特殊序列。（当然如果长度相同，则两个任选一个都是）

**C++代码：**

```cpp
class Solution {
public:
    int findLUSlength(string a, string b) {
        int len1=a.length();
        int len2=b.length();
        if(a==b&&len1==len2){
            return -1;
        }
        return max(len1,len2);
    }
};
```

**Java代码：**

```java
class Solution {
    public int findLUSlength(String a, String b) {
        int len1=a.length();
        int len2=b.length();
        if(len1==len2&&a.equals(b)) return -1;
        return Math.max(len1,len2);
    }
}
```

### 扩展：[最长特殊序列 II](https://leetcode-cn.com/problems/longest-uncommon-subsequence-ii/)

#### 题意

给定字符串列表 strs ，返回 它们中 最长的特殊序列 。如果最长特殊序列不存在，返回 -1 。

最长特殊序列 定义如下：该序列为某字符串 独有的最长子序列（即不能是其他字符串的子序列）。

 s 的 子序列可以通过删去字符串 s 中的某些字符实现。

例如，"abc" 是 "aebdc" 的子序列，因为您可以删除"aebdc"中的下划线字符来得到 "abc" 。"aebdc"的子序列还包括"aebdc"、 "aeb" 和 "" (空字符串)。

**示例1：**

```
输入: strs = ["aba","cdc","eae"]
输出: 3
```

**示例2：**

```
输入: strs = ["aaa","aaa","aa"]
输出: -1
```

**提示：**

- `2 <= strs.length <= 50`
- `1 <= strs[i].length <= 10`
- `strs[i]` 只包含小写英文字母

#### 题解

首先需要知道这样一个性质：**最长的符合“该字符串不是数组中其他字符串的子序列”的子串一定是原字符串。**

> 证明：**只要某个子串s不是其他字符串的子串，那么这个这个子串在任意位置添加N个字符得到的结果ss肯定也不是其他字符串的子串。 所以对这个子串s而言，子串s进行添加字母的操作后最长的串就是原字符串。**

于是通过二重循环，判断所有字符串是否是其他字符串的子序列，例如：strs[i]不是其他所有字符串的子序列,则strs[i]可能是题目所求的最长特殊序列（因为可能还需要判断长度）

例如：s1,s2,s3都满足自身都不是其他字符串的子序列，则需要判断其中谁的长度最长，假设s3长度最长，则s3为题目所求的最长特殊序列

**C++代码：**

```cpp
class Solution {
public:
    bool check(string s1,string s2){
        int i,j;
        for(i=0,j=0;i<s1.length()&&j<s2.length();j++){
            if(s1[i]==s2[j]) i++;
        }
        return i==s1.length()?true:false;
    }
    int findLUSlength(vector<string>& strs) {
        int n=strs.size();
        int ans=-1;
        for(int i=0;i<n;i++){
            int flag=0;
            for(int j=0;j<n;j++){
                if(j==i) continue;
                //如果是前面的子序列，则直接结束循环
                if(check(strs[i],strs[j])){
                    flag=1;break;
                }
            }
            if(!flag) ans=max(ans,(int)strs[i].length());
        }
        return ans;
    }
};
```

### [3.6：适合打劫银行的日子](https://leetcode-cn.com/problems/find-good-days-to-rob-the-bank/)

#### 题意

你和一群强盗准备打劫银行。给你一个下标从 0 开始的整数数组 security ，其中 security[i] 是第 i 天执勤警卫的数量。日子从 0 开始编号。同时给你一个整数 time 。

如果第 i 天满足以下所有条件，我们称它为一个适合打劫银行的日子：

第 i 天前和后都分别至少有 time 天。
第 i 天前连续 time 天警卫数目都是非递增的。
第 i 天后连续 time 天警卫数目都是非递减的。
更正式的，第 i 天是一个合适打劫银行的日子当且仅当：security[i - time] >= security[i - time + 1] >= ... >= security[i] <= ... <= security[i + time - 1] <= security[i + time].

请你返回一个数组，包含 所有 适合打劫银行的日子（下标从 0 开始）。返回的日子可以 任意 顺序排列。

**示例1：**

```
输入：security = [5,3,3,3,5,6,2], time = 2
输出：[2,3]
解释：
第 2 天，我们有 security[0] >= security[1] >= security[2] <= security[3] <= security[4] 。
第 3 天，我们有 security[1] >= security[2] >= security[3] <= security[4] <= security[5] 。
没有其他日子符合这个条件，所以日子 2 和 3 是适合打劫银行的日子。
```

**示例2：**

```
输入：security = [1,1,1,1,1], time = 0
输出：[0,1,2,3,4]
解释：
因为 time 等于 0 ，所以每一天都是适合打劫银行的日子，所以返回每一天。
```

**示例3：**

```
输入：security = [1,2,3,4,5,6], time = 2
输出：[]
解释：
没有任何一天的前 2 天警卫数目是非递增的。
所以没有适合打劫银行的日子，返回空数组。
```

**提示：**

- $1 <= security.length <= 10^5$
- $0 <= security[i], time <= 10^5$

#### 题解：DP

首先很容易想到的是：直接遍历每一天，判断前后是否存在连续time天满足题意的情况。如果通过两重循环处理显然超时，于是想到预处理。

- pre[i]表示：i前面有pre[i]天警卫数量是非递增的
- suf[i]表示：i后面又suf[i]天警卫数量是递减的

预处理后，再遍历每一天通过数组访问判断即可。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> goodDaysToRobBank(vector<int>& security, int time) {
        int n=security.size();
        vector<int> ans;
        //pre[i]:i前面有pre[i]天警卫数量是非递增的
        //suf[i]:i后面又suf[i]天警卫数量是递减的
        vector<int> pre(n);vector<int> suf(n);
        pre[0]=0;
        for(int i=1;i<n;i++){
            if(security[i]<=security[i-1]) pre[i]=pre[i-1]+1;
            else pre[i]=0;
        }
        suf[n-1]=0;
        for(int i=n-2;i>=0;i--){
            if(security[i]<=security[i+1]) suf[i]=suf[i+1]+1;
            else suf[i]=0;
        }
        //确保前后至少都有time天，否则无意义
        for(int i=time;i<n-time;i++){
            if(pre[i]>=time&&suf[i]>=time){
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

### [3.7：七进制数](https://leetcode-cn.com/problems/base-7/)

#### 题意

给定一个整数 `num`，将其转化为 **7 进制**，并以字符串形式输出。

**示例1：**

```
输入: num = 100
输出: "202"
```

**示例2：**

```
输入: num = -7
输出: "-10"
```

**提示：**

- $-10^7 <= num <= 10^7$

#### 题解：模拟

直接模拟即可。但是需要注意：

- 0的情况需要特判
- 负数特殊处理

**C++代码：**

```cpp
class Solution {
public:
    string convertToBase7(int num) {
        if(num==0) return "0";
        string ans="";
        bool flag=false;
        if(num<0){
            flag=true;num=-num;
        }
        while(num){
            ans+=num%7+'0';num/=7;
        }
        if(flag) ans+='-';
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	public String convertToBase7(int num) {
        if(num==0) return "0";
		StringBuffer s1 = new StringBuffer();
		StringBuffer s2=new StringBuffer();
		boolean flag=false;
		if(num<0){
			num=-num;flag=true;
		}
		while(num>0){
			s1.append((char)(num%7+'0'));num/=7;
		}
		if(flag) s1.append('-');
		for(int i=s1.length()-1;i>=0;i--){
			s2.append(s1.charAt(i));
		}
		return new String(s2);
	}
}
```

### [3.8：蜡烛之间的盘子](https://leetcode-cn.com/problems/plates-between-candles/)

#### 题意：

给定一个字符串数组，含有 `*`和 `|`两个字符，分别表示盘子和蜡烛。现给出一个询问数组，每个元素给定一个区间 `[l,r]`，需要求得该区间中两支蜡烛之间的最大盘子数量

其中：

- $3 <= s.length <= 10^5$
- $s 只包含字符 '*' 和 '|'$
- $1 <= queries.length <= 10^5$
- $queries[i].length == 2$
- $0 <= left_i <= right_i < s.length$

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/ex-1.png)

```markdown
输入：s = `"**|**|***|"`, queries = [[2,5],[5,9]]
输出：[2,3]
解释：
- queries[0] 有两个盘子在蜡烛之间。
- queries[1] 有三个盘子在蜡烛之间。
```

**示例2：**

![ex-2](https://gitee.com/serendipity_LB/img/raw/master/ex-2.png)

```
输入：s = `"***|**|*****|**||**|*"` ，queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
输出：[9,0,0,0,0]
解释：
- queries[0] 有 9 个盘子在蜡烛之间。
- 另一个查询没有盘子在蜡烛之间。
```

#### 题解一：二分

1. 用一数组存储所有蜡烛出现的位置
2. 每次询问时，利用二分查找求得区间中第一次蜡烛出现的位置和最后一次蜡烛出现的位置
3. **$两个位置的距离差-区间中蜡烛的数量$**即为结果

**C++代码**

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

#### 题解二：前缀和

题解一是对于蜡烛位置的处理，我们同样可以思考对盘子进行处理：对盘子数量进行前缀和。

同时可以直接对每个位置左边第一个蜡烛的位置l[i]，和右边第一个蜡烛的位置r[i]进行预处理。

- 当询问区间为[x,y]时，通过l[x],r[y]更新区间，
- 此时区间中第一次出现蜡烛的位置为：$fx=r[x]$，最后一次出现蜡烛的位置为：$fy=l[y]$（因为l[i]表示位置i左边第一次出现蜡烛的位置，r[i]同理）
- 于是通过前缀和可以计算区间中满足条件的盘子数量为：$sum[fy]-sum[fx-1]$，但是由于fx,fy均为蜡烛的位置，可以转化为：$sum[fy]-sum[fx]$（为了防止fx=0的情况，fx-1越界）

**C++代码：**

```cpp
class Solution {
public:
    vector<int> platesBetweenCandles(string s, vector<vector<int>>& q) {
        int n=s.length();
        //盘子的前缀和
        vector<int> sum(n,0);
        //l[i]：位置i左边(包括自身位置)第一个蜡烛的位置
        //r[i]：位置i右边(包括自身位置)第一个蜡烛的位置
        vector<int> l(n,0),r(n,0);
        int left=-1,right=-1;
        //先预处理前缀和、l[]
        sum[0]=0;
        left=s[0]=='|'?0:-1;l[0]=left;
        for(int i=1;i<n;i++){
            if(s[i]=='*') sum[i]=sum[i-1]+1;
            else sum[i]=sum[i-1];
            if(s[i]=='|') left=i;
            l[i]=left;
        }
        //再预处理r[]
        for(int i=n-1;i>=0;i--){
            if(s[i]=='|') right=i;
            r[i]=right;
        }
        vector<int> ans;
        int m=q.size();
        for(int i=0;i<m;i++){
            int x=q[i][0],y=q[i][1];
            //记录x右边第一个蜡烛的位置和y左边第一个蜡烛的位置
            x=r[x];y=l[y];
            //不存在的情况
            if(x==-1||y==-1||x>y) ans.push_back(0);
            //由于x、y均为蜡烛的位置，期间盘子的数量可以表示为sum[y]-sum[x]
            else ans.push_back(sum[y]-sum[x]);
        }
        return ans;
    }
};
```

### [3.9：得分最高的最小轮调](https://leetcode-cn.com/problems/smallest-rotation-with-highest-score/)

#### 题意

给你一个数组 nums，我们可以将它按一个非负整数 k 进行轮调，这样可以使数组变为 [nums[k], nums[k + 1], ... nums[nums.length - 1], nums[0], nums[1], ..., nums[k-1]] 的形式。此后，任何值小于或等于其索引的项都可以记作一分。

例如，数组为 nums = [2,4,1,3,0]，我们按 k = 2 进行轮调后，它将变成 [1,3,0,2,4]。这将记为 3 分，因为 1 > 0 [不计分]、3 > 1 [不计分]、0 <= 2 [计 1 分]、2 <= 3 [计 1 分]，4 <= 4 [计 1 分]。
在所有可能的轮调中，返回我们所能得到的最高分数对应的轮调下标 k 。如果有多个答案，返回满足条件的最小的下标 k 。

**示例1：**

```
输入：nums = [2,3,1,4,0]
输出：3
解释：
下面列出了每个 k 的得分：
k = 0,  nums = [2,3,1,4,0],    score 2
k = 1,  nums = [3,1,4,0,2],    score 3
k = 2,  nums = [1,4,0,2,3],    score 3
k = 3,  nums = [4,0,2,3,1],    score 4
k = 4,  nums = [0,2,3,1,4],    score 3
所以我们应当选择 k = 3，得分最高。

```

**示例2：**

```
输入：nums = [1,3,0,2,4]
输出：0
解释：
nums 无论怎么变化总是有 3 分。
所以我们将选择最小的 k，即 0。
```

**提示：**

- $1 <= nums.length <= 10^5$
- $0 <= nums[i] < nums.length$

#### 题解

困难题复制粘贴...

思路分析：这道题最简单的思路：直接移动，然后计算分数，再取最高值。但是时间复杂度为O(n^2)，A的长度比较大，显然不行。

我们以A=[2,3,1,4,0]为例寻找规律:

A[0]=2移动到 2 号索引位置[4,0,2,3,1]其对应的K为3=(0-A[0]+5)%5
A[1]=3移动到 3 号索引位置[0,2,3,1,4]其对应的K为3=(1-A[1]+5)%5
A[2]=1移动到 1 号索引位置[3,1,4,0,2]其对应的K为1=(2-A[2]+5)%5
A[3]=4移动到 4 号索引位置[0,2,3,1,4]其对应的K为1=(3-A[3]+5)%5
A[4]=0移动到 0 号索引位置[0,2,3,1,4]其对应的K为3=(4-A[4]+5)%5

由此可以得出一个公式，将A[i]向左移动到下标A[A[i]]的位置需要K = (i - A[i] + N) % N
并且我们发现，A[A[i]]是第一个A[i]能得分的位置，如果这时减小K，则A[i]继续得分，如果增大K则A[i]将不得分。
如果我们能够刚好把所有A[i]都移动到A[A[i]]的位置，那么我们拿到的分数肯定的是最高的，蛋式这种情况几乎不可能。

当我们把A[i]移动到A[A[i]]后，再向左移动一个位置（即K增加1）。A[i]的移动公式为K’ = (1 + i - A[i] + N) % N这个时候A[i]刚好不得分。

我们可以在这个刚好不得分的k标记一下，通过+1进行标记，这个k就是 (i - A[i] + 1 + N) % N。用一个长度为N
的myK数组，对于每个元素A[i]，我们都找到其刚好不得分的k = (i - A[i] + 1 + N) % N，那么此时myK[k]就表示
数组中的数字在K = k时，A数组中不得分的元素个数。

可以发现，如果当K = k时，A[i]刚好不得分，当K = k + 1时（左移一个）A[i]继续不得分，蛋式当K = k + 1时
有一个元素开始得分了，就是在当K = k处于A[0]的元素开始得分！！！

因此递推公式为：myK[k + 1] += myK[k] - 1

**C++代码：**

```cpp
class Solution {
public:
    int bestRotation(vector<int>& A) {
        int n=A.size();int ansk=0;   //ansk表示：K=ansk时，此时A数组中得分个数最多
        //myk[k]表示：K=k时，A数组中不得分的个数
        //第一步：将A数组中所有元素都向左移动（i-A[i]+1+n)%n 个位置，即K=（i-A[i]+1+n)%n 此时A[i]刚好不得分
        vector<int> myk(n,0);
        for(int i=0;i<n;i++){
            myk[(i-A[i]+1+n)%n]+=1;   //当K=（i-A[i]+1+n）%n，不得分个数自增
        }
        //第二步：寻找最优的ansk（当K=ansk时，A数组中得分个数最多）
        for(int i=1;i<n;i++){
            //递推式当K = i - 1增大到到K = i时
            //在K = i - 1时不得分的继续不得分，但是当K = i - 1转换到K = i时，处于A[0]的元素开始得分
            myk[i]+=myk[i-1]-1;
            if(myk[i]<myk[ansk]){
                //K=ansk时，此时A数组中得分个数最多
                ansk=i;
            }
        }
        return ansk;
    }
};
```

### [3.10：N 叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

#### 题意

给定一个 n 叉树的根节点  root ，返回 其节点值的 前序遍历 。

n 叉树 在输入中按层序遍历进行序列化表示，每组子节点由空值 null 分隔（请参见示例）。

**示例1：**

<img src="https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png" alt="img" style="zoom:67%;" />

```
输入：root = [1,null,3,2,4,null,5,6]
输出：[1,3,5,6,2,4]
```

**示例2：**

<img src="https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png" alt="img" style="zoom:67%;" />

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[1,2,3,6,7,11,14,4,8,12,5,9,13,10]
```

**提示：**

- 节点总数在范围 $[0, 10^4]$内
- $0 <= Node.val <= 10^4$
- n 叉树的高度小于或等于 `1000`

#### 题解一：递归

直接参照二叉树前序遍历即可，很简单。

**C++代码：**

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> ans;
    void dfs(Node* root){
        if(root==NULL) return ;
        ans.push_back(root->val);
        for(int i=0;i<root->children.size();i++){
            dfs(root->children[i]);
        }
    }
    vector<int> preorder(Node* root) {
        ans.clear();
        dfs(root);
        return ans;
    }
};
```

#### 题解二：迭代

通过栈模拟递归操作。前序遍历，我们先遍历当前节点，再从左到右遍历其每个子树。由于栈是**后进先出**的原理，于是我们从右到左入栈，然后出栈顺序便满足了从左到右遍历每个子树。

**C++代码：**

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> ans;
        if(root==NULL) return ans;
        stack<Node*> sta;
        sta.push(root);
        while(!sta.empty()){
            Node* now=sta.top();sta.pop();
            ans.push_back(now->val);
            for(int i=now->children.size()-1;i>=0;i--) sta.push(now->children[i]);
        }
        return ans;
    }
};
```

**Java代码：**

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<Integer> preorder(Node root) {
        List<Integer> ans=new ArrayList<>();
        if(root==null) return ans;
        Deque<Node> stack=new ArrayDeque<>();
        stack.push(root);   //尾部插入
        while(!stack.isEmpty()){
            Node now=stack.poll();   //检索并删除
            ans.add(now.val); 
            for(int i=now.children.size()-1;i>=0;i--){
                stack.push(now.children.get(i));
            }
        }
        return ans;
    }
}
```

### [3.11：统计最高分的节点数目](https://leetcode-cn.com/problems/count-nodes-with-the-highest-score/)

#### 题意

给你一棵根节点为 0 的 二叉树 ，它总共有 n 个节点，节点编号为 0 到 n - 1 。同时给你一个下标从 0 开始的整数数组 parents 表示这棵树，其中 parents[i] 是节点 i 的父节点。由于节点 0 是根，所以 parents[0] == -1 。

一个子树的 大小 为这个子树内节点的数目。每个节点都有一个与之关联的 分数 。求出某个节点分数的方法是，将这个节点和与它相连的边全部 删除 ，剩余部分是若干个 非空 子树，这个节点的 分数 为所有这些子树 大小的乘积 。

请你返回有 最高得分 节点的 数目 。

**示例1：**

![example-1](https://gitee.com/serendipity_LB/img/raw/master/example-1.png)

```
输入：parents = [-1,2,0,2,0]
输出：3
解释：
- 节点 0 的分数为：3 * 1 = 3
- 节点 1 的分数为：4 = 4
- 节点 2 的分数为：1 * 1 * 2 = 2
- 节点 3 的分数为：4 = 4
- 节点 4 的分数为：4 = 4
最高得分为 4 ，有三个节点得分为 4 （分别是节点 1，3 和 4 ）。
```

**示例2：**

![example-2](https://gitee.com/serendipity_LB/img/raw/master/example-2.png)

```
输入：parents = [-1,2,0]
输出：2
解释：
- 节点 0 的分数为：2 = 2
- 节点 1 的分数为：2 = 2
- 节点 2 的分数为：1 * 1 = 1
最高分数为 2 ，有两个节点分数为 2 （分别为节点 0 和 1 ）。
```

**提示：**

- n == parents.length
- $2 <= n <= 10^5$
- parents[0] == -1
- 对于 i != 0 ，有 0 <= parents[i] <= n - 1
- parents 表示一棵二叉树。

#### 题解：DFS

首先将二叉树还原，可以直接通过二维数组保存。直接定义两个数组：cnt[]，score[]，分别表示：

- cnt[i]：记录以i为根节点的子树节点个数（包括自身）
- score[i]：表示当前节点的分数

遍历整个二叉树，保存cnt[i]；对于二叉树中节点可以分为三类：

- 有左孩子或右孩子（叶子节点）
- 有左孩子、右孩子，无父亲节点（也就是整个二叉树的根节点）
- 有左孩子、右孩子和父亲节点（中间结点）

现在需要计算当删除当前节点i所连的所有边后，计算剩余非空子树大小的乘积。

假设当前节点为i，左孩子为l（如果存在），右孩子为r（如果存在），父亲节点为par（如果存在）

于是：score[i]=cnt[l]\*cnt[r]\*cnt[par]

如果其中有不存在的即可不用计算。于是从score[]数组中选出最高得分，并判断存在多少个即可。

注意：数据量比较大，需要开long long

**C++代码：**

```cpp
class Solution {
public:
    typedef long long ll;
    vector<int> tree[100010];
    int cnt[100010];
    ll score[100010];
    int dfs(int root){
        int res=1;
        for(int x:tree[root]){
            res+=dfs(x);
        }
        cnt[root]=res;
        return res;
    }
    int countHighestScoreNodes(vector<int>& parents) {
        int n=parents.size();
        for(int i=0;i<n;i++){
            if(parents[i]==-1) continue;
            tree[parents[i]].push_back(i);
        }
        dfs(0);
        ll maxnum=0,res=1,num=1;
        for(int i=0;i<n;i++){
            res=1,num=1;
            for(int x:tree[i]){
                num*=cnt[x];res+=cnt[x];
            }
            res=n-res;
            if(res!=0) num*=res;
            score[i]=num;
            maxnum=max(maxnum,num);
        }
        int ans=0;
        for(int i=0;i<n;i++) if(score[i]==maxnum) ans++;
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	//表示树
	List<List<Integer>> tree;
	//cnt[i]:记录以i为根节点的子树节点个数（包括自身）
	int cnt[];
	//score[i]:表示当前节点的分数
	long score[];
	int dfs(int root){
		int res=1;
		for(int i=0;i<tree.get(root).size();i++){
			res+=dfs(tree.get(root).get(i));
		}
		cnt[root]=res;
		return res;
	}
	public int countHighestScoreNodes(int[] parents) {
        int n = parents.length;
        tree=new ArrayList<>();
        cnt=new int[n];score=new long[n];
		for(int i=0;i<n;i++){
			tree.add(new ArrayList<>());
		}
		for(int i=0;i<n;i++){
			if(parents[i]==-1) continue;
			else{
				tree.get(parents[i]).add(i);
			}
		}
		dfs(0);
		long maxnum=0,num=1,resnum=1;
		for(int i=0;i<n;i++){
            num=1;resnum=1;   //resnum==1，是因为需要删除当前节点
			for(int x:tree.get(i)){
				resnum+=cnt[x];num*=cnt[x];
			}
			//如果当前节点存在父节点，那除了子树节点外，还存在第三堆，如样例1节点2所示
			resnum=n-resnum;
            if(resnum!=0) num*=resnum;
			score[i]=num;
			maxnum=Math.max(maxnum, num);
		}
		int ans=0;
		for(long x:score){
			if(x==maxnum) ans++;
		}
		return ans;
	}
}
```

### [3.12：N 叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

#### 题意

给定一个 n 叉树的根节点 root ，返回 其节点值的 后序遍历 。

n 叉树 在输入中按层序遍历进行序列化表示，每组子节点由空值 null 分隔（请参见示例）。

**示例1：**

![img](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
输入：root = [1,null,3,2,4,null,5,6]
输出：[5,6,3,2,4,1]
```

**示例2：**

![img](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[2,6,14,11,7,3,12,8,4,13,9,10,5,1]
```

**提示：**

- 节点总数在范围 $[0, 10^4]$ 内
- $0 <= Node.val <= 10^4$
- n 叉树的高度小于或等于 `1000`

#### 题解一：递归

和3.10的题目类似

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> ans;
    void dfs(Node* root){
        if(root==NULL) return ;
        for(int i=0;i<root->children.size();i++){
            dfs(root->children[i]);
        }
        ans.push_back(root->val);
    }
    vector<int> postorder(Node* root) {
        ans.clear();
        dfs(root);
        return ans;
    }
};
```

#### 题解二：迭代

见3.10题解

**C++代码**

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<int> postorder(Node* root) {
        vector<int> ans;
        if(root==NULL) return ans;
        stack<Node*> sta;
        set<Node*> vis;
        sta.push(root);
        while(!sta.empty()){
            Node* now=sta.top();
            if(now->children.size()==0||vis.count(now)){
                ans.push_back(now->val);
                sta.pop();continue;
            }
            for(int i=now->children.size()-1;i>=0;i--){
                sta.push(now->children[i]);
            }
            vis.insert(now);
        }
        return ans;
    }
};
```

**Java代码：**

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<Integer> postorder(Node root) {
        List<Integer> ans=new ArrayList<>();
        if(root==null) return ans;
        Deque<Node> stack=new ArrayDeque<>();
        Set<Node> set=new HashSet<>();
        stack.push(root);
        while(!stack.isEmpty()){
            Node now=stack.peek();
            if(now.children.size()==0||set.contains(now)){
                ans.add(now.val);stack.pop();continue;
            }
            //从尾部插入
            for(int i=now.children.size()-1;i>=0;i--){
                stack.push(now.children.get(i));
            }
            set.add(now);
        }
        return ans;
    }
}
```

### [3.14：两个列表的最小索引总和](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

#### 题意

假设 Andy 和 Doris 想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。

你需要帮助他们用最少的索引和找出他们共同喜爱的餐厅。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设答案总是存在。

**示例1：**

```
输入: list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
输出: ["Shogun"]
解释: 他们唯一共同喜爱的餐厅是“Shogun”。
```

**示例2：**

```
输入:list1 = ["Shogun", "Tapioca Express", "Burger King", "KFC"]，list2 = ["KFC", "Shogun", "Burger King"]
输出: ["Shogun"]
解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
```

**提示：**

- 1 <= list1.length, list2.length <= 1000
- 1 <= list1[i].length, list2[i].length <= 30
- list1[i] 和 list2[i] 由空格 ' ' 和英文字母组成。
- list1 的所有字符串都是 唯一 的。
- list2 中的所有字符串都是 唯一 的。

#### 题解一：暴力

直接两重循环判断

**Java代码：**

```java
class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        List<String> list=new ArrayList<>();
        int num=0x3f3f3f3f;
        for(int i=0;i<list1.length;i++){
            for(int j=0;j<list2.length;j++){
                if(list1[i].equals(list2[j])){
                    if(i+j<num){
                        num=i+j;
                        list.clear();
                        list.add(list2[j]);
                    }else if(i+j==num) list.add(list2[j]);
                }
            }
        }
        return list.toArray(new String[0]);
    }
}
```

#### 题解二：哈希

通过哈希表存储其中一个集合，这里对list1进行存储，key表示字符串，value表示字符串在list1中出现的位置。

于是通过遍历list2，判断list2[i]是否在哈希表中出现，如果出现在比较索引大小即可。

**C++代码：**

```cpp
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        map<string,int> mp;
        vector<string> ans;
        int num=0x3f3f3f3f;
        int n=list1.size();int m=list2.size();
        for(int i=0;i<n;i++){
            mp[list1[i]]=i;
        }
        for(int j=0;j<m;j++){
            if(mp.count(list2[j])){
                if(mp[list2[j]]+j<num){
                    num=mp[list2[j]]+j;
                    ans.clear();ans.push_back(list2[j]);
                }else if(mp[list2[j]]+j==num){
                    ans.push_back(list2[j]);
                }
            }
        }
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        List<String> list=new ArrayList<>();
        Map<String,Integer> map=new HashMap<String,Integer>();
        int num=0x3f3f3f3f;
        int n=list1.length;int m=list2.length;
        for(int i=0;i<n;i++){
            map.put(list1[i],i);
        }
        for(int j=0;j<m;j++){
            if(map.containsKey(list2[j])){
                if(map.get(list2[j])+j<num){
                    num=map.get(list2[j])+j;list.clear();
                    list.add(list2[j]);
                }else if(map.get(list2[j])+j==num){
                    list.add(list2[j]);
                }
            }
        }
        return list.toArray(new String[list.size()]);
    }
}
```

### [3.15：统计按位或能得到最大值的子集数目](https://leetcode-cn.com/problems/count-number-of-maximum-bitwise-or-subsets/)

#### 题意

给你一个整数数组 nums ，请你找出 nums 子集 按位或 可能得到的 最大值 ，并返回按位或能得到最大值的 不同非空子集的数目 。

如果数组 a 可以由数组 b 删除一些元素（或不删除）得到，则认为数组 a 是数组 b 的一个 子集 。如果选中的元素下标位置不一样，则认为两个子集 不同 。

对数组 a 执行 按位或 ，结果等于 a[0] OR a[1] OR ... OR a[a.length - 1]（下标从 0 开始）。

**示例1：**

```
输入：nums = [3,1]
输出：2
解释：子集按位或能得到的最大值是 3 。有 2 个子集按位或可以得到 3 ：
- [3]
- [3,1]
```

**示例2：**

```
输入：nums = [2,2,2]
输出：7
解释：[2,2,2] 的所有非空子集的按位或都可以得到 2 。总共有 23 - 1 = 7 个子集。
```

**示例3：**

```
输入：nums = [3,2,1,5]
输出：6
解释：子集按位或可能的最大值是 7 。有 6 个子集按位或可以得到 7 ：
- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]
```

**提示：**

- $1 <= nums.length <= 16$
- $1 <= nums[i] <= 10^5$

#### 题解：枚举

首先数组长度最大为16，可知子集最大个数为：$2^{16}=65536$，对于每个子集的按位或操作将是个常数，最大为16，时间复杂度不大，可以考虑枚举。

枚举可以用**二进制**枚举也可以通过**回溯**枚举。这里采用回溯枚举。

因为是按位或操作，所以**整个数组的按位或将是最大值**（理解或的基本操作后，得出这个结论并不难），同时整个数组也是一个子集，于是只需判断其他子集的按位或是否为这个值即可。

**C++代码：**

```cpp
class Solution {
public:
    int vis[20],maxnum,ans;
    void dfs(int x,int n,vector<int>& a){
        if(x==n) return ;
        vis[x]=1;int num=0;
        for(int i=0;i<=x;i++){
            if(vis[i]==1) num|=a[i];
        }
        if(num==maxnum) ans++;
        dfs(x+1,n,a);
        vis[x]=0;
        dfs(x+1,n,a);
    }
    int countMaxOrSubsets(vector<int>& nums) {
        maxnum=0;memset(vis,0,sizeof(vis));
        int n=nums.size();
        //整个数组元素的按位或绝对是所能满足的最大值
        for(int i=0;i<n;i++) maxnum|=nums[i];
        ans=0;
        dfs(0,n,nums);
        return ans;
    }
};
```

### [3.17：词典中最长的单词](https://leetcode-cn.com/problems/longest-word-in-dictionary/)

给出一个字符串数组 words 组成的一本英语词典。返回 words 中最长的一个单词，该单词是由 words 词典中其他单词逐步添加一个字母组成。

若其中有多个可行的答案，则返回答案中字典序最小的单词。若无答案，则返回空字符串。

**示例1：**

```
输入：words = ["w","wo","wor","worl", "world"]
输出："world"
解释： 单词"world"可由"w", "wo", "wor", 和 "worl"逐步添加一个字母组成。
```

**示例2：**

```
输入：words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
输出："apple"
解释："apply" 和 "apple" 都能由词典中的单词组成。但是 "apple" 的字典序小于 "apply"
```

**提示：**

- 1 <= words.length <= 1000
- 1 <= words[i].length <= 30
- 所有输入的字符串 words[i] 都只包含小写字母。

#### 题解：哈希

题目需要判断是否存在一个单词，它由词典中其他单词逐步添加一个字母组成。同时它的长度最长，且字典序最小。

我们可以通过set保存词典中所有的字符串，然后遍历所有的字符串

1. 判断当前字符串是否由词典中其他单词逐步添加一个字母组成
2. 如果条件1满足，则判断是否是最长的字符串，如果不是则更新。
3. 如果和原来保存的字符串长度相同，则判断是否是字典序最小

通过上述三步操作，则可得到答案。

```cpp
class Solution {
public:
    string longestWord(vector<string>& words) {
        int n=words.size();
        set<string> st;
        for(int i=0;i<n;i++) st.insert(words[i]);
        string ans="";int maxlen=0,len;
        for(int i=0;i<n;i++){
            len=words[i].length();
            //首先判断是否由词典中其他单词逐步添加一个字母组成
            bool flag=true;
            for(int j=0;j<len-1;j++){
                if(!st.count(words[i].substr(0,j+1))){
                    flag=false;break;
                }
            }
            if(flag){
                //判断是否最长
                if(len>maxlen){
                    ans=words[i],maxlen=len;
                }else if(len==maxlen){
                    //判断是否字典序最小
                    if(ans=="") ans=words[i],maxlen=len;
                    else if(ans>words[i]) ans=words[i],maxlen=len;
                }
            }
        }
        return ans;
    }
};
```

### [3.21：两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

给定一个二叉搜索树 `root` 和一个目标结果 `k`，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 `true`。

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/sum_tree_1.jpg)

```
输入: root = [5,3,6,2,4,null,7], k = 9
输出: true
```

**示例2：**

![img](https://gitee.com/serendipity_LB/img/raw/master/sum_tree_2.jpg)

```
输入: root = [5,3,6,2,4,null,7], k = 28
输出: false
```

**提示：**

- 二叉树的节点个数的范围是 $[1, 10^4]$.
- $-10^4 <= Node.val <= 10^4$
- `root` 为二叉搜索树
- $-10^5 <= k <= 10^5$

#### 题解：dfs+哈希

这题方法很多，dfs和bfs都可以，同时加上BST的特殊性，还可以通过中序遍历+双指针解决。这里选择最简单的 dfs+哈希。

首先遍历整个BST，将各节点的值存入到哈希表中，然后遍历哈希表，判断是否存在两个不同的数的和等于k即可。

**C++代码：**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    map<int,int> mp;
    void dfs(TreeNode* root){
        if(root==NULL) return ;
        mp[root->val]=1;
        dfs(root->left);
        dfs(root->right);
    }
    bool findTarget(TreeNode* root, int k) {
        mp.clear();
        dfs(root);
        for(auto it:mp){
            if(mp.count(k-it.first)&&it.first*2!=k){
                return true;
            }
        }
        return false;
    }
};
```

### [3.22：如果相邻两个颜色均相同则删除当前颜色](https://leetcode-cn.com/problems/remove-colored-pieces-if-both-neighbors-are-the-same-color/)

总共有 n 个颜色片段排成一列，每个颜色片段要么是 'A' 要么是 'B' 。给你一个长度为 n 的字符串 colors ，其中 colors[i] 表示第 i 个颜色片段的颜色。

Alice 和 Bob 在玩一个游戏，他们 轮流 从这个字符串中删除颜色。Alice 先手 。

如果一个颜色片段为 'A' 且 相邻两个颜色 都是颜色 'A' ，那么 Alice 可以删除该颜色片段。Alice 不可以 删除任何颜色 'B' 片段。
如果一个颜色片段为 'B' 且 相邻两个颜色 都是颜色 'B' ，那么 Bob 可以删除该颜色片段。Bob 不可以 删除任何颜色 'A' 片段。
Alice 和 Bob 不能 从字符串两端删除颜色片段。
如果其中一人无法继续操作，则该玩家 输 掉游戏且另一玩家 获胜 。
假设 Alice 和 Bob 都采用最优策略，如果 Alice 获胜，请返回 true，否则 Bob 获胜，返回 false。

**示例1：**

```
输入：colors = "AAABABB"
输出：true
解释：
AAABABB -> AABABB
Alice 先操作。
她删除从左数第二个 'A' ，这也是唯一一个相邻颜色片段都是 'A' 的 'A' 。

现在轮到 Bob 操作。
Bob 无法执行任何操作，因为没有相邻位置都是 'B' 的颜色片段 'B' 。
因此，Alice 获胜，返回 true 。
```

**示例2：**

```
输入：colors = "AA"
输出：false
解释：
Alice 先操作。
只有 2 个 'A' 且它们都在字符串的两端，所以她无法执行任何操作。
因此，Bob 获胜，返回 false 。
```

**示例3：**

```
输入：colors = "ABBBBBBBAAA"
输出：false
解释：
ABBBBBBBAAA -> ABBBBBBBAA
Alice 先操作。
她唯一的选择是删除从右数起第二个 'A' 。

ABBBBBBBAA -> ABBBBBBAA
接下来轮到 Bob 操作。
他有许多选择，他可以选择任何一个 'B' 删除。

然后轮到 Alice 操作，她无法删除任何片段。
所以 Bob 获胜，返回 false 。
```

**提示：**

- $1 <= colors.length <= 10^5$
- `colors` 只包含字母 `'A'` 和 `'B'`

#### 题解：博弈

通过题意知道Alice只能对A操作，Bob只能对B操作。于是我们只需单独考虑彼此的最优解即可。

对于Alice而言，只有两边都是A的时候才能对中间的A进行操作，假设此时有连续n个A，如果n>=3，则Alice可以进行n-2次操作，Bob同理。

于是分别计算出Alice和Bob最多可以进行的操作次数，然后判断即可。

```cpp
class Solution {
public:
    bool winnerOfGame(string colors) {
        int num1=0,num2=0;
        int n=colors.length();
        //计算alice最多可以进行多少次操作
        for(int i=0;i<n;i++){
            if(colors[i]=='A'){
                int j=i,num=0;
                while(j<n&&colors[j]=='A'){
                    ++num;++j;
                }
                if(num>=3) num1+=num-2;
                i=j-1;
            }
        }
        //计算Bob最多可以进行多少次操作
        for(int i=0;i<n;i++){
            if(colors[i]=='B'){
                int j=i,num=0;
                while(j<n&&colors[j]=='B'){
                    ++num;++j;
                }
                if(num>=3) num2+=num-2;
                i=j-1;
            }
        }
        return num1>num2?true:false;
    }
};
```

### [3.23：字典序的第K小数字](https://leetcode-cn.com/problems/k-th-smallest-in-lexicographical-order/)

给定整数 `n` 和 `k`，返回 `[1, n]` 中字典序第 `k` 小的数字。

**示例1：**

```
输入: n = 13, k = 2
输出: 10
解释: 字典序的排列是 [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]，所以第二小的数字是 10。
```

**示例2：**

```
输入: n = 1, k = 1
输出: 1
```

**提示：**

- $1 <= k <= n <= 10^9$

#### 题解：字典树

[优质题解](https://leetcode-cn.com/problems/k-th-smallest-in-lexicographical-order/solution/wu-xu-jie-zhu-shi-cha-shu-ye-neng-rong-yi-li-jie-b/)

```cpp
class Solution {
public:
    typedef long long ll;
    ll get_cnt(ll i,ll n){
        ll cnt=0;
        for(ll a=i,b=i+1;a<=n;a*=10,b*=10){
            cnt+=min(n+1,b)-a;
        }
        return cnt;
    }
    int findKthNumber(int n, int k) {
        ll i=1,p=1;
        while(p<k){
            ll cnt=get_cnt(i,n);
            if(p+cnt>k){
                i*=10;p++;
            }else{
                i++;p+=cnt;
            }
        }
        return i;
    }
};
```

### [3.24：图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

图像平滑器 是大小为 3 x 3 的过滤器，用于对图像的每个单元格平滑处理，平滑处理后单元格的值为该单元格的平均灰度。

每个单元格的  平均灰度 定义为：该单元格自身及其周围的 8 个单元格的平均值，结果需向下取整。（即，需要计算蓝色平滑器中 9 个单元格的平均值）。

如果一个单元格周围存在单元格缺失的情况，则计算平均灰度时不考虑缺失的单元格（即，需要计算红色平滑器中 4 个单元格的平均值）。

![img](https://gitee.com/serendipity_LB/img/raw/master/smoother-grid.jpg)

给你一个表示图像灰度的 `m x n` 整数矩阵 `img` ，返回对图像的每个单元格平滑处理后的图像 。

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/smooth-grid.jpg)

```
输入:img = [[1,1,1],[1,0,1],[1,1,1]]
输出:[[0, 0, 0],[0, 0, 0], [0, 0, 0]]
解释:
对于点 (0,0), (0,2), (2,0), (2,2): 平均(3/4) = 平均(0.75) = 0
对于点 (0,1), (1,0), (1,2), (2,1): 平均(5/6) = 平均(0.83333333) = 0
对于点 (1,1): 平均(8/9) = 平均(0.88888889) = 0
```

**示例2：**

![img](https://gitee.com/serendipity_LB/img/raw/master/smooth2-grid.jpg)

```
输入: img = [[100,200,100],[200,50,200],[100,200,100]]
输出: [[137,141,137],[141,138,141],[137,141,137]]
解释:
对于点 (0,0), (0,2), (2,0), (2,2): floor((100+200+200+50)/4) = floor(137.5) = 137
对于点 (0,1), (1,0), (1,2), (2,1): floor((200+200+50+200+100+100)/6) = floor(141.666667) = 141
对于点 (1,1): floor((50+200+200+200+200+100+100+100+100)/9) = floor(138.888889) = 138
```

**提示：**

- `m == img.length`
- `n == img[i].length`
- `1 <= m, n <= 200`
- `0 <= img[i][j] <= 255`

#### 题解：模拟

由于数据不大，直接按照题意模拟即可

```cpp
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& img) {
        int m=img.size(),n=img[0].size();
        vector<vector<int> > ans(m,vector<int>(n));
        int num=0,cnt=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                num=0;cnt=0;
                for(int x=max(i-1,0);x<=min(i+1,m-1);x++){
                    for(int y=max(j-1,0);y<=min(j+1,n-1);y++){
                        // cout<<img[x][y]<<" ";
                        num+=img[x][y];++cnt;
                    }
                }
                // cout<<endl;
                ans[i][j]=num/cnt;
            }
        }
        return ans;
    }
};
```

### [3.25：阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

给定一个整数 `n` ，返回 `n!` 结果中尾随零的数量。

提示 `n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1`

**示例1：**

```
输入：n = 3
输出：0
解释：3! = 6 ，不含尾随 0
```

**示例2：**

```
输入：n = 5
输出：1
解释：5! = 120 ，有一个尾随 0
```

**示例3：**

```
输入：n = 0
输出：0
```

**提示：**

- $0 <= n <= 10^4$

#### 题解：数学

由于是求后面零的数量，我们可以知道产生0的情况只能是2*5（当然2的倍数 \* 5的倍数也可以）

我们假设需要求n的阶乘，即：1 2 3 4 5 6 .... n

显然因式分解能够产生2的个数比5的个数会大，可以简单证明：

```
n=5时，2的倍数个数共有3个（2,4=2*2），5的倍数共有1个（5）
n=10时，2的倍数共有8个（2,4=2*2,6,8=2*2*2,10），5的倍数共有2个（5,10）
....
```

于是我们只需要求能够因式分解产生5的个数总和即可

**C++代码：**

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int cnt_5=0;
        for(int i=5;i<=n;i+=5){
            int x=i;
            while(x%5==0&&x>=5){
                x/=5;cnt_5++;
            }
        }
        return cnt_5;
    }
};
```

### [3.26：棒球比赛](https://leetcode-cn.com/problems/baseball-game/)

#### 题解：栈

```cpp
class Solution {
public:
    int get_num(string x){
        bool flag=x[0]=='-'?true:false;
        int i=0,len=x.length();
        if(flag) ++i;
        int res=0;
        while(i<len) res=res*10+(x[i++]-'0');
        if(flag) res*=-1;
        return res;
    }
    int calPoints(vector<string>& ops) {
        stack<int> sta;
        int n=ops.size(),m,x,y;
        for(int i=0;i<n;i++){
            if(ops[i]=="C"){
                sta.pop();
            }else if(ops[i]=="D"){
                x=sta.top();sta.push(2*x);
            }else if(ops[i]=="+"){
                x=sta.top();sta.pop();
                y=sta.top();sta.pop();
                sta.push(y);sta.push(x);sta.push(x+y);
            }else{
                sta.push(get_num(ops[i]));
            }
        }
        int ans=0;
        while(!sta.empty()) ans+=sta.top(),sta.pop();
        return ans;
    }
};
```

### [3.28：交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/)

给定一个正整数，检查它的二进制表示是否总是 0、1 交替出现：换句话说，就是二进制表示中相邻两位的数字永不相同。

**示例1：**

```
输入：n = 5
输出：true
解释：5 的二进制表示是：101
```

**示例2：**

```
输入：n = 7
输出：false
解释：7 的二进制表示是：111.
```

**示例3：**

```
输入：n = 11
输出：false
解释：11 的二进制表示是：1011.
```

**提示：**

- $1 <= n <= 2^{31} - 1$

#### 题解：模拟

```cpp
class Solution {
public:
    bool hasAlternatingBits(int n) {
        bool flag=false;
        int prenum;
        while(n){
            if(flag){
                prenum=n%2;
            }else if(prenum==n%2) return false;
            prenum=n%2;
            n/=2;
        }
        return true;
    }
};
```

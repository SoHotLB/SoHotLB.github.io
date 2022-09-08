---
title: LeetCode第286场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第286场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-286/

**四题双百解法**

### [一、找出两数组的不同](https://leetcode-cn.com/problems/find-the-difference-of-two-arrays/)

给你两个下标从 0 开始的整数数组 nums1 和 nums2 ，请你返回一个长度为 2 的列表 answer ，其中：

answer[0] 是 nums1 中所有 不 存在于 nums2 中的 不同 整数组成的列表。
answer[1] 是 nums2 中所有 不 存在于 nums1 中的 不同 整数组成的列表。
注意：列表中的整数可以按 任意 顺序返回。

**示例1：**

```
输入：nums1 = [1,2,3], nums2 = [2,4,6]
输出：[[1,3],[4,6]]
解释：
对于 nums1 ，nums1[1] = 2 出现在 nums2 中下标 0 处，然而 nums1[0] = 1 和 nums1[2] = 3 没有出现在 nums2 中。因此，answer[0] = [1,3]。
对于 nums2 ，nums2[0] = 2 出现在 nums1 中下标 1 处，然而 nums2[1] = 4 和 nums2[2] = 6 没有出现在 nums2 中。因此，answer[1] = [4,6]。
```

**示例2：**

```
输入：nums1 = [1,2,3,3], nums2 = [1,1,2,2]
输出：[[3],[]]
解释：
对于 nums1 ，nums1[2] 和 nums1[3] 没有出现在 nums2 中。由于 nums1[2] == nums1[3] ，二者的值只需要在 answer[0] 中出现一次，故 answer[0] = [3]。
nums2 中的每个整数都在 nums1 中出现，因此，answer[1] = [] 。 
```

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `-1000 <= nums1[i], nums2[i] <= 1000`

#### 题解：集合

使用两个set集合分别存储nums1和nums2出现的元素。

寻找`nums1` 中所有 **不** 存在于 `nums2` 中的 **不同** 整数组成的列表：只需要遍历set1（存储nums1出现的元素集合），判断当前元素是否存在与set2（存储nums2出现的元素集合），不存在则加入值answer[0]数组中。

**C++代码：**

```cpp
class Solution {
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) {
        set<int> st1,st2;
        int n=nums1.size();int m=nums2.size();
        for(int i=0;i<n;i++){
            st1.insert(nums1[i]);
        }
        for(int i=0;i<m;i++){
            st2.insert(nums2[i]);
        }
         vector<vector<int> > ans(2, vector<int>());
        for(auto x:st1){
            if(st2.count(x)) continue;
            ans[0].push_back(x);
        }
        for(auto x:st2){
            if(st1.count(x)) continue;
            ans[1].push_back(x);
        }
        return ans;
    }
};
```

### [二、美化数组的最少删除数](https://leetcode-cn.com/problems/minimum-deletions-to-make-array-beautiful/)

给你一个下标从 0 开始的整数数组 nums ，如果满足下述条件，则认为数组 nums 是一个 美丽数组 ：

nums.length 为偶数
对所有满足 i % 2 == 0 的下标 i ，nums[i] != nums[i + 1] 均成立
注意，空数组同样认为是美丽数组。

你可以从 nums 中删除任意数量的元素。当你删除一个元素时，被删除元素右侧的所有元素将会向左移动一个单位以填补空缺，而左侧的元素将会保持 不变 。

返回使 nums 变为美丽数组所需删除的 最少 元素数目。

**示例1：**

```
输入：nums = [1,1,2,3,5]
输出：1
解释：可以删除 nums[0] 或 nums[1] ，这样得到的 nums = [1,2,3,5] 是一个美丽数组。可以证明，要想使 nums 变为美丽数组，至少需要删除 1 个元素。
```

**示例2：**

```
输入：nums = [1,1,2,2,3,3]
输出：2
解释：可以删除 nums[0] 和 nums[5] ，这样得到的 nums = [1,2,2,3] 是一个美丽数组。可以证明，要想使 nums 变为美丽数组，至少需要删除 2 个元素。
```

**提示：**

- $1 <= nums.length <= 10^5$
- $0 <= nums[i] <= 10^5$

#### 题解：贪心

当出现`nums[i]==nums[i+1]`且`i%2==0`时，无论删除nums[i]还是nums[i+1]其实结果一样。

- 删除nums[i],num[i+1]前移，此时`(i+1)%2==0`需要和nums[i+2]比较
- 删除nums[i+1],`i%2==0`，同样需要和nums[i+2]比较

于是我们只需要遍历数组，对于每个`nums[i]==nums[i+1]`且`i%2==0`的情况计数加一，同时更新后面元素的下标即可。可以通过一个变量cnt记录需要删除元素的个数，同样对于随后的下标，只需要更新至`i-cnt`即可。

当然还需要判断最终的数组元素是否为偶数个。

**C++代码：**

```cpp
class Solution {
public:
    int minDeletion(vector<int>& nums) {
        int n=nums.size();
        int cnt=0;
        for(int i=0;i<n;i++){
            int p=i-cnt;
            if(i+1==n) break;
            if(nums[i]==nums[i+1]&&p%2==0){
                ++cnt;
            }
        }
        if((n-cnt)%2) ++cnt;
        return cnt;
    }
};
```

### [三、找到指定长度的回文数](https://leetcode-cn.com/problems/find-palindrome-with-fixed-length/)

给你一个整数数组 queries 和一个 正 整数 intLength ，请你返回一个数组 answer ，其中 answer[i] 是长度为 intLength 的 正回文数 中第 queries[i] 小的数字，如果不存在这样的回文数，则为 -1 。

回文数 指的是从前往后和从后往前读一模一样的数字。回文数不能有前导 0 。

**示例1：**

```
输入：queries = [1,2,3,4,5,90], intLength = 3
输出：[101,111,121,131,141,999]
解释：
长度为 3 的最小回文数依次是：
101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 201, ...
第 90 个长度为 3 的回文数是 999 。
```

**示例2：**

```
输入：queries = [2,4,6], intLength = 4
输出：[1111,1331,1551]
解释：
长度为 4 的前 6 个回文数是：
1001, 1111, 1221, 1331, 1441 和 1551 。
```

**提示：**

- $1 <= queries.length <= 5 * 10^4$
- $1 <= queries[i] <= 10^9$
- $1 <= intLength <= 15$

#### 题解：数学

通过题意发现，最终结果不会超long long，于是我们可以直接通过数字计算。

将`intLength`除2（`intLength`为奇数，则需要加1），由于是求回文数，于是我们只处理前一半数字即可。

对于样例1：

- intLength=3，则len=intLength/2+1=2，用某一变量x存储后一半需要添加的数字，
- 于是我们只需要考虑前两位数字，最小为10，此时x为1
- 当我们需要计算此时第k小的回文数时，我们只需要对前面元素+(k-1)即可，例如求第3小的元素，此时前一半为10+2=12，后一半x为1，组合为121

对于样例2：

- intLength=4，则len=intLength/2=2，用某一变量x存储后一半需要添加的数组
- 于是我们只考虑前两位数字，最小为10，此时x为1，
- 当我们需要计算此时第k小的回文数时，我们只需要对前面元素+(k-1)即可，例如求第3小的元素，此时前一半为10+2=12，后一半x为21，组合为1221

当然不要忘记考虑-1的情况。

具体见代码：

**C++代码：**

```cpp
class Solution {
public:
    typedef long long ll;
    //得到后一半数字
    ll get_num(int first_past,int flag){
        //如果是奇数为，则需要只需考虑前一半的n-1位（n为前一半数字的位数）
        if(flag) first_past/=10;
        ll res=0;
        while(first_past){
            res=res*10+first_past%10;
            first_past/=10;
        }
        return res;
    }
    vector<long long> kthPalindrome(vector<int>& queries, int intLength) {
        vector<ll> ans;
        int n=queries.size();
        int m=intLength/2;
        int flag=intLength%2?1:0;
        if(flag) ++m;
        for(int i=0;i<n;i++){
            ll cnt=queries[i];
            //前一半数字
            ll first_past=pow(10,m-1)+cnt-1;
            // cout<<first_past<<"---"<<pow(10,m)<<endl;
            //如果前一半的数字都超过m位，则返回-1
            if(first_past>=pow(10,m)){
                ans.push_back(-1);continue;
            }
            //后一半数字
            ll last_past=get_num(first_past,flag);
            //当前第k小的数字
            ll num=first_past*pow(10,m);
            if(flag) num/=10;
            num+=last_past;
            ans.push_back(num);
        }
        return ans;
    }
};
```

### [四、从栈中取出 K 个硬币的最大面值和](https://leetcode-cn.com/problems/maximum-value-of-k-coins-from-piles/)

一张桌子上总共有 n 个硬币 栈 。每个栈有 正整数 个带面值的硬币。

每一次操作中，你可以从任意一个栈的 顶部 取出 1 个硬币，从栈中移除它，并放入你的钱包里。

给你一个列表 piles ，其中 piles[i] 是一个整数数组，分别表示第 i 个栈里 从顶到底 的硬币面值。同时给你一个正整数 k ，请你返回在 恰好 进行 k 次操作的前提下，你钱包里硬币面值之和 最大为多少 。

**示例1：**

![img](https://assets.leetcode.com/uploads/2019/11/09/e1.png)

```
输入：piles = [[1,100,3],[7,8,9]], k = 2
输出：101
解释：
上图展示了几种选择 k 个硬币的不同方法。
我们可以得到的最大面值为 101 。
```

**示例2：**

```
输入：piles = [[100],[100],[100],[100],[100],[100],[1,1,1,1,1,1,700]], k = 7
输出：706
解释：
如果我们所有硬币都从最后一个栈中取，可以得到最大面值和。
```

**提示：**

- $n == piles.length$
- $1 <= n <= 1000$
- $1 <= piles[i][j] <= 10^5$
- $1 <= k <= sum(piles[i].length) <= 2000$

#### 题解：分组背包dp

![qDY76I.png](https://s1.ax1x.com/2022/03/28/qDY76I.png)

**C++代码：**

```cpp
class Solution {
public:
    int inf=0x3f3f3f3f;
    int maxValueOfCoins(vector<vector<int>>& piles, int k) {
        int n=piles.size();
        int dp[2005][2005];
        memset(dp,-inf,sizeof(dp));
        dp[0][0]=0;
        for(int i=1;i<=n;i++){
            vector<int> sum;sum.push_back(0);
            //前缀和
            for(int x:piles[i-1]) sum.push_back(sum.back()+x);
            for(int j=0;j<=k;j++){
                for(int p=0;p<sum.size();p++){
                    if(j>=p) dp[i][j]=max(dp[i][j],dp[i-1][j-p]+sum[p]);
                }
            }
        }
        return dp[n][k];
    }
};
```


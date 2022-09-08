---
title: LeetCode第267场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第267场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-267/

### 一、买票需要的时间

#### 题意

有 n 个人前来排队买票，其中第 0 人站在队伍 最前方 ，第 (n - 1) 人站在队伍 最后方 。

给你一个下标从 0 开始的整数数组 tickets ，数组长度为 n ，其中第 i 人想要购买的票数为 tickets[i] 。

每个人买票都需要用掉 恰好 1 秒 。一个人 一次只能买一张票 ，如果需要购买更多票，他必须走到  队尾 重新排队（瞬间 发生，不计时间）。如果一个人没有剩下需要买的票，那他将会 离开 队伍。

返回位于位置 k（下标从 0 开始）的人完成买票需要的时间（以秒为单位）。

**提示：**

- `n == tickets.length`
- `1 <= n <= 100`
- `1 <= tickets[i] <= 100`
- `0 <= k < n`

#### 示例

**示例1：**

> 输入：tickets = [2,3,2], k = 2
> 输出：6
> 解释： 
>
> - 第一轮，队伍中的每个人都买到一张票，队伍变为 [1, 2, 1] 。
>
> - 第二轮，队伍中的每个都又都买到一张票，队伍变为 [0, 1, 0] 。
>
>   位置 2 的人成功买到 2 张票，用掉 3 + 3 = 6 秒。
>

**示例2：**

> 输入：tickets = [5,1,1,1], k = 0
> 输出：8
> 解释：
>
> - 第一轮，队伍中的每个人都买到一张票，队伍变为 [4, 0, 0, 0] 。
> - 接下来的 4 轮，只有位置 0 的人在买票。
> 位置 0 的人成功买到 5 张票，用掉 4 + 1 + 1 + 1 + 1 = 8 秒。
>

#### 题解

数据量小，直接暴力即可。

```cpp
class Solution {
public:
    int timeRequiredToBuy(vector<int>& tickets, int k) {
        int n=tickets.size();
        int ans=0;
        for(int i=0;i<n;i++){
            if(i==k){
                tickets[k]--;ans++;
                if(tickets[k]==0) break;
            }else{
                if(tickets[i]>0) tickets[i]--,ans++;
            }
            if(i==n-1) i=-1;
        }
        return ans;
    }
};
```

### 二、反转偶数长度组的节点

#### 题意

给你一个链表的头节点 head 。

链表中的节点 按顺序 划分成若干 非空 组，这些非空组的长度构成一个自然数序列（1, 2, 3, 4, ...）。一个组的 长度 就是组中分配到的节点数目。换句话说：

节点 1 分配给第一组
节点 2 和 3 分配给第二组
节点 4、5 和 6 分配给第三组，以此类推
注意，最后一组的长度可能小于或者等于 1 + 倒数第二组的长度 。

反转 每个 偶数 长度组中的节点，并返回修改后链表的头节点 head 。

**提示**

- 链表中节点数目范围是 `[1, 105]`
- `0 <= Node.val <= 105`

#### 示例

**示例1：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/eg1.png)
>
> 输入：head = [5,2,6,3,9,1,7,3,8,4]
> 输出：[5,6,2,3,9,1,4,8,3,7]
> 解释：
> - 第一组长度为 1 ，奇数，没有发生反转。
> - 第二组长度为 2 ，偶数，节点反转。
> - 第三组长度为 3 ，奇数，没有发生反转。
> - 最后一组长度为 4 ，偶数，节点反转。
>

**示例2：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/eg2.png)
>
> 输入：head = [1,1,0,6]
> 输出：[1,0,1,6]
> 解释：
> - 第一组长度为 1 ，没有发生反转。
> - 第二组长度为 2 ，节点反转。
> - 最后一组长度为 1 ，没有发生反转。
>

**示例3：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/eg3.png)
>
> 输入：head = [2,1]
> 输出：[2,1]
> 解释：
> - 第一组长度为 1 ，没有发生反转。
> - 最后一组长度为 1 ，没有发生反转。

#### 题解

当然可以直接用链表翻转，但是这里偷了点懒，首先将链表存入数组，然后对数组进行处理。

<font color="#ff0000">**坑点：**</font>

例如链表$head=[2,1,3,4,2]$，应该输出为$[2,3,1,2,4]$，这里并不是偶数个子链表翻转，而是子链表长度为偶数个就翻转，所以对末尾的数据需要特判处理

于是代码如下：

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseEvenLengthGroups(ListNode* head) {
        vector<int> ve;
        ListNode* p=head;
        while(p!=nullptr){
            ve.push_back(p->val);
            p=p->next;
        }
        int len=1,i=0;
        int n=ve.size();
        ListNode* ans=new ListNode();
        p=ans;
        while(i+len<n){
            if(len&1){
                for(int j=0;j<len;j++){
                    p->next=new ListNode(ve[i++]);
                    p=p->next;
                }
            }else{
                for(int j=i+len-1;j>=i;j--){
                    p->next=new ListNode(ve[j]);
                    p=p->next;
                }
                i=i+len;
            }
            ++len;
        }
        //最后一组元素是偶数个还是奇数个
        if((n-i)&1){
            while(i<n){
                p->next=new ListNode(ve[i++]);
                p=p->next;
            }
        }else{
            for(int j=n-1;j>=i;j--){
                p->next=new ListNode(ve[j]);
                p=p->next;
            }
        }
        return ans->next;
    }
};
```

### 三、解码斜向换位密码

#### 题意

字符串 `originalText` 使用 **斜向换位密码** ，经由 **行数固定** 为 `rows` 的矩阵辅助，加密得到一个字符串 `encodedText` 。`originalText` 先按从左上到右下的方式放置到矩阵中。

![img](https://gitee.com/serendipity_LB/img/raw/master/exa11.png)

先填充蓝色单元格，接着是红色单元格，然后是黄色单元格，以此类推，直到到达 `originalText` 末尾。箭头指示顺序即为单元格填充顺序。所有空单元格用 `' '` 进行填充。矩阵的列数需满足：用 `originalText` 填充之后，最右侧列 **不为空** 。

接着按行将字符附加到矩阵中，构造 `encodedText` 。

![img](https://gitee.com/serendipity_LB/img/raw/master/exa12.png)

先把蓝色单元格中的字符附加到 `encodedText` 中，接着是红色单元格，最后是黄色单元格。箭头指示单元格访问顺序。

例如，如果 `originalText = "cipher"` 且 `rows = 3` ，那么我们可以按下述方法将其编码：

![img](https://gitee.com/serendipity_LB/img/raw/master/desc2.png)

蓝色箭头标识 `originalText` 是如何放入矩阵中的，红色箭头标识形成 `encodedText` 的顺序。在上述例子中，`encodedText = "ch  ie  pr"` 。

给你编码后的字符串 `encodedText` 和矩阵的行数 `rows` ，返回源字符串 `originalText` 。

**注意：**`originalText` **不** 含任何尾随空格 `' '` 。生成的测试用例满足 **仅存在一个** 可能的 `originalText` 。

**提示**

- `0 <= encodedText.length <= 106`
- `encodedText` 仅由小写英文字母和 `' '` 组成
- `encodedText` 是对某个 **不含** 尾随空格的 `originalText` 的一个有效编码
- `1 <= rows <= 1000`
- 生成的测试用例满足 **仅存在一个** 可能的 `originalText`

#### 示例

**示例1：**

> 输入：encodedText = "ch   ie   pr", rows = 3
> 输出："cipher"
> 解释：此示例与问题描述中的例子相同。

**示例2：**

>![img](https://gitee.com/serendipity_LB/img/raw/master/exam1.png)
>
>输入：encodedText = "iveo    eed   l te   olc", rows = 4
>输出："i love leetcode"
>解释：上图标识用于编码 originalText 的矩阵。 
>蓝色箭头展示如何从 encodedText 找到 originalText 。

**示例3：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/exam3.png)
>
> 输入：encodedText = " b  ac", rows = 2
> 输出：" abc"
> 解释：originalText 不能含尾随空格，但它可能会有一个或者多个前置空格。

#### 题解

直接按照题意模拟即可，将给定的`encodedText`放入矩阵中，斜向读取`originalText`即可。由于会末尾会产生多余的空格，所以最后需要处理一下。具体代码如下：

```cpp
class Solution {
public:
    string decodeCiphertext(string encodedText, int rows) {
        int n=encodedText.length();
        int cols=n/rows;
        string ans="";
        for(int i=0;i<cols;i++){
            for(int j=0;j<rows;j++){
                int p=j*(cols+1)+i;
                if(p>=n) continue;
                ans+=encodedText[p];
            }
        }
        string res=ans;
        for(int i=ans.length()-1;i>=0;i--){
            if(ans[i]!=' ') break;
            res.pop_back();
        }
        return res;
    }
};
```

### 四、[处理含限制条件的好友请求](https://leetcode-cn.com/problems/process-restricted-friend-requests/)

#### 题意

给你一个整数 n ，表示网络上的用户数目。每个用户按从 0 到 n - 1 进行编号。

给你一个下标从 0 开始的二维整数数组 restrictions ，其中 restrictions[i] = [xi, yi] 意味着用户 xi 和用户 yi 不能 成为 朋友 ，不管是 直接 还是通过其他用户 间接 。

最初，用户里没有人是其他用户的朋友。给你一个下标从 0 开始的二维整数数组 requests 表示好友请求的列表，其中 requests[j] = [uj, vj] 是用户 uj 和用户 vj 之间的一条好友请求。

如果 uj 和 vj 可以成为 朋友 ，那么好友请求将会 成功 。每个好友请求都会按列表中给出的顺序进行处理（即，requests[j] 会在 requests[j + 1] 前）。一旦请求成功，那么对所有未来的好友请求而言， uj 和 vj 将会 成为直接朋友 。

返回一个 布尔数组 result ，其中元素遵循此规则：如果第 j 个好友请求 成功 ，那么 result[j] 就是 true ；否则，为 false 。

注意：如果 uj 和 vj 已经是直接朋友，那么他们之间的请求将仍然 成功 。

**提示**

- `2 <= n <= 1000`
- `0 <= restrictions.length <= 1000`
- `restrictions[i].length == 2`
- `0 <= xi, yi <= n - 1`
- `xi != yi`
- `1 <= requests.length <= 1000`
- `requests[j].length == 2`
- `0 <= uj, vj <= n - 1`
- `uj != vj`

#### 示例

**示例1：**

> 输入：n = 3, restrictions = [[0,1]], requests = [[0,2],[2,1]]
> 输出：[true,false]
> 解释：
> 请求 0 ：用户 0 和 用户 2 可以成为朋友，所以他们成为直接朋友。 
> 请求 1 ：用户 2 和 用户 1 不能成为朋友，因为这会使 用户 0 和 用户 1 成为间接朋友 (1--2--0) 。

**示例2：**

> 输入：n = 3, restrictions = [[0,1]], requests = [[1,2],[0,2]]
> 输出：[true,false]
> 解释：
> 请求 0 ：用户 1 和 用户 2 可以成为朋友，所以他们成为直接朋友。 
> 请求 1 ：用户 0 和 用户 2 不能成为朋友，因为这会使 用户 0 和 用户 1 成为间接朋友 (0--2--1) 。
>

**示例3：**

> 输入：n = 5, restrictions = [[0,1],[1,2],[2,3]], requests = [[0,4],[1,2],[3,1],[3,4]]
> 输出：[true,false,true,false]
> 解释：
> 请求 0 ：用户 0 和 用户 4 可以成为朋友，所以他们成为直接朋友。 
> 请求 1 ：用户 1 和 用户 2 不能成为朋友，因为他们之间存在限制。
> 请求 2 ：用户 3 和 用户 1 可以成为朋友，所以他们成为直接朋友。 
> 请求 3 ：用户 3 和 用户 4 不能成为朋友，因为这会使 用户 0 和 用户 1 成为间接朋友 (0--4--3--1) 。
>

#### 题解

由题意可知，显然是一个**并查集**的题目。

由于数据量不大，可以这么处理：我们将能够直接或间接成为朋友的放入同一个集合。然后遍历`requests`数组，

- 如果本来就能直接或间接成为朋友，则直接为`true`
- 如果在原来集合中不能判断两者能否直接或间接成为朋友，则遍历`restrictions`数组。若存在相关限制，则为`false`；否则为`true`，同时将两者放入同一集合。

具体代码如下：

```cpp
class Solution {
public:
    int par[1010];
    int get_par(int a){
        if(a!=par[a]) par[a]=get_par(par[a]);
        return par[a];
    }
    vector<bool> friendRequests(int n, vector<vector<int>>& restrictions, vector<vector<int>>& requests) {
        vector<bool> ans;
        for(int i=0;i<n;i++) par[i]=i;
        for(int i=0;i<requests.size();i++){
            int par_u=get_par(requests[i][0]);int par_v=get_par(requests[i][1]);
            if(par_u==par_v){
                ans.push_back(1);continue;
            }else{
                bool flag=1;
                for(int j=0;j<restrictions.size();j++){
                    int u=restrictions[j][0];int v=restrictions[j][1];
                    if((get_par(u)==par_u&&get_par(v)==par_v) || (get_par(v)==par_u&&get_par(u)==par_v)){
                        flag=0;break;
                    }
                }
                ans.push_back(flag);
                if(flag) par[par_u]=par_v;
            }
        }
        return ans;
    }
};
```



### 总结：

最后一题真的不是很难，应该看下题目的...
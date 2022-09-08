---
title: LeetCode第283场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---

## LeetCode第283场周赛题解

> 比赛地址：https://leetcode-cn.com/contest/weekly-contest-283/

### [一、Excel 表中某个范围内的单元格](https://leetcode-cn.com/problems/cells-in-a-range-on-an-excel-sheet/)

#### 题意

Excel 表中的一个单元格 (r, c) 会以字符串 "<col><row>" 的形式进行表示，其中：

<col> 即单元格的列号 c 。用英文字母表中的 字母 标识。
例如，第 1 列用 'A' 表示，第 2 列用 'B' 表示，第 3 列用 'C' 表示，以此类推。
<row> 即单元格的行号 r 。第 r 行就用 整数 r 标识。
给你一个格式为 "<col1><row1>:<col2><row2>" 的字符串 s ，其中 <col1> 表示 c1 列，<row1> 表示 r1 行，<col2> 表示 c2 列，<row2> 表示 r2 行，并满足 r1 <= r2 且 c1 <= c2 。

找出所有满足 r1 <= x <= r2 且 c1 <= y <= c2 的单元格，并以列表形式返回。单元格应该按前面描述的格式用 字符串 表示，并以 非递减 顺序排列（先按列排，再按行排）。

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/ex1drawio.png)

```
输入：s = "K1:L2"
输出：["K1","K2","L1","L2"]
解释：
上图显示了列表中应该出现的单元格。
红色箭头指示单元格的出现顺序。
```

**示例2：**

![img](https://gitee.com/serendipity_LB/img/raw/master/exam2drawio.png)

```
输入：s = "A1:F1"
输出：["A1","B1","C1","D1","E1","F1"]
解释：
上图显示了列表中应该出现的单元格。 
红色箭头指示单元格的出现顺序。
```

**提示：**

- s.length == 5
- 'A' <= s[0] <= s[3] <= 'Z'
- '1' <= s[1] <= s[4] <= '9'
- s 由大写英文字母、数字、和 ':' 组成

#### 题解：模拟

直接按照题意模拟即可

```cpp
class Solution {
public:
    vector<string> cellsInRange(string s) {
        int l=s[0]-'A',r=s[3]-'A';
        int u=s[1]-'0',d=s[4]-'0';
        string str;
        vector<string> ans;
        for(int i=l;i<=r;i++){
            for(int j=u;j<=d;j++){
                str="";
                str+=i+'A';str+=j+'0';
                ans.push_back(str);
            }
        }
        return ans;
    }
};
```

### [二、向数组中追加 K 个整数](https://leetcode-cn.com/problems/append-k-integers-with-minimal-sum/)

#### 题意

给你一个整数数组 nums 和一个整数 k 。请你向 nums 中追加 k 个 未 出现在 nums 中的、互不相同 的 正 整数，并使结果数组的元素和 最小 。

返回追加到 nums 中的 k 个整数之和。

**示例1：**

```
输入：nums = [1,4,25,10,25], k = 2
输出：5
解释：在该解法中，向数组中追加的两个互不相同且未出现的正整数是 2 和 3 。
nums 最终元素和为 1 + 4 + 25 + 10 + 25 + 2 + 3 = 70 ，这是所有情况中的最小值。
所以追加到数组中的两个整数之和是 2 + 3 = 5 ，所以返回 5 。
```

**示例2：**

```
输入：nums = [5,6], k = 6
输出：25
解释：在该解法中，向数组中追加的两个互不相同且未出现的正整数是 1 、2 、3 、4 、7 和 8 。
nums 最终元素和为 5 + 6 + 1 + 2 + 3 + 4 + 7 + 8 = 36 ，这是所有情况中的最小值。
所以追加到数组中的两个整数之和是 1 + 2 + 3 + 4 + 7 + 8 = 25 ，所以返回 25 。
```

**提示：**

- $1 <= nums.length <= 10^5$
- $1 <= nums[i], k <= 10^9$

#### 题解：思维

不妨将题意理解为：**(原来的数组元素中重复元素和始终k大的数的总和sum1+追加进去后的元素总和num)-原来的数组元素总和sum2**

- 假设原来数组元素均大于k，则只要追加1到k即可
- 如果数组中元素存在<=k的情况，则k需要动态的增加

举个例子。例如示例1中，只存在1小于2，于是k+1=3。我们换种角度，假设数组原来元素中存在小于k的元素，我们不视为在原来数组中，而是在新追加的元素集合中，也就是新追加的元素集合始终为1到k的和（k如上所述随着数组元素的情况而动态改变）。同样对于示例1来说：

我们可以对最终数组元素总和理解为：(1+2+3)+(4+25+10+25)。

对于新追加元素集合，直接使用等差数列求和公式即可：$num=\frac{n*(n+1)}{2}$。

但是需要注意如果原来数组总存在多个<=k的数，且彼此相同，例如：

```
nums=[1,1,2,3,7,8],k=2
```

对于上述例子，我们只处理一次1，另一个1还是视为原来的数组元素总和。也就是理解为：(1+2+3+4+5)+(1+7+8)。

于是需要用集合判重，这里采用map。

当然需要额外对数组排序。

具体见代码。

```cpp
class Solution {
public:
    long long minimalKSum(vector<int>& nums, int k) {
        int n=nums.size();
        long long m=(long long)k;
        map<int,int>mp;mp.clear();
        long long ans=0;
        long long sum=0;
        sort(nums.begin(),nums.end());
        for(int i=0;i<n;i++){
            if((long long)nums[i]<=m){
                if(mp[nums[i]]==0){
                    mp[nums[i]]=1;m++;
                }else{
                    ans+=nums[i];
                }
            }
            sum+=(long long)nums[i];
        }
        for(int i=0;i<n;i++){
            if(nums[i]>m) ans+=(long long)nums[i];
        }
        ans+=m*(m+1)/2;
        return ans-sum;
    }
};
```

### [三、根据描述创建二叉树](https://leetcode-cn.com/problems/create-binary-tree-from-descriptions/)

#### 题意

给你一个二维整数数组 descriptions ，其中 descriptions[i] = [parenti, childi, isLefti] 表示 parenti 是 childi 在 二叉树 中的 父节点，二叉树中各节点的值 互不相同 。此外：

如果 isLefti == 1 ，那么 childi 就是 parenti 的左子节点。
如果 isLefti == 0 ，那么 childi 就是 parenti 的右子节点。
请你根据 descriptions 的描述来构造二叉树并返回其 根节点 。

测试用例会保证可以构造出 有效 的二叉树。

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/example1drawio.png)

```
输入：descriptions = [[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]
输出：[50,20,80,15,17,19]
解释：根节点是值为 50 的节点，因为它没有父节点。
结果二叉树如上图所示。
```

**示例2：**

![img](https://gitee.com/serendipity_LB/img/raw/master/example2drawio.png)

```
输入：descriptions = [[1,2,1],[2,3,0],[3,4,1]]
输出：[1,2,null,null,3,4]
解释：根节点是值为 1 的节点，因为它没有父节点。 
结果二叉树如上图所示。 
```

**提示：**

- 1 <= descriptions.length <= 104
- descriptions[i].length == 3
- 1 <= parenti, childi <= 105
- 0 <= isLefti <= 1
- descriptions 所描述的二叉树是一棵有效二叉树

#### 题解

通过数组par[]保存节点是否有父节点。数组TreeNode a[]保存所有的树节点。

遍历descriptions数组，假设当前遍历的元素是x，判断a数组中是否存在x[0]和x[1]，如果不存在则加入a数组中，通过x[2]判断是左孩子或右孩子。

由于节点数最多为100000，于是遍历1到100000，判断是否存在父节点不存在的情况，若当前节点父节点不存在即为根节点。

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
    int par[100010];
    TreeNode* a[100010];
    TreeNode* createBinaryTree(vector<vector<int>>& descriptions) {
        for(int i=1;i<=100000;i++){
            a[i]=NULL;par[i]=0;
        }
        for(auto x:descriptions){
            TreeNode* far=a[x[0]]==NULL?a[x[0]]=new TreeNode(x[0]):a[x[0]];
            TreeNode* son=a[x[1]]==NULL?a[x[1]]=new TreeNode(x[1]):a[x[1]];
            if(x[2]){
                far->left=son;
            }else{
                far->right=son;
            }
            par[x[1]]=1;
        }
        for(int i=1;i<=100000;i++){
            if(a[i]!=NULL&&par[i]==0){
                return a[i];
            }
        }
        return NULL;
    }
};
```


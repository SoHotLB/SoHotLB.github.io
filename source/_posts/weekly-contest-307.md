---
title: LeetCode第307场周赛
categories: 'LeetCode周赛题解'
tags: 
  - LeetCode
  - Algorithm
katex: true
---
## LeetCode第307场周赛题解

> 比赛地址：https://leetcode.cn/contest/weekly-contest-307/

<!-- more -->

### 一、[赢得比赛需要的最少训练时长](https://leetcode.cn/problems/minimum-hours-of-training-to-win-a-competition/)

#### 题意

你正在参加一场比赛，给你两个 正 整数 initialEnergy 和 initialExperience 分别表示你的初始精力和初始经验。

另给你两个下标从 0 开始的整数数组 energy 和 experience，长度均为 n 。

你将会 依次 对上 n 个对手。第 i 个对手的精力和经验分别用 energy[i] 和 experience[i] 表示。当你对上对手时，需要在经验和精力上都 严格 超过对手才能击败他们，然后在可能的情况下继续对上下一个对手。

击败第 i 个对手会使你的经验 增加 experience[i]，但会将你的精力 减少  energy[i] 。

在开始比赛前，你可以训练几个小时。每训练一个小时，你可以选择将增加经验增加 1 或者 将精力增加 1 。

返回击败全部 n 个对手需要训练的 最少 小时数目。

**示例1：**

```md
输入：initialEnergy = 5, initialExperience = 3, energy = [1,4,3,2], experience = [2,6,3,1]
输出：8
解释：在 6 小时训练后，你可以将精力提高到 11 ，并且再训练 2 个小时将经验提高到 5 。
按以下顺序与对手比赛：
- 你的精力与经验都超过第 0 个对手，所以获胜。
  精力变为：11 - 1 = 10 ，经验变为：5 + 2 = 7 。
- 你的精力与经验都超过第 1 个对手，所以获胜。
  精力变为：10 - 4 = 6 ，经验变为：7 + 6 = 13 。
- 你的精力与经验都超过第 2 个对手，所以获胜。
  精力变为：6 - 3 = 3 ，经验变为：13 + 3 = 16 。
- 你的精力与经验都超过第 3 个对手，所以获胜。
  精力变为：3 - 2 = 1 ，经验变为：16 + 1 = 17 。
在比赛前进行了 8 小时训练，所以返回 8 。
可以证明不存在更小的答案。
```

**示例2：**

```md
输入：initialEnergy = 2, initialExperience = 4, energy = [1], experience = [3]
输出：0
解释：你不需要额外的精力和经验就可以赢得比赛，所以返回 0 。
```

**提示：**

- $n == energy.length == experience.length$
- $1 <= n <= 100$
- $1 <= initialEnergy, initialExperience, energy[i], experience[i] <= 100$

#### 题解：模拟

由于需要依次打败n个对手，且每次打败的条件为：经验和精力均大于对手。于是遍历n个对手，如果当前精力或经验小于对手时则不断训练以增加经验和精力。

**C++代码：**

```cpp
class Solution {
public:
    int minNumberOfHours(int num1, int num2, vector<int>& a, vector<int>& b) {
        int n=a.size();
        int ans=0;
        for(int i=0;i<n;i++){
            while(num1<=a[i]||num2<=b[i]){
                ++ans;
                if(num1<=a[i]) ++num1;
                else if(num2<=b[i]) ++num2;
            }
            num1-=a[i];num2+=b[i];
        }
        return ans;
    }
};
```

### 二、[6166. 最大回文数字](https://leetcode.cn/problems/largest-palindromic-number/)

#### 题意

给你一个仅由数字（0 - 9）组成的字符串 num 。

请你找出能够使用 num 中数字形成的 最大回文 整数，并以字符串形式返回。该整数不含 前导零 。

注意：

- 你 无需 使用 num 中的所有数字，但你必须使用 至少 一个数字。
- 数字可以重新排序。

**示例1：**

```
输入：num = "444947137"
输出："7449447"
解释：
从 "444947137" 中选用数字 "4449477"，可以形成回文整数 "7449447" 。
可以证明 "7449447" 是能够形成的最大回文整数。
```

**示例2：**

```
输入：num = "00009"
输出："9"
解释：
可以证明 "9" 能够形成的最大回文整数。
注意返回的整数不应含前导零。
```

**提示**

- `1 <= num.length <= 10^5`
- `num` 由数字（`0 - 9`）组成

#### 题解：

首先用哈希表存储每个数字对应的个数，然后依次从大到小构造回文串的前一半，具体为：

- 假设当前遍历的数字为i，对应的个数为cnt[i]，则回文串前一半有$cnt[i]/2$个数字i
- 当遍历完所有数字后，如果还有数字空余，则选一个最大的数字作为中间的数字

当前上述过程需要排除到前导0的存在。所有对于只存在0的字符串，需要特判下。

**C++代码：**

```cpp
class Solution {
public:
    string largestPalindromic(string num) {
        int cnt[10];
        memset(cnt,0,sizeof(cnt));
        int n=num.length();
        for(int i=0;i<n;i++){
            cnt[num[i]-'0']++;
        }
        //判断是否为前导0
        int flag=true;
        string ans="";
        for(int i=9;i>=0;i--){
            if(i==0 && flag) break;
            if(cnt[i]/2){
                string str(cnt[i]/2,(i+'0'));
                ans+=str;
                if(flag) flag=false;
            }
          
            cnt[i]-=(cnt[i]/2)*2;
        }
        string s=ans;
        if(s.length()>0) reverse(s.begin(),s.end());
        for(int i=9;i>=0;i--){
            if(i==0 && flag) break;
            if(cnt[i]>0){
                ans+=(i+'0');break;
            }
        }
        ans+=s;
        if(ans.length()==0 && cnt[0]) ans+='0';
        return ans;
    }
};
```

### 三、[感染二叉树需要的总时间](https://leetcode.cn/problems/amount-of-time-for-binary-tree-to-be-infected/)

#### 题意

给你一棵二叉树的根节点 root ，二叉树中节点的值 互不相同 。另给你一个整数 start 。在第 0 分钟，感染 将会从值为 start 的节点开始爆发。

每分钟，如果节点满足以下全部条件，就会被感染：

- 节点此前还没有感染。
- 节点与一个已感染节点相邻。
- 返回感染整棵树需要的分钟数。

**示例1：**

![image-20220625231744-1-2022-09-11-19-51-26](http://lbbuket-blog.oss-cn-hangzhou.aliyuncs.com/blog-img/image-20220625231744-1-2022-09-11-19-51-26.png/resize80)

```
输入：root = [1,5,3,null,4,10,6,9,2], start = 3
输出：4
解释：节点按以下过程被感染：
- 第 0 分钟：节点 3
- 第 1 分钟：节点 1、10、6
- 第 2 分钟：节点5
- 第 3 分钟：节点 4
- 第 4 分钟：节点 9 和 2
感染整棵树需要 4 分钟，所以返回 4 。
```

**示例2：**

```
输入：root = [1], start = 1
输出：0
解释：第 0 分钟，树中唯一一个节点处于感染状态，返回 0 。
```

**提示：**

- 树中节点的数目在范围 `[1, 10^5]` 内
- `1 <= Node.val <= 10^5`
- 每个节点的值 **互不相同**
- 树中必定存在值为 `start` 的节点

#### 题解一：DFS

关于起始节点只有两种情况存在：

- 第一种情况：start节点为整棵二叉树根节点，于是感染时间即为该二叉树的高度
- 第二种情况：start节点为二叉树某一子节点，又分为左子树节点和右子树节点
  - 左子树节点：则感染时间为： 以start为根节点的树的高度 和 整棵树的右子树的最大高度+root节点到start节点的距离 两者取最大值
  - 右子树节点：感染时间为： 以start为根节点的树的高度 和 整棵树的左子树的最大高度+root节点到start节点的距离 两者取最大值

具体见代码。

**C++代码：**

```cpp
class Solution {
public:
    int ans=0; //最终结果
    int depth=-1;  //起始节点的高度
    int amountOfTime(TreeNode* root, int start) {
        dfs(root, 0, start);
        return ans;
    }
    int dfs(TreeNode* root,int level,int start){
        if(root==nullptr) return 0;
        if(root->val==start) depth=level;
        int l=dfs(root->left,level+1,start);      //左子树的高度
        bool inLeft= depth!=-1;       //判断起始节点是否在左子树上
        int r=dfs(root->right,level+1,start);     //右子树的高度
        if(root->val == start) ans=max(ans,max(l,r));   //第一种情况：感染start为根节点的树所需的时间
        if(inLeft){                                     //第二种情况：感染root为根节点的树所需的时间
            ans=max(ans,depth-level+r);
        }else{
            ans=max(ans,depth-level+l);
        }
        return max(l,r)+1;              //返回树高
    }
};
```

#### 题解二：建图+遍历

**当然还可以建图（将二叉树转化为图），然后直接从start节点遍历整张图，求最长路径（dfs和bfs都可以）即可**

**C++代码**

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
    vector<int> edge[100005];
    void build_edge(TreeNode* root){
        if(root==nullptr) return ;
        int u=root->val,v;
        if(root->left!=nullptr){
            v=root->left->val;
            edge[u].push_back(v);
            edge[v].push_back(u);
        }
        if(root->right!=nullptr){
            v=root->right->val;
            edge[v].push_back(u);
            edge[u].push_back(v);
        }
        build_edge(root->left);
        build_edge(root->right);
    }
    int vis[100005];
    int amountOfTime(TreeNode* root, int start) {
        memset(edge,0,sizeof(edge));
        build_edge(root);

        memset(vis,0,sizeof(vis));
        queue<pair<int,int>>q;
        int ans=0;
        vis[start]=1;q.push(make_pair(start,0));
        while(!q.empty()){
            pair<int,int> now=q.front();q.pop();
            int u=now.first;
            ans=max(ans,now.second);
            for(int i=0;i<edge[u].size();i++){
                int v=edge[u][i];
                if(vis[v]) continue;
                q.push(make_pair(v,now.second+1));
                vis[v]=1;
            }
        }
        return ans;
    }   
};
```

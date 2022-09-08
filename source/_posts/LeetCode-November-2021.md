---
title: LeetCode2021年每日一题11月打卡汇总
categories: 'LeetCode每日一题打卡'
tags: 

  - LeetCode
  - Algorithm
katex: true
---

## LeetCode2021年每日一题11月打卡汇总

### [11.15 灯泡开关](https://leetcode-cn.com/problems/bulb-switcher/)

#### 题意

初始时有 n 个灯泡处于关闭状态。第一轮，你将会打开所有灯泡。接下来的第二轮，你将会每两个灯泡关闭一个。

第三轮，你每三个灯泡就切换一个灯泡的开关（即，打开变关闭，关闭变打开）。第 i 轮，你每 i 个灯泡就切换一个灯泡的开关。直到第 n 轮，你只需要切换最后一个灯泡的开关。

找出并返回 n 轮后有多少个亮着的灯泡。

**提示：**

- $0 <= n <= 10^9$

#### 示例

**示例1：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/bulb.jpg)
>
> 输入：n = 3
> 输出：1 
> 解释：
> 初始时, 灯泡状态 [关闭, 关闭, 关闭].
> 第一轮后, 灯泡状态 [开启, 开启, 开启].
> 第二轮后, 灯泡状态 [开启, 关闭, 开启].
> 第三轮后, 灯泡状态 [开启, 关闭, 关闭]. 
>
> 你应该返回 1，因为只有一个灯泡还亮着。

**示例2：**

> 输入：n = 0
> 输出：0

**示例3：**

> 输入：n = 1
> 输出：1

#### 题解：规律

首先看数据范围n高达$10^9$，所以显然线性复杂度解决不了，于是尝试打表寻找规律，发现如下：

> 0:0
>
> 1:1
>
> 2:1
>
> 3:1
>
> 4:1
>
> 5:2
>
> ...
>
> 8:2
>
> ...

打表了n从1-100的情况，可以发现，最终灯泡亮的数目以1 3 5 7 9...（公差为2的等差数列增长），所以提前用一数组存储等差数列和在$10^9$以内的情况。

遍历数组，若当前遍历的数（即为等差数列和）大于或等于n时，则表示找到答案。于是代码如下：

**C++代码**

```cpp
class Solution {
public:
    int bulbSwitch(int n) {
        vector<int> ve;
        int num=0,x=3;
        while(num<1e9){
            ve.push_back(num);num+=x;x+=2;
        }
        // cout<<ve.size()<<endl;
        int ans=lower_bound(ve.begin(),ve.end(),n)-ve.begin();
        return ans;
    }
};
```

 **Java代码**

```java
class Solution {
    public int bulbSwitch(int n) {
        ArrayList<Integer> list=new ArrayList<Integer>();
        int num=0,x=3;
        while(num<(int)1e9){
            list.add(num);num+=x;x+=2;
        }
        int ans=0;
        for(int i=0;i<list.size();i++){
            if(list.get(i)>=n){
                ans=i;break;
            }
        }
        return ans;
    }
}
```

### [11.16 完美矩形](https://leetcode-cn.com/problems/perfect-rectangle/)

#### 题意

给你一个数组 rectangles ，其中 rectangles[i] = [xi, yi, ai, bi] 表示一个坐标轴平行的矩形。这个矩形的左下顶点是 (xi, yi) ，右上顶点是 (ai, bi) 。

如果所有矩形一起精确覆盖了某个矩形区域，则返回 true ；否则，返回 false 。

**提示：**

- $1 <= rectangles.length <= 2 * 10^4$
- $rectangles[i].length == 4$
- $10^5 <= xi, yi, ai, bi <= 10^5$

#### 示例

**示例1：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/perectrec1-plane.jpg)
>
> 输入：rectangles = [[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]
> 输出：true
> 解释：5 个矩形一起可以精确地覆盖一个矩形区域。 

**示例2：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/perfectrec2-plane.jpg)
>
> 输入：rectangles = [[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]
> 输出：false
> 解释：两个矩形之间有间隔，无法覆盖成一个矩形。
>

**示例3：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/perfectrec3-plane.jpg)
>
> 输入：rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[3,2,4,4]]
> 输出：false
> 解释：图形顶端留有空缺，无法覆盖成一个矩形。
>

**示例4：**

> ![img](https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg)
>
> 输入：rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]
> 输出：false
> 解释：因为中间有相交区域，虽然形成了矩形，但不是精确覆盖。

#### 题解：hash

1. 官方做法：https://leetcode-cn.com/problems/perfect-rectangle/solution/wan-mei-ju-xing-by-leetcode-solution-ty8q/

   精确覆盖意味着：

   矩形区域中不能有空缺，即矩形区域的面积等于所有矩形的面积之和；
   矩形区域中不能有相交区域。
   我们需要一个统计量来判定是否存在相交区域。由于精确覆盖意味着矩形的边和顶点会重合在一起，我们不妨统计每个矩形顶点的出现次数。同一个位置至多只能存在四个顶点，在满足该条件的前提下，如果矩形区域中有相交区域，这要么导致矩形区域四角的顶点出现不止一次，要么导致非四角的顶点存在出现一次或三次的顶点；

   因此要满足精确覆盖，除了要满足矩形区域的面积等于所有矩形的面积之和，还要满足矩形区域四角的顶点只能出现一次，且其余顶点的出现次数只能是两次或四次。

   在代码实现时，我们可以遍历矩形数组，计算矩形区域四个顶点的位置，以及矩形面积之和，并用哈希表统计每个矩形的顶点的出现次数。遍历完成后，检查矩形区域的面积是否等于所有矩形的面积之和，以及每个顶点的出现次数是否满足上述要求。


```cpp
class Solution {
    typedef pair<int,int> p;
    typedef long long ll;
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        ll s=0;
        int minx=rectangles[0][0],miny=rectangles[0][1],maxx=rectangles[0][2],maxy=rectangles[0][3];
        map<p,int>cnt;
        for(auto &rect:rectangles){
            int x=rect[0],y=rect[1],a=rect[2],b=rect[3];
            s+=(ll)(a-x)*(b-y);

            minx=min(minx,x);miny=min(miny,y);maxx=max(maxx,a);maxy=max(maxy,b);

            p p1({x,y});p p2({x,b});p p3({a,y});p p4({a,b});

            cnt[p1]++;cnt[p2]++;cnt[p3]++;cnt[p4]++;
        }
        p pmin_min({minx,miny});p pmin_max({minx,maxy});p pmax_min({maxx,miny});p pmax_max({maxx,maxy});
        if(s!=(ll)(maxx-minx)*(maxy-miny)|| !cnt.count(pmin_min) || !cnt.count(pmin_max) || !cnt.count(pmax_min) || !cnt.count(pmax_max)){
            return false;
        }
        cnt.erase(pmin_min);cnt.erase(pmin_max);cnt.erase(pmax_min);cnt.erase(pmax_max);

        for(auto &it:cnt){
            int val=it.second;
            if(val!=2&&val!=4){
                return false;
            }
        }
        return true;
    }
};
```

2.扫描线

### [11.17 最大单词长度乘积]()

#### 题意

给定一个字符串数组 words，找到 length(word[i]) * length(word[j]) 的最大值，并且这两个单词不含有公共字母。你可以认为每个单词只包含小写字母。如果不存在这样的两个单词，返回 0。

**提示：**

- `2 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` 仅包含小写字母

#### 示例

**示例1：**

> 输入: ["abcw","baz","foo","bar","xtfn","abcdef"]
> 输出: 16 
> 解释: 这两个单词为 "abcw", "xtfn"。
>

**示例2：**

> 输入: ["a","ab","abc","d","cd","bcd","abcd"]
> 输出: 4 
> 解释: 这两个单词为 "ab", "cd"。

**示例3：**

> 输入: ["a","aa","aaa","aaaa"]
> 输出: 0 
> 解释: 不存在这样的两个单词。

#### 题解1：暴力

首先看数据量，字符串长度和数组长度都在1000以内，于是可以考虑对每个字符串出现的字母数目进行存储。然后两重循环遍历每一对字符串，如果能够满足条件，则更新当前最大的长度乘积。时间复杂度$O(26n^2)$

具体代码如下：

**C++代码**

**注意：**

- **在LeetCode环境下C++数组需要初始化**
- 求string长度函数返回的是**无符号数**，如果需要与int型比较大小，需要强转。

```cpp
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int cnt[1010][26];
        memset(cnt,0,sizeof(cnt));
        int n=words.size();
        //每个字符串中每个字母的个数
        for(int i=0;i<n;i++){
            for(int j=0;j<words[i].length();j++){
                cnt[i][words[i][j]-'a']++;
            }
        }
        int ans=0;
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                bool flag=1;
                for(int k=0;k<26;k++){
                    if(cnt[i][k]&&cnt[j][k]){
                        flag=0;break;
                    }
                }
                if(flag) ans=max(ans,(int)words[i].length()*(int)words[j].length());
            }
        }
        return ans;
    }
};
```

 **Java代码**

```java
class Solution {
    public int maxProduct(String[] words) {
        int n=words.length;
        int cnt[][]=new int[n][26];
        int ans=0;
        //记录每个字符串中各个字母的个数
        for(int i=0;i<n;i++){
        	char[] s=words[i].toCharArray();
        	for(int j=0;j<s.length;j++){
        		cnt[i][s[j]-'a']++;
        	}
        }
        for(int i=0;i<n;i++){
        	for(int j=i+1;j<n;j++){
        		boolean flag=true;
        		for(int k=0;k<26;k++){
        			if(cnt[i][k]>0&&cnt[j][k]>0){
        				flag=false;break;
        			}
        		}
        		if(flag==true){
        			ans=Math.max(ans,words[i].length()*words[j].length());
        		}
        	}
        }
        return ans;
    }
}
```

#### 题解2：位运算

假设题目数组长度和字符串长度在10000以内，则上述方法失效，于是思考有没有能降到$O(n^2)$的算法。

题目已知字符串只含小写字母，最后26位，于是我们可以考虑用位运算，将一个字符串出现的 $a-z$ 26个字母用一二进制数表示，若某一位为1则表示该字符在这个字符串中出现，反之没出现。

于是我们可以将判断两个字符串是否含有公共字母的时间复杂度降低到$O(1)$。具体代码如下：

 **C++代码**

```cpp
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int n=words.size();
        int masks[1010];memset(masks,0,sizeof(masks));
        int ans=0;
        //记录每个字符串的位掩码
        for(int i=0;i<n;i++){
            for(int j=0;j<words[i].length();j++){
                masks[i]|=(1<<(words[i][j]-'a'));
            }
        }
        for(int i=0;i<n;i++){
            for(int j=i+1;j<n;j++){
                if((masks[i]&masks[j])==0){
                    ans=max(ans,(int)words[i].length()*(int)words[j].length());
                }
            }
        }
        return ans;
    }
};
```

**Java代码**

```java
class Solution {
    public int maxProduct(String[] words) {
        int n=words.length;
        int ans=0;
        int masks[]=new int[n];
        //计算每个字符串的位掩码
        for(int i=0;i<n;i++){
        	char s[]=words[i].toCharArray();
        	for(int j=0;j<s.length;j++){
        		masks[i]|=(1<<(s[j]-'a'));
        	}
        }
        for(int i=0;i<n;i++){
        	for(int j=i+1;j<n;j++){
        		if((masks[i]&masks[j])==0){
        			ans=Math.max(ans,words[i].length()*words[j].length());
        		}
        	}
        }
        return ans;
    }
}
```

### [11.18 二叉树的坡度](https://leetcode-cn.com/problems/binary-tree-tilt/)

#### 题意

给定一个二叉树，计算 整个树 的坡度 。

一个树的 节点的坡度 定义即为，该节点左子树的节点之和和右子树节点之和的 差的绝对值 。如果没有左子树的话，左子树的节点之和为 0 ；没有右子树的话也是一样。空结点的坡度是 0 。

整个树 的坡度就是其所有节点的坡度之和。

**提示：**

- 树中节点数目的范围在 $[0, 10^4]$ 内
- `-1000 <= Node.val <= 1000`

#### 示例

**示例1：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/tilt1.jpg)
>
> 输入：root = [1,2,3]
> 输出：1
> 解释：
> 节点 2 的坡度：|0-0| = 0（没有子节点）
> 节点 3 的坡度：|0-0| = 0（没有子节点）
> 节点 1 的坡度：|2-3| = 1（左子树就是左子节点，所以和是 2 ；右子树就是右子节点，所以和是 3 ）
> 坡度总和：0 + 0 + 1 = 1

**示例2：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/tilt2.jpg)
>
> 输入：root = [4,2,9,3,5,null,7]
> 输出：15
> 解释：
> 节点 3 的坡度：|0-0| = 0（没有子节点）
> 节点 5 的坡度：|0-0| = 0（没有子节点）
> 节点 7 的坡度：|0-0| = 0（没有子节点）
> 节点 2 的坡度：|3-5| = 2（左子树就是左子节点，所以和是 3 ；右子树就是右子节点，所以和是 5 ）
> 节点 9 的坡度：|0-7| = 7（没有左子树，所以和是 0 ；右子树正好是右子节点，所以和是 7 ）
> 节点 4 的坡度：|(3+5+2)-(9+7)| = |10-16| = 6（左子树值为 3、5 和 2 ，和是 10 ；右子树值为 9 和 7 ，和是 16 ）
> 坡度总和：0 + 0 + 0 + 2 + 7 + 6 = 15
>

**示例3：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/tilt3.jpg)
>
> 输入：root = [21,7,14,1,1,2,2,3,3]
> 输出：9

#### 题解：深搜

简单递归深搜。深度优先遍历整个二叉树，用一全局变量记录整个二叉树的坡度之和，每次深度优先遍历返回当前节点为根的所有节点之和。具体代码如下：

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
    int ans=0;
    int dfs(TreeNode* root){
        if(root==nullptr) return 0;
        int l_sum=dfs(root->left);
        int r_sum=dfs(root->right);
        ans+=abs(l_sum-r_sum);
        return l_sum+r_sum+root->val;
    }
    int findTilt(TreeNode* root) {
        dfs(root);
        return ans;
    }
};
```

 **Java代码**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int ans=0;
    public int findTilt(TreeNode root) {
        dfs(root);
        return ans;
    }
    public int dfs(TreeNode node){
        if(node==null) return 0;
        int l_sum=dfs(node.left);
        int r_sum=dfs(node.right);
        ans=ans+Math.abs(l_sum-r_sum);
        return l_sum+r_sum+node.val;
    }
}
```

### [11.19 整数替换](https://leetcode-cn.com/problems/integer-replacement/)

#### 题意

给定一个正整数 n ，你可以做如下操作：

如果 n 是偶数，则用 n / 2替换 n 。
如果 n 是奇数，则可以用 n + 1或n - 1替换 n 。
n 变为 1 所需的最小替换次数是多少？

**提示：**

- $1 <= n <= 2^31 - 1$

#### 示例

**示例1：**

> 输入：n = 8
> 输出：3
> 解释：8 -> 4 -> 2 -> 1

**示例2：**

> 输入：n = 7
> 输出：4
> 解释：7 -> 8 -> 4 -> 2 -> 1
> 或 7 -> 6 -> 3 -> 2 -> 1

**示例3：**

> 输入：n = 4
> 输出：2

#### 题解1：枚举（递归）

假设当前需要处理的数为x

- 如果x为偶数，则直接x/2，操作次数+1
- 如果x为奇数，则从$\frac{x+1}{2}$ 和$\frac{x-1}{2}$中选择需要操作次数少的那一个。(这里操作变成了两步，分别是+1或者-1 再加上除2操作)

需要注意的是：

如果n=$2^{31}-1$，则可能会越界，一种解决方法是直接将n转为长整型，另一种则是使用向下取整代替，$\lfloor \frac{n}{2} \rfloor+1$ 和$\lfloor \frac{n}{2} \rfloor$分别表示 $\frac{n+1}{2}$ 和 $\frac{n-1}{2}$。

具体代码如下：

```cpp
class Solution {
public:
    int integerReplacement(int n) {
        if(n==1) return 0;
        if(n%2==0){
            return integerReplacement(n/2)+1;
        }else{
            return min(integerReplacement(n/2),integerReplacement(n/2+1))+2;
        }
    }
};
```

#### 题解2：记忆化搜索

思路和递归思路相近，不过引入map存储以及处理过的数（不能用数组，因为数太大，导致数组空间太大，而在操作过程中又有很多数不会出现导致浪费空间，这时键值对便是不二之选），具体见代码：

**C++代码：**

```cpp
class Solution {
public:
    map<int,int>mp;
    int integerReplacement(int n) {
        if(n==1) return 0;
        if(mp.count(n)) return mp[n];
        if(n%2==0){
            mp[n]=integerReplacement(n/2)+1;
        }else{
            mp[n]=min(integerReplacement(n/2),integerReplacement(n/2+1))+2;
        }
        return mp[n];
    }
};
```

**Java代码：**

```java
class Solution {
    HashMap<Integer,Integer> mp=new HashMap<Integer,Integer>();
    public int integerReplacement(int n) {
        if(n==1) return 0;
        if(mp.containsKey(n)) return mp.get(n);
        if(n%2==0){
        	mp.put(n,integerReplacement(n/2)+1);
        }else{
        	mp.put(n, Math.min(integerReplacement(n/2), integerReplacement(n/2+1))+2);
        }
        return mp.get(n);
    }
}
```

#### 题解3：贪心

> 这里借鉴LeetCode官方题解：https://leetcode-cn.com/problems/integer-replacement/solution/zheng-shu-ti-huan-by-leetcode-solution-swef/

```cpp
class Solution {
public:
    int integerReplacement(int n) {
        int ans=0;
        while(n!=1){
            if(n%2==0) ans++,n/=2;
            else if(n%4==1) ans+=2,n/=2;
            else{
                if(n==3) ans+=2,n=1;
                else ans+=2,n=n/2+1;
            }
        }
        return ans;
    }
};
```

### [11.20 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

#### 题意

和谐数组是指一个数组里元素的最大值和最小值之间的差别 正好是 1 。

现在，给你一个整数数组 nums ，请你在所有可能的子序列中找到最长的和谐子序列的长度。

数组的子序列是一个由数组派生出来的序列，它可以通过删除一些元素或不删除元素、且不改变其余元素的顺序而得到。

**提示：**

- $1 <= nums.length <= 2 * 10^4$
- $-10^9 <= nums[i] <= 10^9$

#### 示例

**示例1：**

> 输入：nums = [1,3,2,2,5,2,3,7]
> 输出：5
> 解释：最长的和谐子序列是 [3,2,2,2,3]

**示例2：**

> 输入：nums = [1,2,3,4]
> 输出：2

**示例3：**

> 输入：nums = [1,1,1,1]
> 输出：0

#### 题解：hash表

由于题目只是求满足要求的最长子序列。于是可以先对每个数在数组中出现的次数用hash表存储，然后再遍历hash表每个数，设当前遍历的数x是子序列中小的那个数，如果x+1在hash表中存在，则更新一下当前最大值：$ans=max(ans,mp[x+1]+mp[x])$，具体代码如下：

**C++代码：**

```cpp
class Solution {
public:
    int findLHS(vector<int>& nums) {
        map<int,int>mp;
        for(int i=0;i<nums.size();i++){
            mp[nums[i]]++;
        }
        int ans=0;
        for(auto &it:mp){
            if(mp.count(it.first+1)){
                ans=max(ans,it.second+mp[it.first+1]);
            }
        }
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
    public int findLHS(int[] nums) {
        HashMap<Integer,Integer>mp=new HashMap<Integer,Integer>();
        for(int x:nums){
        	if(mp.containsKey(x)) mp.put(x, mp.get(x)+1);
        	else mp.put(x, 1);
        }
        int ans=0;
        for(int x:mp.keySet()){
        	if(mp.containsKey(x+1)){
        		ans=Math.max(ans, mp.get(x)+mp.get(x+1));
        	}
        }
        return ans;
    }
}
```

### [11.21 N 叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

#### 题意

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

N 叉树输入按层序遍历序列化表示，每组子节点由空值分隔（请参见示例）。

**提示：**

- 树的深度不会超过 `1000` 。
- 树的节点数目位于 $[0, 10^4]$ 之间。

#### 示例

**示例1：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/narytreeexample.png)
>
> 输入：root = [1,null,3,2,4,null,5,6]
> 输出：3

**示例2：**

> ![img](https://gitee.com/serendipity_LB/img/raw/master/sample_4_964.png)
>
> 输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
> 输出：5

#### 题解：递归（dfs）

**需要注意的是，可能会出现空树，所以需要特判下**，其他就简单了，直接递归即可

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
    int maxDepth(Node* root) {
        if(root==nullptr) return 0;
        if(root->children.size()==0) return 1;
        int ans=0;
        for(auto &child:root->children){
            ans=max(ans,maxDepth(child)+1);
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
    public int maxDepth(Node root) {
        if(root==null) return 0;
        if(root.children.size()==0) return 1;
        int ans=0;
        for(Node node:root.children){
            ans=Math.max(ans,maxDepth(node)+1);
        }
        return ans;
    }
}
```


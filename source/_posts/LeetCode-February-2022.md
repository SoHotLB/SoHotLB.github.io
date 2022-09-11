---
title: LeetCode2022年每日一题2月打卡汇总
categories: 'LeetCode每日一题'
tags: 
  - LeetCode
  - Algorithm
katex: true
---
## LeetCode2022年每日一题2月打卡汇总

不能再鸽了...

### [2.11：学生分数的最小差值](https://leetcode-cn.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)

#### 题意

给你一个 下标从 0 开始 的整数数组 nums ，其中 nums[i] 表示第 i 名学生的分数。另给你一个整数 k 。

从数组中选出任意 k 名学生的分数，使这 k 个分数间 最高分 和 最低分 的 差值 达到 最小化 。

返回可能的 最小差值 。

**提示：**

- $1 <= k <= nums.length <= 1000$
- $0 <= nums[i] <= 10^5$

**示例1：**

> 输入：nums = [90], k = 1
> 输出：0
> 解释：选出 1 名学生的分数，仅有 1 种方法：
>
> - [90] 最高分和最低分之间的差值是 90 - 90 = 0
>   可能的最小差值是 0

**示例2：**

> 输入：nums = [9,4,1,7], k = 2
> 输出：2
> 解释：选出 2 名学生的分数，有 6 种方法：
>
> - [9,4,1,7] 最高分和最低分之间的差值是 9 - 4 = 5
> - [9,4,1,7] 最高分和最低分之间的差值是 9 - 1 = 8
> - [9,4,1,7] 最高分和最低分之间的差值是 9 - 7 = 2
> - [9,4,1,7] 最高分和最低分之间的差值是 4 - 1 = 3
> - [9,4,1,7] 最高分和最低分之间的差值是 7 - 4 = 3
> - [9,4,1,7] 最高分和最低分之间的差值是 7 - 1 = 6
>   可能的最小差值是 2

#### 题解：模拟

题目要求k个数中最高分与最低分差值的最小值，可以先排序，然后依次比较k个数值之间最高分与最低分差值，保存最小值即可。

**C++代码：**

```cpp
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        int n=nums.size();
        sort(nums.begin(),nums.end());
        int ans=0x3f3f3f3f;
        for(int i=k-1;i<n;i++){
            ans=min(ans,nums[i]-nums[i-k+1]);
        }
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	public int minimumDifference(int[] nums, int k) {
		int n = nums.length;
		Arrays.sort(nums, 0, n);
		int ans = 0x3f3f3f3f;
        for(int i=k-1;i<n;i++){
            ans=Math.min(ans,nums[i]-nums[i-k+1]);
        }
		return ans;
	}
}
```

### [2.13“气球” 的最大数量](https://leetcode-cn.com/problems/maximum-number-of-balloons/)

#### 题意

给你一个字符串 text，你需要使用 text 中的字母来拼凑尽可能多的单词 "balloon"（气球）。

字符串 text 中的每个字母最多只能被使用一次。请你返回最多可以拼凑出多少个单词 "balloon"。

**示例1：**


```
输入：text = "nlaebolko"
输出：1
```

**示例2：**


```
输入：text = "loonbalxballpoon"
输出：2
```

**示例3**

```
输入：text = "leetcode"
输出：0
```

**提示：**

- $1 <= text.length <= 10^4$
- `text` 全部由小写英文字母组成

#### 题解：暴力

直接见代码

**C++代码：**

```cpp
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        map<char,int> mp;
        for(int i=0;i<text.length();i++){
            mp[text[i]]++;
        }
        int ans=mp['b'];
        ans=min(ans,mp['a']);ans=min(ans,mp['l']/2);ans=min(ans,mp['o']/2);ans=min(ans,mp['n']);
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	public int maxNumberOfBalloons(String text) {
		Map<Character, Integer> mp = new HashMap<>();
		char[] s = text.toCharArray();
		for (int i = 0; i < s.length; i++) {
			mp.put(s[i], mp.getOrDefault(s[i], 0) + 1);
		}
		int ans = (int) mp.getOrDefault('b', 0);
		ans = Math.min(ans, mp.getOrDefault('a', 0));
		ans = Math.min(ans, mp.getOrDefault('l', 0) / 2);
		ans = Math.min(ans, mp.getOrDefault('o', 0) / 2);
		ans = Math.min(ans, mp.getOrDefault('n', 0));
		return ans;
	}
}
```

### [2.14：有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

#### 题意

给你一个仅由整数组成的有序数组，其中每个元素都会出现两次，唯有一个数只会出现一次。

请你找出并返回只出现一次的那个数。

你设计的解决方案必须满足 O(log n) 时间复杂度和 O(1) 空间复杂度。

**示例1：**

```
输入: nums = [1,1,2,3,3,4,4,8,8]
输出: 2
```

**示例2：**

```
输入: nums =  [3,3,7,7,10,11,11]
输出: 10
```

**提示：**

- $1 <= nums.length <= 10^5$
- $0 <= nums[i] <= 10^5$

#### 题解：二分

时间复杂度O(logn)秒想**二分**。由于是有序数组，假设我们需要查找的下标为x，当前二分中心点为mid，则：

- 如果mid为奇数下标，且a[mid]==a[mid-1]
- 如果mid为偶数下标，且a[mid]==a[mid+1]

上述满足则x在mid的右侧，调整l，否则x在mid的左侧调整r

> 注意：下标从0开始

当然可以用异或优化，这里省略。

**C++代码：**

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& a) {
        int n=a.size();
        int l=0,r=n-1;
        int mid,ans;
        while(l<r){
            mid=(l+r)>>1;
            if(mid&1){
                if(mid-1>=0&&a[mid]==a[mid-1]) l=mid+1;
                else r=mid;
            }else{
                if(mid+1<n&&a[mid+1]==a[mid]) l=mid+1;
                else r=mid;
            }
        }
        return a[l];
    }
};
```

**Java代码：**

```java
class Solution {
    public int singleNonDuplicate(int[] a) {
        int n=a.length;
        int l=0,r=n-1,mid=0;
        while(l<r){
            mid=(l+r)>>1;
            if(mid%2==1){
                if(mid-1>=0&&a[mid-1]==a[mid]) l=mid+1;
                else r=mid;
            }else{
                if(mid+1<n&&a[mid+1]==a[mid]) l=mid+1;
                else r=mid;
            }
        }
        return a[l];
    }
}
```

### [2.15：矩阵中的幸运数](https://leetcode-cn.com/problems/lucky-numbers-in-a-matrix/)

#### 题意

给你一个 m * n 的矩阵，矩阵中的数字 各不相同 。请你按 任意 顺序返回矩阵中的所有幸运数。

幸运数是指矩阵中满足同时下列两个条件的元素：

- 在同一行的所有元素中最小
- 在同一列的所有元素中最大

**示例1：**

```
输入：matrix = [[3,7,8],[9,11,13],[15,16,17]]
输出：[15]
解释：15 是唯一的幸运数，因为它是其所在行中的最小值，也是所在列中的最大值。
```

**示例2：**

```
输入：matrix = [[1,10,4,2],[9,3,8,7],[15,16,17,12]]
输出：[12]
解释：12 是唯一的幸运数，因为它是其所在行中的最小值，也是所在列中的最大值。
```

**示例3：**

```
输入：matrix = [[7,8],[1,2]]
输出：[7]
```

**提示：**

- $m == mat.length$
- $n == mat[i].length$
- $1 <= n, m <= 50$
- $1 <= matrix[i][j] <= 10^5$
- 矩阵中的所有元素都是不同的

#### 题解：模拟

由于数据量比较小，可以直接先遍历整个矩阵，存储每行最小值和每列最大值。再遍历一次矩阵找出符合条件的元素即可。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        int n=matrix.size();int m=matrix[0].size();
        vector<int> rmin(n,0x3f3f3f3f);
        vector<int> cmax(m,0);
        vector<int> ans;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                rmin[i]=min(rmin[i],matrix[i][j]);
                cmax[j]=max(cmax[j],matrix[i][j]);
            }
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(matrix[i][j]==rmin[i]&&matrix[i][j]==cmax[j]) ans.push_back(matrix[i][j]);
            }
        }
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	public List<Integer> luckyNumbers(int[][] a) {
		List<Integer> ans = new ArrayList<>();
		int n = a.length;
		int m = a[0].length;
		int rmin[] = new int[n];
		int cmax[] = new int[m];
		Arrays.fill(rmin, 0x3f3f3f3f);
		Arrays.fill(cmax, 0);
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				rmin[i] = Math.min(rmin[i], a[i][j]);
				cmax[j] = Math.max(cmax[j], a[i][j]);
			}
		}
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (a[i][j] == rmin[i] && a[i][j] == cmax[j])
					ans.add(a[i][j]);
			}
		}
		return ans;
	}
}
```

### [2.17：骑士在棋盘上的概率](https://leetcode-cn.com/problems/knight-probability-in-chessboard/)

#### 题意

在一个 n x n 的国际象棋棋盘上，一个骑士从单元格 (row, column) 开始，并尝试进行 k 次移动。行和列是 从 0 开始 的，所以左上单元格是 (0,0) ，右下单元格是 (n - 1, n - 1) 。

象棋骑士有8种可能的走法，如下图所示。每次移动在基本方向上是两个单元格，然后在正交方向上是一个单元格。

![img](https://gitee.com/serendipity_LB/img/raw/master/knight.png)

每次骑士要移动时，它都会随机从8种可能的移动中选择一种(即使棋子会离开棋盘)，然后移动到那里。

骑士继续移动，直到它走了 k 步或离开了棋盘。

返回 骑士在棋盘停止移动后仍留在棋盘上的概率 。

**示例1：**

```markdown
输入: n = 3, k = 2, row = 0, column = 0
输出: 0.0625
解释: 有两步(到(1,2)，(2,1))可以让骑士留在棋盘上。
在每一个位置上，也有两种移动可以让骑士留在棋盘上。
骑士留在棋盘上的总概率是0.0625。
```

**示例2：**

```markdown
输入: n = 1, k = 0, row = 0, column = 0
输出: 1.00000
```

**提示：**

- `1 <= n <= 25`
- `0 <= k <= 100`
- `0 <= row, column <= n`

#### 题解：dp

$dp[i][j][k]为从位置(i,j)出发，使用步数不超过k步，最后仍在棋盘内的概率$

状态方程转移，对下一步落点$(fx,fy)$进行分情况讨论：

- 对于$(fx,fy)$在棋盘外的情况，无需考虑
- 若下一步落点$(fx,fy)$在棋盘内，其剩余可用步数为k-1，则最后仍在棋盘的概率为$f[fx][fy][k-1]$，则落点$(fx,fy)$对$f[i][j][k]$的贡献为$f[fx][fy][k-1]*\frac{1}{8}$，其中$\frac{1}{8}$为事件 **从(i,j)走到(fx,fy)**的概率，该事件与**到达(fx,fy)后进行后续移动并留在棋盘**为相互独立事件。

最终$f[i][j][k]$为 **八连通**落点概率之和，即：

$$
f[i][j][k]=\Sigma f[fx][fy][k-1]*\frac{1}{8}
$$

**C++代码：**

```cpp
class Solution {
public:
    int dir[8][2]={-2,1,-2,-1,2,1,2,-1,1,-2,1,2,-1,2,-1,-2};
    double knightProbability(int n, int k, int row, int column) {
        double dp[n][n][k+1];
        memset(dp,0,sizeof(dp));
        for(int i=0;i<n;i++) for(int j=0;j<n;j++) dp[i][j][0]=1;
        for(int p=1;p<=k;p++){
            for(int i=0;i<n;i++){
                for(int j=0;j<n;j++){
                    for(int d=0;d<8;d++){
                        int fx=i+dir[d][0];int fy=j+dir[d][1];
                        if(fx<0||fx>=n||fy<0||fy>=n) continue;
                        dp[i][j][p]+=dp[fx][fy][p-1]/8;
                    }
                }
            }
        }
        return dp[row][column][k];
    }
};
```

**Java代码：**

```java
class Solution {
	int dir[][]=new int[][]{{-1,-2},{-1,2},{1,-2},{1,2},{-2,1},{-2,-1},{2,1},{2,-1}};
	public double knightProbability(int n, int k, int row, int column) {
        double dp[][][]=new double[n][n][k+1];
        for(int i=0;i<n;i++){
        	for(int j=0;j<n;j++){
        		dp[i][j][0]=1;
        	}
        }
        for(int p=1;p<=k;p++){
        	for(int i=0;i<n;i++){
        		for(int j=0;j<n;j++){
        			for(int[] d:dir){
        				int fx=i+d[0];int fy=j+d[1];
        				if(fx<0||fx>=n||fy<0||fy>=n) continue;
        				dp[i][j][p]+=dp[fx][fy][p-1]/8;
        			}
        		}
        	}
        }
        return dp[row][column][k];
    }
}
```

### [2.18：找出星型图的中心节点](https://leetcode-cn.com/problems/find-center-of-star-graph/)

#### 题意

有一个无向的 星型 图，由 n 个编号从 1 到 n 的节点组成。星型图有一个 中心 节点，并且恰有 n - 1 条边将中心节点与其他每个节点连接起来。

给你一个二维整数数组 edges ，其中 edges[i] = [ui, vi] 表示在节点 ui 和 vi 之间存在一条边。请你找出并返回 edges 所表示星型图的中心节点。

**示例1：**

![img](https://gitee.com/serendipity_LB/img/raw/master/star_graph.png)

```markdown
输入：edges = [[1,2],[2,3],[4,2]]
输出：2
解释：如上图所示，节点 2 与其他每个节点都相连，所以节点 2 是中心节点。
```

**示例2：**

```markdown
输入：edges = [[1,2],[5,1],[1,3],[1,4]]
输出：1
```

**提示：**

- $3 <= n <= 10^5$
- $edges.length == n - 1$
- $edges[i].length == 2$
- $1 <= ui, vi <= n$
- $ui != vi$
- 题目数据给出的 edges 表示一个有效的星型图

#### 题解

简单题，由于是星型图，所以中间结点有n-1（n为结点个数）条边，直接用数组存储每个节点边的条数，然后遍历n个结点，判断是否存在n-1条边的结点即可。

**C++代码：**

```cpp
class Solution {
public:
    int a[100005];
    int findCenter(vector<vector<int>>& edges) {
        int n=edges.size();
        for(int i=0;i<n;i++){
            a[edges[i][0]]++;a[edges[i][1]]++;
        }
        int ans=0;
        for(int i=1;i<=n+1;i++){
            if(a[i]==n){
                ans=i;break;
            }
        }
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
    public int findCenter(int[][] edges) {
        int n=edges.length;
        int a[]=new int[n+2];
        for(int i=0;i<n;i++){
            a[edges[i][0]]++;a[edges[i][1]]++;
        }
        int ans=0;
        for(int i=1;i<=n+1;i++){
            if(a[i]==n){
                ans=i;break;
            }
        }
        return ans;
    }
}
```

### [2.20：1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)

#### 题意

有两种特殊字符：

- 第一种字符可以用一个比特 0 来表示
- 第二种字符可以用两个比特(10 或 11)来表示、

给定一个以 0 结尾的二进制数组 bits ，如果最后一个字符必须是一位字符，则返回 true 。

**示例1：**

```
输入: bits = [1, 0, 0]
输出: true
解释: 唯一的编码方式是一个两比特字符和一个一比特字符。
所以最后一个字符是一比特字符。
```

**示例2：**

```
输入: bits = [1, 1, 1, 0]
输出: false
解释: 唯一的编码方式是两比特字符和两比特字符。
所以最后一个字符不是一比特字符。
```

**提示：**

- `1 <= bits.length <= 1000`
- `bits[i] == 0 or 1`

#### 题解：简单模拟

判断最后一个字符是否为一位字符需要满足两个条件：

- 最后一个字符必须为0
- 最后一个字符0不能与前面的匹配

于是只需遍历数组，如果当前遍历的字符为1，则需要和后面的字符组成一个二位字符；如果当前遍历的字符为0，则只需要组成一位字符即可。

**C++代码：**

```cpp
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int i=0;
        for(i=0;i<bits.size()-1;i++){
            if(bits[i]==1) i++;
        }
        if(i==bits.size()-1&&bits[bits.size()-1]==0) return true;
        return false;
    }
};
```

**Java代码：**

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int n=bits.length;
        int i=0;
        for(i=0;i<n-1;i++){
            if(bits[i]==1) i++;
        }
        if(i==n-1&&bits[n-1]==0) return true;
        return false; 
    }
}
```

### [2.21：推多米诺](https://leetcode-cn.com/problems/push-dominoes/)

#### 题意

n 张多米诺骨牌排成一行，将每张多米诺骨牌垂直竖立。在开始时，同时把一些多米诺骨牌向左或向右推。

每过一秒，倒向左边的多米诺骨牌会推动其左侧相邻的多米诺骨牌。同样地，倒向右边的多米诺骨牌也会推动竖立在其右侧的相邻多米诺骨牌。

如果一张垂直竖立的多米诺骨牌的两侧同时有多米诺骨牌倒下时，由于受力平衡， 该骨牌仍然保持不变。

就这个问题而言，我们会认为一张正在倒下的多米诺骨牌不会对其它正在倒下或已经倒下的多米诺骨牌施加额外的力。

给你一个字符串 dominoes 表示这一行多米诺骨牌的初始状态，其中：

- dominoes[i] = 'L'，表示第 i 张多米诺骨牌被推向左侧，
- dominoes[i] = 'R'，表示第 i 张多米诺骨牌被推向右侧，
- dominoes[i] = '.'，表示没有推动第 i 张多米诺骨牌。

返回表示最终状态的字符串。

**示例1：**

```
输入：dominoes = "RR.L"
输出："RR.L"
解释：第一张多米诺骨牌没有给第二张施加额外的力。
```

**示例2：**

![img](https://gitee.com/serendipity_LB/img/raw/master/domino.png)

```
输入：dominoes = ".L.R...LR..L.."
输出："LL.RR.LLRRLL.."
```

**提示：**

- $n == dominoes.length$
- $1 <= n <= 10^5$
- `dominoes[i]` 为 `'L'`、`'R'` 或 `'.'`

#### 题解：模拟

枚举所有连续的没有被推动的骨牌，

- 如果两侧方向相同，则倒向同一侧
- 如果两侧方向相对，则从两侧往中间倒
- 如果两侧方向相反，则保持竖立

如果左侧没有被推倒的骨牌，则假设有一个向左倒的牌；如果右侧没有被推倒的骨牌，则假设有一个向右倒的骨牌。这样假设不会影响最终形态，也避免的边界情况特判。于是代码如下：

**C++代码：**

```cpp
class Solution {
public:
    string pushDominoes(string dominoes) {
        string s=dominoes;
        int n=s.length(),i=0;
        char l='L';
        while(i<n){
            int j=i;
            while(j<n&&s[j]=='.') j++;
            char r=j<n?s[j]:'R';
            if(l==r){  //如果方向相同，则倒向同一侧
                while(i<j) s[i]=r,i++;
            }else if(l=='R'&&r=='L'){   //如果方向相对，则从两侧往中间倒
                int k=j-1;
                while(i<k){
                    s[i]=l;i++;
                    s[k]=r;k--;
                }
            }
            l=r;i=j+1;
        }
        return s;
    }
};
```

**Java代码：**

```java
class Solution {
	public String pushDominoes(String dominoes) {
		char[] s=dominoes.toCharArray();
		int n=s.length;int i=0;
		char l='L';
		while(i<n){
			int j=i;
			while(j<n&&s[j]=='.'){
				j++;
			}
			char r=j<n?s[j]:'R';
			if(l==r){//如果方向相同，则倒向同一侧
				while(i<j){
					s[i]=r;i++;
				}
			}else if(l=='R'&&r=='L'){  //如果方向相对，则从两侧往中间倒
				int k=j-1;
				while(i<k){
					s[i]=l;i++;
					s[k]=r;k--;
				}
			}
			l=r;i=j+1;
		}
		return new String(s);
	}
}
```

### [2.23：仅仅反转字母](https://leetcode-cn.com/problems/reverse-only-letters/)

#### 题意

给你一个字符串 `s` ，根据下述规则反转字符串：

- 所有非英文字母保留在原有位置。
- 所有英文字母（小写或大写）位置反转。

返回反转后的 `s` 。

**示例1：**

```
输入：s = "ab-cd"
输出："dc-ba"
```

**示例2：**

```
输入：s = "a-bC-dEf-ghIj"
输出："j-Ih-gfE-dCba"
```

**示例3：**

```
输入：s = "Test1ng-Leet=code-Q!"
输出："Qedo1ct-eeLg=ntse-T!"
```

**提示：**

- $1 <= s.length <= 100$
- `s` 仅由 ASCII 值在范围 `[33, 122]` 的字符组成
- `s` 不含 `'\"'` 或 `'\\'`

#### 题解：模拟

用两变量表示当前需要反转的两个字母的下标l,r，如果$l>=r$，则证明已经反转完成。

**C++代码：**

```cpp
class Solution {
public:
    int checkEng(char c){
        if(c>='a'&&c<='z') return 1;
        if(c>='A'&&c<='Z') return 1;
        return 0;
    }
    string reverseOnlyLetters(string s) {
        int n=s.length();
        int r=n-1;
        for(int l=0;l<n;l++){
            if(checkEng(s[l])){
                while(!checkEng(s[r])) r--;
                if(l>=r) break;
                swap(s[l],s[r]);
                r--;
            }
        }
        return s;
    }
};
```

**Java代码：**

```java
class Solution {
	public boolean checkEng(char c) {
		if (c >= 'a' && c <= 'z')
			return true;
		if (c >= 'A' && c <= 'Z')
			return true;
		return false;
	}
	public String reverseOnlyLetters(String s) {
		char[] str = s.toCharArray();
		int n = str.length;
		int l = 0, r = n - 1;
		for (int i = 0; i < n; i++, l++) {
			if (checkEng(str[i])) {
				while (!checkEng(str[r]))
					r--;
				if (l >= r)
					break;
				char c = str[l];
				str[l] = str[r];
				str[r] = c;
				r--;
			}
		}
		return new String(str);
	}
}
```

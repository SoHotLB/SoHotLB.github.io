---
title: LeetCode2022年每日一题1月打卡汇总
categories: 'LeetCode每日一题打卡'
tags: 
  - LeetCode
  - Algorithm
katex: true

---

## LeetCode2022年每日一题1月打卡汇总

### [1.14：查找和最小的K对数字](https://leetcode-cn.com/problems/find-k-pairs-with-smallest-sums/)

####  **题意**

给定两个以 升序排列 的整数数组 $nums1$ 和 $nums2$ , 以及一个整数 $k$ 。

定义一对值 (u,v)，其中第一个元素来自 $nums1$，第二个元素来自 $nums2$ 。

请找到和最小的 k 个数对 $(u1,v1)$,  $(u2,v2)$  ...  $(uk,vk)$ 。

**示例**

**示例1：**

> 输入: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
> 输出: [1,2],[1,4],[1,6]
> 解释: 返回序列中的前 3 对数：
>      [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

**示例2：**

> 输入: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
> 输出: [1,1],[1,1]
> 解释: 返回序列中的前 2 对数：
>      [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

**示例3：**

> 输入: nums1 = [1,2], nums2 = [3], k = 3 
> 输出: [1,3],[2,3]
> 解释: 也可能序列中所有的数对都被返回:[1,3],[2,3]

**提示：**

- $1 <= nums1.length, nums2.length <= 10^5$
- $10^9 <= nums1[i], nums2[i] <= 10^9$
- $nums1 和 nums2 均为升序排列$
- $1 <= k <= 1000$

#### **题解：优先队列**

由于题目所求为前k个最小的数对，同时两个数组均为**升序排列**，所以最小的一定为$nums1[0],nums2[0]$，次小的必定在${nums1[0],nums2[1]},{nums1[1],nums2[0]}$之中，而具体是哪个需要判断：

- 如果直接二重循环暴力判断，题目给定数组长度为$10^5$显然不行
- 考虑用数据结构存储，从而判断哪个次小，于是首选优先队列

如果优先队列存储$nums1$和$nums2$的值的话，可能会出现重复，于是优先队列只存储$nums1$和$nums2$的下标.

因为题目只需要前k个最小的，于是最多只需要出队k次即可。

> - 时间复杂度：O(klogk) （因为需要出队k次，同时每次入队需要logk的时间复杂度）
> - 空间复杂度：O(k)

**C++代码：**

> ！！记得a，b数组初始置空，LeetCode判题机到现在还没摸清

```cpp
vector<int> a,b;
class Solution {
public:
    struct node{
        int u,v;
        bool operator < (const node p) const{
            return a[u]+b[v] > (a[p.u]+b[p.v]);
        }
    };
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        a.clear();b.clear();
        int n=nums1.size(),m=nums2.size();
        for(int i=0;i<n;i++) a.push_back(nums1[i]);
        for(int i=0;i<m;i++) b.push_back(nums2[i]);
        vector<vector<int> >ans;
        
        //定义优先队列，存储两数组元素下标
        priority_queue<node> q;
        //先nums1所有的下标入队
        for(int i=0;i<min(k,n);i++){
            q.push(node{i,0});
        }
        //最多出队k次
        while(k>0&&!q.empty()){
            node now=q.top();q.pop();

            vector<int> ve;ve.push_back(nums1[now.u]);ve.push_back(nums2[now.v]);
            ans.push_back(ve);
            //nums2数字下标右移一位，然后入队
            if(++now.v<m)
                q.push(node{now});
            k--;
        }
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
		//返回值
        List<List<Integer>> ans=new ArrayList<>();
        
        //定义优先队列存储两数组下标
        PriorityQueue<int[]> q=new PriorityQueue<>((a,b) ->
        		nums1[a[0]]+nums2[a[1]]-(nums1[b[0]]+nums2[b[1]])); 
        //首先将nums1数组下标入队（因为不需要只需要入队k个即可，所以取个最小值）
        for(int i=0;i<Math.min(k, nums1.length);i++){
        	q.add(new int[]{i,0});
        }
        //最多弹出k次
        while(k>0&&!q.isEmpty()){
        	int[] a=q.poll();
        	//返回当前最小的和
        	List now=new ArrayList<>();
        	now.add(nums1[a[0]]);now.add(nums2[a[1]]);
        	ans.add(now);
        	
        	//nums2数组下标右移，然后继续入队
        	if(++a[1]<nums2.length)
        		q.add(a);
        	k--;
        }
        return ans;
    }
}
```

### [1.15：计算力扣银行的钱](https://leetcode-cn.com/problems/calculate-money-in-leetcode-bank/)

#### 题意

Hercy 想要为购买第一辆车存钱。他 每天 都往力扣银行里存钱。

最开始，他在周一的时候存入 1 块钱。从周二到周日，他每天都比前一天多存入 1 块钱。在接下来每一个周一，他都会比 前一个周一 多存入 1 块钱。

给你 n ，请你返回在第 n 天结束的时候他在力扣银行总共存了多少块钱。

**示例1：**

> 输入：n = 4
> 输出：10
> 解释：第 4 天后，总额为 1 + 2 + 3 + 4 = 10 。

**示例2：**

> 输入：n = 10
> 输出：37
> 解释：第 10 天后，总额为 (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4) = 37 。注意到第二个星期一，Hercy 存入 2 块钱。
>

**示例3：**

> 输入：n = 20
> 输出：96
> 解释：第 20 天后，总额为 (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4 + 5 + 6 + 7 + 8) + (3 + 4 + 5 + 6 + 7 + 8) = 96 。
>

**提示：**

- `1 <= n <= 1000`

#### 题解：暴力

由于n最大为1000，于是直接循环遍历即可，用两个变量记录当前为第几周的周几即可。具体见代码

**C++代码：**

```cpp
class Solution {
public:
    int totalMoney(int n) {
        int sum=0,num,x;   //num表示第几周，x表示周几
        for(int i=1;i<=n;i++){
            x=i%7;num=i/7;
            if(x==0) x=7,num--;
            sum+=x+num;
        }
        return sum;
    }
};
```

**Java代码：**

```java
class Solution {
    public int totalMoney(int n) {
        int ans=0,week,day;
        for(int i=1;i<=n;i++){
            week=i/7;day=i%7;
            if(day==0) {
                week--;day=7;
            }
            ans+=week+day;
        }
        return ans;
    }
}
```

### [1.16：链表随机节点](https://leetcode-cn.com/problems/linked-list-random-node/)

#### 题意

给你一个单链表，随机选择链表的一个节点，并返回相应的节点值。每个节点 被选中的概率一样 。

实现 Solution 类：

- Solution(ListNode head) 使用整数数组初始化对象。
- int getRandom() 从链表中随机选择一个节点并返回该节点的值。链表中所有节点被选中的概率相等。

**示例：**

![img](https://gitee.com/serendipity_LB/img/raw/master/getrand-linked-list.jpg)

> 输入
> ["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"]
> [[[1, 2, 3]], [], [], [], [], []]
> 输出
> [null, 1, 3, 2, 2, 3]
>
> 解释
> Solution solution = new Solution([1, 2, 3]);
> solution.getRandom(); // 返回 1
> solution.getRandom(); // 返回 3
> solution.getRandom(); // 返回 2
> solution.getRandom(); // 返回 2
> solution.getRandom(); // 返回 3
> // getRandom() 方法应随机返回 1、2、3中的一个，每个元素被返回的概率相等。

#### 题解：随机数

**C++代码：**

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
    vector<int> ve;
    Solution(ListNode* head) {
        while(head!=nullptr){
            ve.push_back(head->val);
            head=head->next;
        }
    }
    
    int getRandom() {
        return ve[rand()%ve.size()];
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```

**Java代码：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    List<Integer> list;
    public Solution(ListNode head) {
        list=new ArrayList<Integer>();
        while(head!=null){
            list.add(head.val);
            head=head.next;
        }
    }
    
    public int getRandom() {
        return list.get(new Random().nextInt(list.size()));
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

### [1.17：统计元音字母序列的数目](https://leetcode-cn.com/problems/count-vowels-permutation/)

#### 题意

给你一个整数 n，请你帮忙统计一下我们可以按下述规则形成多少个长度为 n 的字符串：

- 字符串中的每个字符都应当是小写元音字母（'a', 'e', 'i', 'o', 'u'）
- 每个元音 'a' 后面都只能跟着 'e'
- 每个元音 'e' 后面只能跟着 'a' 或者是 'i'
- 每个元音 'i' 后面 不能 再跟着另一个 'i'
- 每个元音 'o' 后面只能跟着 'i' 或者是 'u'
- 每个元音 'u' 后面只能跟着 'a'

由于答案可能会很大，所以请你返回 模 10^9 + 7 之后的结果。

**提示：**

- `1 <= n <= 2 * 10^4`

**示例1：**

> 输入：n = 1
> 输出：5
> 解释：所有可能的字符串分别是："a", "e", "i" , "o" 和 "u"。

**示例2：**

> 输入：n = 2
> 输出：10
> 解释：所有可能的字符串分别是："ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" 和 "ua"。
>

**示例3：**

> 输入：n = 5
> 输出：68

#### 题解：dp

通过题意可以知道：

- 以a结尾的话前面只能是u,i,e
- 以e结尾的话前面只能是a,i
- 以i结尾的话前面只能是e,o
- 以o结尾的话前面只能是i
- 以u结尾的话前面只能是i,o

于是我们最终的结果可以视为：n个字符分别以a,e,i,o,u结尾的个数总和。

联想到dp，于是$dp[i][j]：前i个字符其中以字符j结尾能满足题意的字符串个数$

> 个人习惯dp数组下标从1开始：于是j=1,2,3,4,5分别表示为a,e,i,o,u

于是状态转移方程为：

$dp[i][1]=dp[i-1][2]+dp[i-1][3]+dp[i-1][5]$
$dp[i][2]=dp[i-1][1]+dp[i-1][3]$
$dp[i][3]=dp[i-1][2]+dp[i-1][4]$
$dp[i][4]=dp[i-1][3]$
$dp[i][5]=dp[i-1][3]+dp[i-1][4]$

具体代码如下：

**C++代码：**

```cpp
class Solution {
public:
    int dp[20010][6];
    int mod=1e9+7;
    int countVowelPermutation(int n) {
        int ans=0;
        for(int i=1;i<=5;i++) dp[1][i]=1;
        for(int i=2;i<=n;i++){
            dp[i][1]=((dp[i-1][2]+dp[i-1][3])%mod+dp[i-1][5])%mod;  //a结尾的话前面只能是u,i,e;后面同理
            dp[i][2]=(dp[i-1][1]+dp[i-1][3])%mod;
            dp[i][3]=(dp[i-1][2]+dp[i-1][4])%mod;
            dp[i][4]=dp[i-1][3]%mod;
            dp[i][5]=(dp[i-1][3]+dp[i-1][4])%mod;
        }
        for(int i=1;i<=5;i++) ans=(ans+dp[n][i])%mod;
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
    public int countVowelPermutation(int n) {
        int dp[][]=new int[n+1][6];  //dp[i][j]：前i个字符其中以字符j结尾组成满足题意的个数
        int mod=(int)1e9+7;
        int ans=0;
        //初始化
        for(int i=1;i<=5;i++) dp[1][i]=1;
        for(int i=2;i<=n;i++){
            dp[i][1]=((dp[i-1][2]+dp[i-1][3])%mod+dp[i-1][5])%mod;  //a结尾的话前面只能是u,i,e;后面同理
            dp[i][2]=(dp[i-1][1]+dp[i-1][3])%mod;
            dp[i][3]=(dp[i-1][2]+dp[i-1][4])%mod;
            dp[i][4]=dp[i-1][3]%mod;
            dp[i][5]=(dp[i-1][3]+dp[i-1][4])%mod;
        }
        for(int i=1;i<=5;i++) ans=(ans+dp[n][i])%mod;
        return ans;
    }
}
```

### [1.18：最小时间差](https://leetcode-cn.com/problems/minimum-time-difference/)

#### 题意

给定一个 24 小时制（小时:分钟 **"HH:MM"**）的时间列表，找出列表中任意两个时间的最小时间差并以分钟数表示。

 **示例1：**

> 输入：timePoints = ["23:59","00:00"]
> 输出：1

**示例2：**

> 输入：timePoints = ["00:00","23:59","00:00"]
> 输出：0

**提示：**

- $2 <= timePoints.length <= 2 * 10^4$
- $timePoints[i]$ 格式为 **"HH:MM"**

#### 题解：排序

**第一个注意点：**

首先需要注意的是，因为是24小时制，所以最大的差为：12:00——0:00（即12*60=720min）。

我们先将所有的时间转化为分钟，若两者的时间差超过了720min（如示例1中$23*60+59-0min$ 明显大于了720则表示应该往后计算，即： ![](https://gitee.com/serendipity_LB/img/raw/master/image-20220118145723933.png)<img src="" alt="image-20220118145723933" style="zoom: 80%;" />

于是便可将所有的时间存储到一个数组中，然后按照时间从小到大排序，再两两之间求和。

**第二个注意点：**

需要注意最后一个时间和第一个时间也需要求差。

具体代码如下：

**C++代码：**

```cpp
class Solution {
public:
    int findMinDifference(vector<string>& timePoints) {
        vector<int> ve;
        int n=timePoints.size();
        for(int i=0;i<n;i++){
            string s=timePoints[i];
            int h=(s[0]-'0')*10+(s[1]-'0');
            int m=(s[3]-'0')*10+(s[4]-'0');
            ve.push_back(h*60+m);
        }
        sort(ve.begin(),ve.end());
        int ans=0x3f3f3f3f;
        int num;
        for(int i=0;i<n-1;i++){
            num=ve[i+1]-ve[i];
            if(num>720) num=24*60-num;
            ans=min(ans,num);
        }
        num=ve[n-1]-ve[0];
        if(num>720) num=24*60-num;
        ans=min(ans,num);
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	public int findMinDifference(List<String> timePoints) {
		List<Integer> list = new ArrayList<>();
		int n = timePoints.size();
		for (int i = 0; i < n; i++) {
			int h = Integer.parseInt(timePoints.get(i).substring(0, 2));
			int m = Integer.parseInt(timePoints.get(i).substring(3, 5));
			list.add(h * 60 + m);
		}
		int ans = 0x3f3f3f3f;
		// 可以知道最大的情况为：12:00-0:00 差最大为720
		Collections.sort(list, new cmp());
        int num;
		for (int i = 0; i < n - 1; i++) {
            num=list.get(i+1)-list.get(i);
            if(num>720) num=24*60-num;
			ans = Math.min(ans,num);
		}
		//计算最后一个和第一个的差
        num=list.get(n-1)-list.get(0);
        if(num>720) num=24*60-num;
        ans=Math.min(ans,num);
		return ans;
	}

	class cmp implements Comparator {
		@Override
		public int compare(Object o1, Object o2) {
			// TODO Auto-generated method stub
            if((int)o1-(int)o2>=0) return 1;
            else return -1;
		}
	}
}
```

### [1.19：存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

#### 题意

给你一个整数数组 nums 和一个整数 k ，判断数组中是否存在两个 不同的索引 i 和 j ，满足 nums[i] == nums[j] 且 abs(i - j) <= k 。如果存在，返回 true ；否则，返回 false 。

**示例1：**

> 输入：nums = [1,2,3,1], k = 3
> 输出：true

**示例2：**

> 输入：nums = [1,0,1,1], k = 1
> 输出：true

**示例3：**

> 输入：nums = [1,2,3,1,2,3], k = 2
> 输出：false

**提示：**

- $1 <= nums.length <= 10^5$
- $-10^9 <= nums[i] <= 10^9$
- $0 <= k <= 10^5$

#### 题解：排序 & 哈希

**个人题解：排序**

简单题我重拳出击, 中等题我唯唯诺诺, 困难题我复制粘贴。

数据范围$n<=10^5$，直接O(nlogn)搞起，题目要求判断数组中是否存在两个相同的数，且其下标的差的绝对值<=k。于是用一结构体将数组中每个元素的值和下标存入，接着按照元素的值从小到大排序，**如果元素相同，则按照索引从小到大排序**

再遍历一遍数组，因为排序的规则如上，只需判断相邻的元素值是否相同，如果相同则比较两者原来的索引的差的绝对值是否<=k即可。

<img src="https://gitee.com/serendipity_LB/img/raw/master/QQ%E5%9B%BE%E7%89%8720220119141727.png" alt="QQ图片20220119141727" style="zoom: 33%;" />

**C++代码：**

```cpp
class Solution {
public:
    struct node{
        int num,pos;
        bool operator < (const node p) const{
            if(num!=p.num) return num<p.num;
            else return pos<p.pos;
        }
    };
    node a[100010];
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n=nums.size();
        for(int i=0;i<n;i++){
            a[i].num=nums[i];a[i].pos=i;
        }
        sort(a,a+n);
        bool flag=false;
        for(int i=0;i<n-1;i++){
            if(a[i].num==a[i+1].num){
                if(abs(a[i].pos-a[i+1].pos)<=k){
                    flag=true;break;
                }
            }
        }
        return flag;
    }
};
```

**Java代码：**

```java
class Node {
	int num, pos;

	Node(int num, int pos) {
		this.num = num;
		this.pos = pos;
	}
}

class Cmp implements Comparator<Node> {

	@Override
	public int compare(Node o1, Node o2) {
		// TODO Auto-generated method stub
		if (o1.num != o2.num) {
			if (o1.num < o2.num)
				return 1;
			else
				return -1;
		} else {
			if (o1.pos < o2.pos)
				return 1;
			else
				return -1;
		}
	}

}

class Solution {
	public boolean containsNearbyDuplicate(int[] nums, int k) {
		int n = nums.length;
		Node[] a = new Node[n];
		for (int i = 0; i < n; i++) {
			Node now = new Node(nums[i], i);
			a[i] = now;
		}
		Arrays.sort(a, new Cmp());
		for (int i = 0; i < n - 1; i++) {
			if (a[i].num == a[i + 1].num) {
				if (Math.abs(a[i].pos - a[i + 1].pos) <= k) {
					return true;
				}
			}
		}
		return false;
	}
}
```

**官方题解：哈希**

用哈希表存储每个元素的**最大下标**。从左到右遍历数组，例如遍历到元素nums[i]

- 如果哈希表中存在当前元素，则比较最大下标与当前元素的下标i差的绝对值是否<=k，
  - 如果是，则返回true
  - 否则比较当前的下标，更新最大下标
- 哈希表中插入当前下标

**C++代码：**

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n=nums.size();
        map<int,int> mp;
        for(int i=0;i<n;i++){
            if(mp.count(nums[i])){
                if(abs(i-mp[nums[i]])<=k) return true;
                mp[nums[i]]=max(mp[nums[i]],i);
            }else mp[nums[i]]=i;
        }
        return false;
    }
};
```

**Java代码：**

```java
class Solution {
	public boolean containsNearbyDuplicate(int[] nums, int k) {
		int n = nums.length;
		Map<Integer, Integer> map = new HashMap<Integer, Integer>();
		for (int i = 0; i < n; i++) {
			if (map.containsKey(nums[i])) {
				if (Math.abs(map.get(nums[i]) - i) <= k)
					return true;
				if (map.get(nums[i]) < i)
					map.put(nums[i], i);
			} else {
				map.put(nums[i], i);
			}
		}
		return false;
	}
}
```

### [1.20：石子游戏 IX](https://leetcode-cn.com/problems/stone-game-ix/)

#### **题意**

Alice 和 Bob 再次设计了一款新的石子游戏。现有一行 n 个石子，每个石子都有一个关联的数字表示它的价值。给你一个整数数组 stones ，其中 stones[i] 是第 i 个石子的价值。

Alice 和 Bob 轮流进行自己的回合，Alice 先手。每一回合，玩家需要从 stones 中移除任一石子。

- 如果玩家移除石子后，导致 所有已移除石子 的价值 总和 可以被 3 整除，那么该玩家就 输掉游戏 。
- 如果不满足上一条，且移除后没有任何剩余的石子，那么 Bob 将会直接获胜（即便是在 Alice 的回合）。

假设两位玩家均采用 最佳 决策。如果 Alice 获胜，返回 true ；如果 Bob 获胜，返回 false 。

#### 题解：博弈

博弈，思路还是有点难想

见官方题解：https://leetcode-cn.com/problems/stone-game-ix/solution/shi-zi-you-xi-ix-by-leetcode-solution-kk5f/

Alice获胜的条件为：

- 0为偶数个，且1,2至少有1个
- 0为奇数个，且1比2多2个（或2比1多2个）

**C++代码：**

```cpp
class Solution {
public:
    bool stoneGameIX(vector<int>& stones) {
        int cnt0=0,cnt1=0,cnt2=0;
        for(int i=0;i<stones.size();i++){
            if(stones[i]%3==0) cnt0++;
            else if(stones[i]%3==1) cnt1++;
            else cnt2++;
        }
        if(cnt0%2==0){
            if(cnt1&&cnt2) return true;
            else return false;
        }else{
            if(cnt1-cnt2>2||cnt2-cnt1>2) return true;
            else return false;
        }
    }
};
```

### [1.21：跳跃游戏 IV](https://leetcode-cn.com/problems/jump-game-iv/)

#### 题意

给你一个整数数组 arr ，你一开始在数组的第一个元素处（下标为 0）。

每一步，你可以从下标 i 跳到下标：

- i + 1 满足：i + 1 < arr.length
- i - 1 满足：i - 1 >= 0
- j 满足：arr[i] == arr[j] 且 i != j

请你返回到达数组最后一个元素的下标处所需的 最少操作次数 。

注意：任何时候你都不能跳到数组外面。

**示例1：**

> 输入：arr = [100,-23,-23,404,100,23,23,23,3,404]
> 输出：3
> 解释：那你需要跳跃 3 次，下标依次为 0 --> 4 --> 3 --> 9 。下标 9 为数组的最后一个元素的下标。
>

**示例2：**

> 输入：arr = [7,6,9,6,9,6,9,7]
> 输出：1
> 解释：你可以直接从下标 0 处跳到下标 7 处，也就是数组的最后一个元素处。

**示例3：**

> 输入：arr = [11,22,7,7,7,7,7,7,7,22,13]
> 输出：3

**提示：**

- $1 <= arr.length <= 5 * 10^4$
- $-10^8 <= arr[i] <= 10^8$

#### 题解：BFS

首先将相同值存入map中(value为数组)，然后bfs遍历求最短路径

**C++代码：**

```cpp
class Solution {
public:
    int inf=0x3f3f3f3f;
    int minJumps(vector<int>& a) {
        int n=a.size();
        map<int,vector<int> > mp;
        //初始化map
        for(int i=0;i<n;i++) mp[a[i]].push_back(i);
        vector<int> dis(n,inf);
        queue<int> q;
        q.push(0);dis[0]=0;

        while(!q.empty()){
            int u=q.front(),now=dis[u];q.pop();
            if(u==n-1) return now;
            if(u+1<n&&dis[u+1]==inf){
                q.push(u+1);dis[u+1]=now+1;
            }
            if(u-1>=0&&dis[u-1]==inf){
                q.push(u-1);dis[u-1]=now+1;
            }
            for(int i=0;i<mp[a[u]].size();i++){
                if(dis[mp[a[u]][i]]==inf){
                    q.push(mp[a[u]][i]);
                    dis[mp[a[u]][i]]=now+1;
                }
            }
            //注意需要在map中清空当前值
            mp[a[u]].clear();
        }
        return -1;
    }
};
```

**Java代码：**

```java
class Solution {
	int inf = 0x3f3f3f3f;

	public int minJumps(int[] arr) {
		int n = arr.length;
		Map<Integer, List<Integer>> mp = new HashMap<>();
		// 初始化map
		for (int i = 0; i < n; i++) {
			List<Integer> list = mp.getOrDefault(arr[i], new ArrayList<>());
			list.add(i);
			mp.put(arr[i], list);
		}
		int dis[] = new int[n];
		Arrays.fill(dis, inf);
		ArrayDeque<Integer> q = new ArrayDeque();
		q.add(0);
		dis[0] = 0;
		while (!q.isEmpty()) {
			int u = q.poll();
			int now = dis[u];
			if (u == n - 1)
				return now;
			if (u + 1 < n && dis[u + 1] == inf) {
				q.add(u + 1);
				dis[u + 1] = now + 1;
			}
			if (u - 1 >= 0 && dis[u - 1] == inf) {
				q.add(u - 1);
				dis[u - 1] = now + 1;
			}
			List<Integer> list = mp.getOrDefault(arr[u], new ArrayList<>());
			for (int i : list) {
				if (dis[i] == inf) {
					q.add(i);
					dis[i] = now + 1;
				}
			}
			// 在map中清空当前值
			mp.getOrDefault(arr[u], new ArrayList<>()).clear();
		}
		return -1;
	}
}
```

### [1.22：删除回文子序列](https://leetcode-cn.com/problems/remove-palindromic-subsequences/)

#### 题意

给你一个字符串 s，它仅由字母 'a' 和 'b' 组成。每一次删除操作都可以从 s 中删除一个回文 子序列。

返回删除给定字符串中所有字符（字符串为空）的最小删除次数。

「子序列」定义：如果一个字符串可以通过删除原字符串某些字符而不改变原字符顺序得到，那么这个字符串就是原字符串的一个子序列。

「回文」定义：如果一个字符串向后和向前读是一致的，那么这个字符串就是一个回文。

**示例1：**

> 输入：s = "ababa"
> 输出：1
> 解释：字符串本身就是回文序列，只需要删除一次。

**示例2：**

> 输入：s = "abb"
> 输出：2
> 解释："abb" -> "bb" -> "". 
> 先删除回文子序列 "a"，然后再删除 "bb"。

**示例2：**

> 输入：s = "baabb"
> 输出：2
> 解释："baabb" -> "b" -> "". 
> 先删除回文子序列 "baab"，然后再删除 "b"。

**提示：**

- $1 <= s.length <= 1000$
- $s 仅包含字母 'a' 和 'b'$

#### 题解：简单思维

题目给定s只包含字母a和b，于是可以知道：

- 如果原串是回文串，则只需要一次操作即可
- 如果原串不是回文串，则最差的情况可以为先移除所有的a，再移除所有的b，只需要两次

于是只需判断原串是否为回文串即可。

**C++代码：**

```cpp
class Solution {
public:
    int removePalindromeSub(string s) {
        int n=s.length();
        for(int i=0;i<n;i++){
            if(s[i]!=s[n-i-1]) return 2;
        }
        return 1;
    }
};
```

**Java代码：**

```java
class Solution {
	public int removePalindromeSub(String s) {
		char[] str=s.toCharArray();
        for(int i=0;i<s.length();i++){
            if(str[i]!=str[s.length()-i-1]) return 2;
        }
        return 1;
    }
}
```

### [1.23：股票价格波动](https://leetcode-cn.com/problems/stock-price-fluctuation/)

#### 题意

给你一支股票价格的数据流。数据流中每一条记录包含一个 时间戳 和该时间点股票对应的 价格 。

不巧的是，由于股票市场内在的波动性，股票价格记录可能不是按时间顺序到来的。某些情况下，有的记录可能是错的。如果两个有相同时间戳的记录出现在数据流中，前一条记录视为错误记录，后出现的记录 更正 前一条错误的记录。

请你设计一个算法，实现：

- 更新 股票在某一时间戳的股票价格，如果有之前同一时间戳的价格，这一操作将 更正 之前的错误价格。
- 找到当前记录里 最新股票价格 。最新股票价格 定义为时间戳最晚的股票价格。
- 找到当前记录里股票的 最高价格 。
- 找到当前记录里股票的 最低价格 。

请你实现 StockPrice 类：

- StockPrice() 初始化对象，当前无股票价格记录。
- void update(int timestamp, int price) 在时间点 timestamp 更新股票价格为 price 。
- int current() 返回股票 最新价格 。
- int maximum() 返回股票 最高价格 。
- int minimum() 返回股票 最低价格 。

**提示：**

- $1 <= timestamp, price <= 10^9$
- update，current，maximum 和 minimum 总 调用次数不超过 $10^5$ 。
- current，maximum 和 minimum 被调用时，update 操作 至少 已经被调用过 一次 。

#### 题解：数据结构

直接数据结构模拟，具体看代码。（全依靠于高级程序设计语言实现，感觉题目没意思）

**C++代码：**

```cpp
class StockPrice {
public:
    int cur,curPrice;  //记录最近的时间戳以及对应的价格
    multiset<int> set;    //价格从小到大排序
    map<int,int> mp;    //记录各个时间戳和价格
    StockPrice() {
        set.clear();mp.clear();cur=0;
    }
    
    void update(int timestamp, int price) {
        if(cur<=timestamp){
            cur=timestamp;curPrice=price;
        }
        int pre_price=mp[timestamp];
        mp[timestamp]=price;
        if(pre_price>0){
            auto it=set.find(pre_price);
            if(it!=set.end()){
                set.erase(it);
            }
        }
        set.emplace(price);
    }
    
    int current() {
        return curPrice;
    }
    
    int maximum() {
        return *set.rbegin();
    }
    
    int minimum() {
        return *set.begin();
    }
};

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice* obj = new StockPrice();
 * obj->update(timestamp,price);
 * int param_2 = obj->current();
 * int param_3 = obj->maximum();
 * int param_4 = obj->minimum();
 */
```



**Java代码：**

```java
class StockPrice {
	int cur; // 记录最近的时间戳
	int curPrice;
	Map<Integer, Integer> mp1;
	TreeMap<Integer, Integer> mp2;

	public StockPrice() {
		mp1 = new HashMap<Integer, Integer>();
		mp2 = new TreeMap<Integer, Integer>();
        cur=0;
	}

	public void update(int timestamp, int price) {
		if (cur <= timestamp) {
			cur = timestamp;
			curPrice = price;
		}
		int pre_price = 0;
		if (mp1.containsKey(timestamp))
			pre_price = mp1.get(timestamp);
		mp1.put(timestamp, price);
		if (pre_price > 0) {
			mp2.put(pre_price, mp2.get(pre_price) - 1);
			if (mp2.get(pre_price) == 0) {
				mp2.remove(pre_price);
			}
		}
		mp2.put(price, mp2.getOrDefault(price, 0) + 1);
	}

	public int current() {
		return curPrice;
	}

	public int maximum() {
		return mp2.lastKey();
	}

	public int minimum() {
		return mp2.firstKey();
	}
}

/**
 * Your StockPrice object will be instantiated and called as such: StockPrice
 * obj = new StockPrice(); obj.update(timestamp,price); int param_2 =
 * obj.current(); int param_3 = obj.maximum(); int param_4 = obj.minimum();
 */
```

### [1.25：到达目的地的第二短时间](https://leetcode-cn.com/problems/second-minimum-time-to-reach-destination/)

#### 题意

城市用一个 双向连通 图表示，图中有 n 个节点，从 1 到 n 编号（包含 1 和 n）。图中的边用一个二维整数数组 edges 表示，其中每个 edges[i] = [ui, vi] 表示一条节点 ui 和节点 vi 之间的双向连通边。每组节点对由 最多一条 边连通，顶点不存在连接到自身的边。穿过任意一条边的时间是 time 分钟。

每个节点都有一个交通信号灯，每 change 分钟改变一次，从绿色变成红色，再由红色变成绿色，循环往复。所有信号灯都 同时 改变。你可以在 任何时候 进入某个节点，但是 只能 在节点 信号灯是绿色时 才能离开。如果信号灯是  绿色 ，你 不能 在节点等待，必须离开。

第二小的值 是 严格大于 最小值的所有值中最小的值。

例如，[2, 3, 4] 中第二小的值是 3 ，而 [2, 2, 4] 中第二小的值是 4 。
给你 n、edges、time 和 change ，返回从节点 1 到节点 n 需要的 第二短时间 。

注意：

- 你可以 任意次 穿过任意顶点，包括 1 和 n 。
- 你可以假设在 启程时 ，所有信号灯刚刚变成 绿色 。

**示例1：**

<img src="https://assets.leetcode.com/uploads/2021/09/29/e1.png" alt="img" style="zoom: 50%;" /><img src="https://assets.leetcode.com/uploads/2021/09/29/e2.png" alt="img" style="zoom: 50%;" />  

> 输入：n = 5, edges = [[1,2],[1,3],[1,4],[3,4],[4,5]], time = 3, change = 5
> 输出：13
> 解释：
> 上面的左图展现了给出的城市交通图。
> 右图中的蓝色路径是最短时间路径。
> 花费的时间是：
> - 从节点 1 开始，总花费时间=0
> - 1 -> 4：3 分钟，总花费时间=3
> - 4 -> 5：3 分钟，总花费时间=6
> 因此需要的最小时间是 6 分钟。
>
> 右图中的红色路径是第二短时间路径。
> - 从节点 1 开始，总花费时间=0
> - 1 -> 3：3 分钟，总花费时间=3
> - 3 -> 4：3 分钟，总花费时间=6
> - 在节点 4 等待 4 分钟，总花费时间=10
> - 4 -> 5：3 分钟，总花费时间=13
> 因此第二短时间是 13 分钟。   
>

#### 题解：dijkstra & bfs

次短路直接dijkstra，但是由于边权值和上次前几天那题一样都相同，于是bfs也可以

### [1.25：比赛中的配对次数](https://leetcode-cn.com/problems/count-of-matches-in-tournament/)

#### 题意

给你一个整数 n ，表示比赛中的队伍数。比赛遵循一种独特的赛制：

如果当前队伍数是 偶数 ，那么每支队伍都会与另一支队伍配对。总共进行 n / 2 场比赛，且产生 n / 2 支队伍进入下一轮。
如果当前队伍数为 奇数 ，那么将会随机轮空并晋级一支队伍，其余的队伍配对。总共进行 (n - 1) / 2 场比赛，且产生 (n - 1) / 2 + 1 支队伍进入下一轮。
返回在比赛中进行的配对次数，直到决出获胜队伍为止。

**提示：**

- `1 <= n <= 200`

**示例1：**

> 输入：n = 7
> 输出：6
> 解释：比赛详情：
> - 第 1 轮：队伍数 = 7 ，配对次数 = 3 ，4 支队伍晋级。
> - 第 2 轮：队伍数 = 4 ，配对次数 = 2 ，2 支队伍晋级。
> - 第 3 轮：队伍数 = 2 ，配对次数 = 1 ，决出 1 支获胜队伍。
> 总配对次数 = 3 + 2 + 1 = 6
>

**示例2：**

> 输入：n = 14
> 输出：13
> 解释：比赛详情：
> - 第 1 轮：队伍数 = 14 ，配对次数 = 7 ，7 支队伍晋级。
> - 第 2 轮：队伍数 = 7 ，配对次数 = 3 ，4 支队伍晋级。 
> - 第 3 轮：队伍数 = 4 ，配对次数 = 2 ，2 支队伍晋级。
> - 第 4 轮：队伍数 = 2 ，配对次数 = 1 ，决出 1 支获胜队伍。
> 总配对次数 = 7 + 3 + 2 + 1 = 13
>

#### 题解：模拟

由于n最多为200，于是直接模拟即可

```cpp
class Solution {
public:
    int numberOfMatches(int n) {
        int ans=0;
        while(n>1){
            ans+=n/2;
            if(n&1) n=n/2+1;
            else n/=2;
        }
        return ans;
    }
};
```



### [1.27：句子中的有效单词数](https://leetcode-cn.com/problems/number-of-valid-words-in-a-sentence/)

#### 题意

句子仅由小写字母（'a' 到 'z'）、数字（'0' 到 '9'）、连字符（'-'）、标点符号（'!'、'.' 和 ','）以及空格（' '）组成。每个句子可以根据空格分解成 一个或者多个 token ，这些 token 之间由一个或者多个空格 ' ' 分隔。

如果一个 token 同时满足下述条件，则认为这个 token 是一个有效单词：

仅由小写字母、连字符和/或标点（不含数字）。
至多一个 连字符 '-' 。如果存在，连字符两侧应当都存在小写字母（"a-b" 是一个有效单词，但 "-ab" 和 "ab-" 不是有效单词）。
至多一个 标点符号。如果存在，标点符号应当位于 token 的 末尾 。
这里给出几个有效单词的例子："a-b."、"afad"、"ba-c"、"a!" 和 "!" 。

给你一个字符串 sentence ，请你找出并返回 sentence 中 有效单词的数目 。

**提示：**

- 1 <= sentence.length <= 1000
- sentence 由小写英文字母、数字（0-9）、以及字符（' '、'-'、'!'、'.' 和 ','）组成
- 句子中至少有 1 个 token

**示例1：**

> 输入：sentence = "cat and  dog"
> 输出：3
> 解释：句子中的有效单词是 "cat"、"and" 和 "dog"

**示例2：**

> 输入：sentence = "!this  1-s b8d!"
> 输出：0
> 解释：句子中没有有效单词
> "!this" 不是有效单词，因为它以一个标点开头
> "1-s" 和 "b8d" 也不是有效单词，因为它们都包含数字
>

**示例3：**

> 输入：sentence = "alice and  bob are playing stone-game10"
> 输出：5
> 解释：句子中的有效单词是 "alice"、"and"、"bob"、"are" 和 "playing"
> "stone-game10" 不是有效单词，因为它含有数字
>

**示例4：**

> 输入：sentence = "he bought 2 pencils, 3 erasers, and 1  pencil-sharpener."
> 输出：6
> 解释：句子中的有效单词是 "he"、"bought"、"pencils,"、"erasers,"、"and" 和 "pencil-sharpener."

#### 题解

直接模拟即可。首先分割出子字符串，然后不是token的情况为：

- 连字符最多一个
- 连字符左右两边必须是字母
- 连字符不能在字符串首尾
- 不能有数字
- 最多一个标点符号且位于末尾

**C++代码：**

```cpp
class Solution {
public:
    int check(string s){
        int n=s.length();
        if(n==0) return 0;
        int cnt=0;
        for(int i=0;i<n;i++){
            if(s[i]=='-'){
                if(cnt==1) return 0;
                cnt=1;
                if(i==0||i==n-1) return 0;
                if(!(s[i-1]>='a'&&s[i-1]<='z')||!(s[i+1]>='a'&&s[i+1]<='z')) return 0;
            }else if((s[i]=='!'||s[i]==','||s[i]=='.')&&i!=(n-1)) return 0;
            else if(s[i]>='0'&&s[i]<='9') return 0;
        }
        return 1;
    }
    int countValidWords(string sentence) {
        int l=0,r=0,len=sentence.length();
        string s;
        int ans=0,flag=0;
        for(int i=0;i<len;i++){
            if(sentence[i]==' '){
                flag=1;
                r=i-1;
                if(l>r){
                    l=i+1;continue;
                }
                s=sentence.substr(l,r-l+1);
                // cout<<s<<endl;
                if(check(s)) ans++;
                l=i+1;
            }
        }
        //最后一个
        if(flag) s=sentence.substr(l,r-l+1);
        else s=sentence.substr(l,len);
        // cout<<s<<endl;
        if(check(s)) ans++;
        return ans;
    }
};
```

**Java代码：**

```java
class Solution {
	boolean check(String s) {
		char[] ss = s.toCharArray();
		int n = s.length();
        int cnt=0;
		for (int i = 0; i < n; i++) {
			if (ss[i] == '-') {
                if(cnt==1) return false;
                cnt=1;
				if (i == 0 || i == n - 1) {
					return false;
				} else if (!(ss[i - 1] >= 'a' && ss[i - 1] <= 'z') || !(ss[i + 1] >= 'a' && ss[i + 1] <= 'z')) {
					return false;
				}
			} else if (ss[i] >= '0' && ss[i] <= '9') {
				return false;
			}else if((ss[i]=='!'||ss[i]=='.'||ss[i]==',')&&i!=n-1){
				return false;
			}
		}
		return true;
	}

	public int countValidWords(String sentence) {
		int ans = 0;
		String s[] = sentence.split(" ");
		for (String str : s) {
			// System.out.println(str + " " + str.length());
			if (str.length() == 0)
				continue;
			if (check(str))
				ans++;
		}
		return ans;
	}
}
```


---
title: LeetCode2022年每日一题4月打卡汇总
categories: 'LeetCode每日一题打卡'
tags: 
  - LeetCode
  - Algorithm
katex: true

---

## LeetCode2022年每日一题4月打卡汇总

> 点击题目即可跳转至LeetCode题目

### [4.1：二倍数对数组](https://leetcode-cn.com/problems/array-of-doubled-pairs/)

给定一个长度为偶数的整数数组 arr，只有对 arr 进行重组后可以满足 “对于每个 0 <= i < len(arr) / 2，都有 arr[2 * i + 1] = 2 * arr[2 * i]” 时，返回 true；否则，返回 false。

**示例1：**

```
输入：arr = [3,1,3,6]
输出：false
```

**示例2：**

```
输入：arr = [2,1,2,6]
输出：false
```

**示例3：**

```
输入：arr = [4,-2,2,-4]
输出：true
解释：可以用 [-2,-4] 和 [2,4] 这两组组成 [-2,-4,2,4] 或是 [2,4,-2,-4]
```

**提示：**

- $0 <= arr.length <= 3 * 10^4$
- `arr.length` 是偶数
- $-10^5 <= arr[i] <= 10^5$

#### 题解：哈希+排序

首先将数组中所有元素出现的次数存入哈希表中，通过题意我们不难发现，只需要判断下标为奇数（下标从0开始）的数是前面数字的两倍即可。

于是自定义排序：按照绝对值的大小从大到小排序

遍历排好序的数组，只需判断当前数字是否还存在（可能是前面数字的两倍）以及它乘以两倍的数字是否存在即可。

如果存在n/2对数对，则返回true，否则返回false。

注意：需要特判下0

**C++代码：**

```cpp
class Solution {
public:
    static bool cmp(int x,int y){
        return abs(x)<abs(y);
    }
    bool canReorderDoubled(vector<int>& arr) {
        int n=arr.size();
        map<int,int> mp;
        for(int i=0;i<n;i++) mp[arr[i]]++;
        sort(arr.begin(),arr.end(),cmp);
        int cnt=0;
        for(int i=0;i<n;i++){
            if(mp[arr[i]]==0||mp[2*arr[i]]==0) continue;
            if(arr[i]==0&&mp[arr[i]]<2) continue;
            cnt++;
            --mp[arr[i]];--mp[2*arr[i]];
        }
        if(cnt<n/2) return false;
        return true;
    }
};
```

### [4.3：寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

给你一个排序后的字符列表 letters ，列表中只包含小写英文字母。另给出一个目标字母 target，请你寻找在这一有序列表里比目标字母大的最小字母。

在比较时，字母是依序循环出现的。举个例子：

如果目标字母 target = 'z' 并且字符列表为 letters = ['a', 'b']，则答案返回 'a'

**示例1：**

```
输入: letters = ["c", "f", "j"]，target = "a"
输出: "c"
```

**示例2：**

```
输入: letters = ["c","f","j"], target = "c"
输出: "f"
```

**示例3：**

```
输入: letters = ["c","f","j"], target = "d"
输出: "f"
```

**提示：**

- $2 <= letters.length <= 10^4$
- letters[i] 是一个小写字母
- letters 按非递减顺序排序
- letters 最少包含两个不同的字母
- target 是一个小写字母

#### 题解：模拟

直接遍历列表，判断是否存在大于target的字母，若不存在则输出列表中第一个字符即可。

```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int n=letters.size();
        for(int i=0;i<n;i++){
            if(letters[i]>target) return letters[i];
        }
        return letters[0];
    }
};
```

### [4.11：统计各位数字都不同的数字个数](https://leetcode-cn.com/problems/count-numbers-with-unique-digits/)

#### 题解一：暴力DFS

n最大为8，比较容易想到的思路便是直接通过dfs遍历每一位数字，如果各位数字都不同则记录数+1，最终统计满足条件的个数即可。

注意：前导0的情况需要特判下。

缺点：费时

**C++代码：**

```cpp
class Solution {
public:
    map<int,int> mp;
    int ans;
    int check(){
        for(int i=1;i<10;i++) if(mp[i]) return 0;
        return 1;
    }
    void dfs(int cnt,int  n){
        if(cnt>n){
            ++ans;return ;
        }
        for(int i=0;i<10;i++){
            if(mp[i]) continue;
            if(check()&&i==0){ //前面全是前导0
                dfs(cnt+1,n);
            }else{
                mp[i]++;
                dfs(cnt+1,n);
                mp[i]--;
            }
        }
    }
    int countNumbersWithUniqueDigits(int n) {
        mp.clear();
        if(n==1) return 10;
        ans=0;
        dfs(1,n);
        return ans;
    }
};
```

#### 题解二：排列组合

官方提供的思路。

- n=0时：$0<=x<1$，满足条件个数为：1个
- n=1时：$0<=x<10$，满足条件个数为：0~9，10个

- n=2时：$0<=x<100$，可以由两部分构成：只有一位数的情况和两位数的情况。其中
  - 只有一位数的情况由上述可得x有10种；
  - 有两位数的情况：第一位可以取1~9（9种）， 第二位可以取0~9除了和第一位不同的数字（9种），于是就有：9*9=81种。
- n=3时，可以分为三部分：一位数、两位数、三位数
  - 一位数：10种（0~9）
  - 两位数：9*9=81种
  - 三位数：9\*9\*8
- 扩展至n>=2：含有n位数字的情况，可以表示为：9*$A_{9}^{n-1}$

于是代码如下：

**C++代码：**

```cpp
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        if(n==0) return 1;
        else if(n==1) return 10;
        int ans=10,num=9;
        for(int i=1;i<=n-1;i++){
            num*=(9-i+1);   //9*9*8*....
            ans+=num;
        }
        return ans;
    }
};
```

当然，还有其他的解法，比如：n最大为8，可以直接打表（存储n为0~8的结果）；还可以用dp等等，此处并不一一列举。

### [4.12：写字符串需要的行数](https://leetcode-cn.com/problems/number-of-lines-to-write-string/)

#### 题解：模拟

直接根据题意模拟即可，如果当前行已有的宽度+当前遍历字符的宽度>100，则直接换行即可。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> numberOfLines(vector<int>& widths, string s) {
        int n=s.length(),num=0,cnt=1;
        for(int i=0;i<n;i++){
            if(num+widths[s[i]-'a']>100){
                ++cnt;num=widths[s[i]-'a'];
            }else{
                num+=widths[s[i]-'a'];
            }
        }
        vector<int> ans(2);ans[0]=cnt;ans[1]=num;
        return ans;
    }
};
```

### [4.13：O(1) 时间插入、删除和获取随机元素](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/)

#### 题解：哈希表

对于获取随机元素，数组可以在O(1)时间做到，但是对于插入和删除操作，数组却无能为力。

对于插入和删除操作，哈希表可以在O(1)时间做到，但是对于获取随机元素操作，哈希表却又不可奈何。

于是思考：能否将两者结合，实现O(1)时间：插入、删除和获取随机元素。

- 用一可变数组num：存储集合中的元素
- 哈希表中：key存储num中的元素，value存储对应的索引

当执行插入操作（插入元素在集合中未出现）时：

1. num数组直接在末尾添加
2. 哈希表则把插入的元素和对应的末尾下标存入其中

当执行删除操作（待删除元素在集合中）时：

1. 将num数组中末尾元素移动至当前需要删除元素的索引处
2. 在哈希表中更新num数组中末尾对应的value值（即索引）
3. num数组删除最后一个元素，哈希表删除待删除的元素

借此巩固下Java集合类的使用，于是提供C++和Java代码：

**C++代码：**

```cpp
class RandomizedSet {
public:
    unordered_map<int,int> mp;   //key->num,value->index
    vector<int>  num;
    RandomizedSet() {
        mp.clear();num.clear();
        srand(time(0));   //随机数种子
    }
    
    bool insert(int val) {
        if(mp.count(val)) return false;
        num.push_back(val);
        mp[val]=num.size()-1;
        return true;
    }
    
    bool remove(int val) {
        if(!mp.count(val)) return false;
        //将num数组中最后一个元素填入到此位置
        int index=mp[val];
        int x=num.back();
        mp[x]=index;
        num[index]=x;
        //在num数组中删除最后一个元素，同时mp中也删除需要删除的元素
        num.pop_back();mp.erase(val);
        return true;
    }
    
    int getRandom() {
        int pos=rand()%num.size();
        return num[pos];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

**Java代码：**

```java
class RandomizedSet {
    Map<Integer,Integer> map;
    List<Integer> num;
    public RandomizedSet() {
        map=new HashMap<Integer,Integer>();
        num=new ArrayList<>();
    }
    
    public boolean insert(int val) {
        if(map.containsKey(val)) return false;
        num.add(val);
        map.put(val,num.size()-1);
        return true;
    }
    
    public boolean remove(int val) {
        if(!map.containsKey(val)) return false;
        //将num最后一个元素移动至当前位置
        int index=map.get(val);
        int last=num.get(num.size()-1);
        num.set(index,last);
        map.put(last,index);
        //删除num最后一个元素，删除map中需要删除的元素
        num.remove(num.size()-1);
        map.remove(val);
        return true;
    }
    
    public int getRandom() {
        Random rand=new Random();
    	int index=rand.nextInt(num.size());
    	return num.get(index);
    }
}
```

### [4.14：最富有客户的资产总量](https://leetcode-cn.com/problems/richest-customer-wealth/)

#### 题解：模拟

直接按照题意，计算每位客户的资产总量，在选出最大的资产总量即可。

**C++代码：**

```cpp
class Solution {
public:
    int maximumWealth(vector<vector<int>>& accounts) {
        int ans=0,num=0;
        for(int i=0;i<accounts.size();i++){
            num=0;
            for(int j=0;j<accounts[i].size();j++){
                num+=accounts[i][j];
            }
            ans=max(ans,num);
        }
        return ans;
    }
};
```

### [4.15：迷你语法分析器](https://leetcode-cn.com/problems/mini-parser/)

#### 题解：dfs或栈

今天题目可能有点小复杂，但难度不大，可以理解为模拟。

可以使用dfs和栈来实现，dfs思路比较简单，但是相比于栈比较复杂，于是在此用栈来模拟实现。

栈中元素保存的是NestedInteger对象。

每一个NestedInteger对象存在两个元素：整数 或 列表

- 当出现 $[$ 字符时，则表示创建新的NestedInteger对象
- 当出现 $,$ 或 $]$ 字符时，表示要创建新的元素或对象
  - 如果是 $,$ 则表示此时是一个列表，把上一个整数添加至栈顶元素的列表中
  - 如果是 $]$ ，则此时NestedInteger对象已经结束，添加至栈顶

最后只需要返回栈顶元素即可。具体见代码。

注意：如果只有一个整数表示的情况，如示例1，则需要特判下。

**C++代码**

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    NestedInteger deserialize(string s) {
        if(s[0]!='['){  //只有一个数字的情况，如示例1
            return NestedInteger(stoi(s));   //stoi()：c++内置string to int函数
        }
        stack<NestedInteger> sta;
        int n=s.length();
        bool flag=0;   //记录正负
        int num=0;
        for(int i=0;i<n;i++){
            if(s[i]=='-') flag=1;
            else if(isdigit(s[i])){  // c++内置判断是否为数字函数
                num=num*10+(s[i]-'0');
            }else if(s[i]=='['){   //创建一个新的对象
                sta.push(NestedInteger());
            }else if(s[i]==','||s[i]==']'){   //新的元素或对象
                if(isdigit(s[i-1])){
                    if(flag) num=-num;
                    sta.top().add(NestedInteger(num));
                }
                num=0;flag=0;
                if(s[i]==']'&&sta.size()>1){
                    NestedInteger ni=sta.top();
                    sta.pop();
                    sta.top().add(ni);
                }
            }
        }
        return sta.top();
    }
};
```

### [4.16：最大回文数乘积](https://leetcode-cn.com/problems/largest-palindrome-product/)

#### 题解：数学+枚举

由于需要计算两位n位整数乘积的**最大回文整数**。于是可以直接枚举所有的回文数（由于是求最大值，所有从大到小枚举）

两位n位整数最多也只能产生$10^{2n}$位的数字。又由于是回文数，我们可以只枚举一半，于是可以从$10^n-1$开始枚举回文数的一半。

然后再遍历它的因子，如果两个因子都是n位整数，则找到答案，即可结束循环。

**C++代码：**

```cpp
class Solution {
public:
    int largestPalindrome(int n) {
        if(n==1) return 9;
        int ans=0;
        //枚举回文数左边
        for(int i=pow(10,n)-1;i>pow(10,n-1);i--){
            int num=i;long long p=i;   //p表示回文数
            while(num){
                p=p*10+num%10;num/=10;
            }
            
            //遍历它的因子
            for(long long j=pow(10,n)-1;j*j>=p;j--){
                if(p%j==0&&(j>pow(10,n-1))){
                    ans=p%1337;break;
                }
            }
            if(ans!=0) break;
        }
        return ans;
    }
};
```

### [4.17：最常见的单词](https://leetcode-cn.com/problems/most-common-word/)

#### 题解：模拟

1. 首先将所有字母转化为小写字母
2. 截取段落中所有的单词存储到一个哈希表中，key存储单词，value存储单词出现的次数
3. 在哈希表中找出出现次数最多，且不在禁用列表中的单词

**注意：截取段落中的所有单词可能会存在一些小细节，需要留意下（例如最后一个单词末尾可能是字母或者是其他字符等）**

**C++代码：**

```cpp
class Solution {
public:
    char s[7]={' ','!','?','\'',',',';','.'};
    int get_minpos(string paragraph,int pos){
        int n=paragraph.size();
        int res=n-1;
        for(int i=pos;i<n;i++){
            for(int j=0;j<7;j++){
                if(paragraph[i]==s[j]){
                    res=i;break;
                }
            }
            if(res!=n-1) break;
        }
        return res;
    }
    string mostCommonWord(string paragraph, vector<string>& banned) {
        int n=paragraph.size();
        //先将字符串中所有字母转化为小写
        for(int i=0;i<n;i++){
            if(paragraph[i]>='A'&&paragraph[i]<='Z') paragraph[i]+=32;
        }
        // cout<<paragraph<<endl;
        map<string,int> mp;
        int pos=0;
        while(pos<n-1){
            int p=get_minpos(paragraph,pos);
            string str;
            if(paragraph[p]>='a'&&paragraph[p]<='z') str=paragraph.substr(pos,p-pos+1);
            else str=paragraph.substr(pos,p-pos);
            // cout<<pos<<"--"<<p<<"---"<<str<<endl;
            if(str!="") mp[str]++;
            pos=p+1;
        }
        int maxnum=0;string ans;
        for(auto it:mp){
            if(count(banned.begin(),banned.end(),it.first)) continue;
            if(it.second>maxnum){
                maxnum=it.second;ans=it.first;
            }
        }
        return ans;
    }
};
```

### [4.18：字典序排数](https://leetcode-cn.com/problems/lexicographical-numbers/)

#### 题解：DFS

题目要求按照字典序返回[1,n]中的所有数字。时间复杂度要求为$O(n)$，空间复杂度要求为：$O(1)$

例如：n=13时，输出为：[1,10,11,12,13,2,3,4,5,6,7,8,9]。

思考能否直接从1开始遍历出第一位数字为1且小于等于n的所有数字；然后再遍历2,3,...9。这样便可在$O(n)$时间复杂度条件下实现题目要求。

显然dfs可以实现上述思路：

- 最高位从1到9
- 次高为从1到9
- ....

期间如果遍历的数字超过了n则直接结果此轮的dfs。

代码如下：

**C++代码：**

```cpp
class Solution {
public:
    int cnt,num;
    vector<int> ans;
    void dfs(int now,int n){
        if(now>n||cnt>n) return ;
        ++cnt;ans.push_back(now);
        for(int i=0;i<=9;i++){
            dfs(now*10+i,n);
        }
    }
    vector<int> lexicalOrder(int n) {
        cnt=1,num=1;
        for(int i=1;i<=9;i++){
            dfs(i,n);
        }
        return ans;
    }
};
```

### [ 4.19：字符的最短距离](https://leetcode-cn.com/problems/shortest-distance-to-a-character/)

#### 题解一：暴力

首先将字符串s中所有字符c的位置存入数组中，然后遍历字符串s，假设当前遍历位置为i，再二重循环遍历所有字符c的位置，找出两个下标之间的最小距离。

时间复杂度：$O(n^2)$

**C++代码：**

```cpp
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        vector<int> pos_arr;
        int n=s.length();
        for(int i=0;i<n;i++){
            if(s[i]==c) pos_arr.push_back(i);
        }
        vector<int> ans(n,0x3f3f3f3f);
        for(int i=0;i<n;i++){
            for(auto j:pos_arr){
                ans[i]=min(ans[i],abs(i-j));
            }
        }
        return ans;
    }
};
```

#### 题解二：两次遍历

- 第一次遍历，从左到右遍历，找出s[i]到左侧最近的字符c的距离
- 第二次遍历，从右到左遍历，找出s[i]到右侧最近的字符c的距离

然后选择其中的最小值即可。

注意：可能存在此时并没出现过字符c的情况（例如：从左到右遍历时，左侧并没有字符c，从右到左遍历同理）所以需要初始化一个最大值，为方便处理：

- 从左开始遍历时，我们可以初始字符c的位置为：$pos=-n$，如果未出现则取：i-pos>=n，显然可行
- 从右开始遍历时，我们可以初始字符c的位置为：$pos=2n$，如果未出现则取：pos-i>n，显然可行

当然也可以直接初始化非常大的值。

**C++代码：**

```cpp
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        int n=s.length();
        vector<int> ans(n);
        int pos=-n;
        for(int i=0;i<n;i++){
            if(s[i]==c) pos=i;
            ans[i]=i-pos;
        }
        pos=2*n;
        for(int i=n-1;i>=0;i--){
            if(s[i]==c) pos=i;
            ans[i]=min(ans[i],pos-i);
        }
        return ans;
    }
};
```

### [4.20：文件的最长绝对路径](https://leetcode-cn.com/problems/longest-absolute-file-path/)

#### 题解：模拟

**注意**：题目给出的换行符$\n$和制表符$\t$，都只是字符，而不是由两个字符组成。

上述理解后，这题就不难了，对于每一个文件file：

- 要么是由上一层的文件夹所得到，此时路径长度为：文件名长度+上一层路径长度+1（表示/）
- 要么直接为根目录下的文件，此时路径长度直接为：文件名长度

判断当前遍历的为文件还是文件夹 ，于是只需要判断是否存在字符 $.$ 即可。

又由于只需要计算文件的最长绝对路径，于是我们只需要存储每一层的绝对路径的长度，然后按照上述规则不断更新结结果即可。

具体见代码注释。

**C++代码：**

```cpp
class Solution {
public:
    int lengthLongestPath(string input) {
        int n=input.length();
        int ans=0;
        int pos=0;  //遍历字符串
        int dep;   //遍历文件的层数
        vector<int> f(n+1);  //记录每一层的路径长度
        while(pos<n){
            //当前层数
            dep=1;
            while(pos<n&&input[pos]=='\t'){
                pos++;dep++;
            }
            //记录当前文件/文件夹的长度
            int len=0;
            bool flag=false; //判断是否为文件
            while(pos<n&&input[pos]!='\n'){
                if(input[pos]=='.') flag=true;
                pos++;len++;
            }
            // 不是根目录
            if(dep>1) len+=f[dep-1]+1;
            if(flag){ //is file
                ans=max(ans,len);
            }else{ //is folder
                f[dep]=len;
            }
            //跳过换行符
            pos++;
        }
        return ans;
    }
};
```

### [4.21：山羊拉丁文](https://leetcode-cn.com/problems/goat-latin/)

#### 题解：模拟

直接按照题意模拟即可。通过一次遍历便可按照题意输出想要的结果

- 元音字母包含大小写10个
- 如果此时遍历的是单词的第一个字母，且为辅音字母，则直接跳过（因为该字母需要加入到单词末尾）
- 如果此时遍历的空格，则表示此时已经遍历了一个单词，按照题目要求进行相应的操作（如果单词第一个字母为辅音字母则加入到末尾；然后加入对应数量的字母a）

可以发现，这样处理对于最后一个单词不是很友好，因为末尾没有空格，为了方便处理，可以在末尾加入一个空格，直接一步到位。

官方题解时间复杂度为$O(n^2)$，此思路时间复杂度为$O(n)$

**C++代码：**

```cpp
class Solution {
public:
    string str="aeiouAEIOU";
    bool check(char c){  
        for(int i=0;i<10;i++) if(c==str[i]) return true;
        return false;
    }
    string toGoatLatin(string s) {
        //为了方便处理最后一个单词，所以在字符串末尾添加' '
        s+=' ';
        int n=s.length();
        string ans="";

        int cnt=0; //记录单词在句子中的索引
        int pos=0;  //记录当前遍历的单词第一个字母位置
        for(int i=0;i<n;i++){
            if(i==pos && !check(s[i])){//单词以辅音字母开头
                continue;
            }
            if(s[i]==' '){
                ++cnt;
                if(!check(s[pos])) ans+=s[pos];    //如果第一个字母是辅音字母则加入到末尾
                ans+="ma";
                for(int j=0;j<cnt;j++) ans+='a';

                pos=i+1;   //更新pos
            }
            if(i!=n-1) ans+=s[i];
        }

        return ans;
    }
};
```

### [4.22：旋转函数](https://leetcode-cn.com/problems/rotate-function/)

#### 题解：迭代+思维

通过具体示例进行讲解：例如nums = [4, 3, 2, 6]

```
F[0] = (0*4) + (1*3) + (2*2) + (3*6)
F[1] = (1*4) + (2*3) + (3*2) + (0*6) = F[0] + sum - n*nums[n-1]（其中sum表示数组nums所有元素总和）
F[2] = (2*4) + (3*3) + (0*2) + (1*6) = F[1] + sum - n*nums[n-2]
```

进而推广至$k(1<k<n)$：$F[k]=f[k-1]+sum-n*nums[n-k]$

从F[]数组中选择最大值即为答案。

**C++代码**

```cpp
class Solution {
public:
    int maxRotateFunction(vector<int>& nums) {
        int n=nums.size();
        //数组元素的和
        int sum=0;
        for(int i=0;i<n;i++) sum+=nums[i];
        vector<int> f(n);
        for(int i=1;i<n;i++) f[0]+=i*nums[i];
        int ans=f[0];
        for(int k=1;k<n;k++){
            f[k]=f[k-1]+sum-n*nums[n-k];
            ans=max(ans,f[k]);
        }

        return ans;
    }
};
```

当然可以通过变量存储每一次旋转函数的值，从而降低空间复杂度。

### [4.25：随机数索引](https://leetcode-cn.com/problems/random-pick-index/)

#### 题解：哈希表

题目给定数组长度最大为$2*10^4$，同时最多调用pick函数$10^4$次，于是可以直接通过哈希表存储数组中的元素对应的下标，即：

- key为数组元素
- value为数组元素对应下标的列表

对于随机数，直接使用C++ rand()函数即可。

**C++代码：**

```cpp
class Solution {
public:
    map<int,vector<int> > mp;
    Solution(vector<int>& nums) {
        srand(time(0));
        int n=nums.size();
        for(int i=0;i<n;i++){
            mp[nums[i]].push_back(i);
        }
    }
    
    int pick(int target) {
        int n=mp[target].size();
        return mp[target][rand()%n];
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * int param_1 = obj->pick(target);
 */
```


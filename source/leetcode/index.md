---
title: Leetcode
date: 2021-04-11 19:01:14
---

Leetcode Problem


# 264. 丑数II #
[https://leetcode-cn.com/problems/ugly-number-ii/](https://leetcode-cn.com/problems/ugly-number-ii/)

找到第n个抽数，只含2，3，5的正整数。


有点类似于素数筛，先排序，找到对应的下一个丑数将其push到数组中。查看是哪个指针提供了素材，将指针后移，寻找更大的新的丑数。


	class Solution {
	public:
	    int nthUglyNumber(int n) {
	        vector<int> dp(n);
	        int x = 0, y = 0, z = 0;
	        dp[0] = 1;
	        for(int i = 1; i < n; ++ i){
	            dp[i] = min(min(dp[x] * 2, dp[y] * 3), dp[z] * 5);
	            if(dp[i] == dp[x] * 2) ++ x;
	            if(dp[i] == dp[y] * 3) ++ y;
	            if(dp[i] == dp[z] * 5) ++ z;
	        }
	        return dp[n-1];
	    }
	};

# 313.超级丑数 #

[https://leetcode-cn.com/problems/super-ugly-number/](https://leetcode-cn.com/problems/super-ugly-number/)

类比丑数II。方法和思想是一样的。

	class Solution {
	public:
	    int nthSuperUglyNumber(int n, vector<int>& p) {
	        if(n == 1)return 1;
	        vector<int> dp(n), idx(p.size());
	        dp[0] = 1;
	        for(int i = 1;i < n; ++ i){
	            int m = INT_MAX ;
	            for(int j = 0; j < p.size(); ++ j){
	                m = min(m, dp[idx[j]] * p[j]);
	            }
	            dp[i] = m;
	            for(int j = 0; j < p.size(); ++ j){
	                if(m == dp[idx[j]] * p[j]) ++ idx[j];
	            }
	        }
	        return dp[n-1];
	    }
	};

# 279.完全平方数 #

给一个整数n，求最少多少个完全平方数（1，4，9，16这种）可以求和出n？

DP
令dp[n]表示可以用最少数量求和出整数n。
边界条件：dp[1] = 1, dp[0] = 0;
初始化：dp[i] = i;（全为1）

	dp[i] = min(dp[i], dp[i - j*j]),  0<=j*j<=i

代码：

	class Solution {
	public:
	    int numSquares(int n) {
	        vector<int> dp(n+1);
	        dp[1] = 1;
	        for(int i = 2; i <= n; ++ i){
	            dp[i] = i;
	            for(int j = 0; j * j <= i; ++ j){
	                dp[i] = min(dp[i], dp[i - j * j] + 1);
	            }
	        }
	        return dp[n];
	    }
	};

# 252.会议室1 #
> 给定一个会议时间安排的数组 intervals ，每个会议时间都会包括开始和结束的时间 intervals[i] = [starti, endi] ，请你判断一个人是否能够参加这里面的全部会议。

直接排序后扫一遍。

		class Solution {
		public:
		    bool canAttendMeetings(vector<vector<int>>& t) {
		        sort(t.begin(), t.end());
		        if(t.size() == 0)return true;
		
		        int end = t[0][1];
		        for(int i = 1; i < t.size(); ++ i){
		            if(t[i][0] < end){
		                return false;
		            }
		            end = max(t[i][1], end);
		        }
		        return true;
		    }
	};

# 253.会议室2 #
[https://leetcode-cn.com/problems/meeting-rooms-ii/](https://leetcode-cn.com/problems/meeting-rooms-ii/)
> 
> 给你一个会议时间安排的数组 intervals ，每个会议时间都会包括开始和结束的时间 intervals[i] = [starti, endi] ，为避免会议冲突，同时要考虑充分利用会议室资源，请你计算至少需要多少间会议室，才能满足这些会议安排。


1. 优先队列。可以解决。首先，先按开始时间排序，维护一个结束时间的最小堆。如果，当前堆为空，则把结束时间放进来。如果堆顶的时间大于开始的时间，则表示需要一个新的房间，并把新的结束时间添加进堆里。如果堆顶的时间小于开始的时间，则表示，堆内一个“房间”空余了，pop出来后，把新的会议安排进这个房间。
2. 可以把开始时间和结束时间分别来看。加入一个结束时间，如果开始时间小于结束时间，表示要多一个房间。
## 用优先队列 ##：


	class Solution {
	public:
	    int minMeetingRooms(vector<vector<int>>& t) {
	        priority_queue<int, vector<int>, greater<int> > pq;
	        sort(t.begin(), t.end());
	        int m = 0;
	        for(auto i : t){
	            while( !pq.empty() && pq.top() <= i[0])
	                pq.pop();
	            
	            pq.push(i[1]);
	            m = max((int)pq.size(), m);
	        }
	        return m;
	    }
	};
## 贪心 ##

	class Solution {
	public:
	    int minMeetingRooms(vector<vector<int>>& t) {
	        int n = t.size();
	        vector<int> s(n),e(n);
	        for(int i = 0; i < n; ++ i){
	            s[i] = t[i][0];
	            e[i] = t[i][1];
	        }
	        sort(s.begin(), s.end());
	        sort(e.begin(), e.end());
	        int i = 0,j = 0,ans = 0;
	        while(i < n){
	            if(s[i] < e[j])
	                ans ++;
	            else
	                ++ j;
	            ++ i;      
	        }
	        return ans;
	    }
	};

# 986.区间列表的交集 #
[https://leetcode-cn.com/problems/interval-list-intersections/](https://leetcode-cn.com/problems/interval-list-intersections/)
> 给定两个由一些 闭区间 组成的列表，firstList 和 secondList ，其中 firstList[i] = [starti, endi] 而 secondList[j] = [startj, endj] 。每个区间列表都是成对 不相交 的，并且 已经排序 。
> 
> 返回这 两个区间列表的交集 。
> 
> 形式上，闭区间 [a, b]（其中 a <= b）表示实数 x 的集合，而 a <= x <= b 。
> 
> 两个闭区间的 交集 是一组实数，要么为空集，要么为闭区间。例如，[1, 3] 和 [2, 4] 的交集为 [2, 3] 。

有点类似于用两个指针，分别从firstlist和secondlist中对应获取值。
比较是否有重合，有重合就拉入ans中。
之后判断怎么往下一个。这里可以发现，通过比较end，end后与该区间就无关了。所以把end小的，往下移动。
代码：

	class Solution {
	public:
	    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
	        int n = firstList.size(), m = secondList.size();
	        vector<vector<int>> ans;
	        if( !n || !m )return ans;
	        int i = 0, j = 0;
	        while(i < n && j < m){
	            auto l = firstList[i] ;
	            auto r = secondList[j] ;
	
	            int x = max(l[0],r[0]);
	            int y = min(l[1],r[1]);
	            if(x <= y)ans.push_back({x,y});
	   
	            if(l[1] == r[1]){
	                i++;j++;
	            }
	            else if(l[1] < r[1])i++;
	            else j++;
	        }
	        return ans;
	    }
	};


# 763.划分字母区间 #
**TAG：区间调度**
[https://leetcode-cn.com/problems/partition-labels/](https://leetcode-cn.com/problems/partition-labels/)

> 字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

## nlogn ##
最初的想法，和每个字母第一次出现的下标和最后一次出现的下标有关。那么就转换成了区间问题。先扫一遍，获取每个字母出现的对应位置，然后排序后，存储在ans里面，再扫一遍获取插值。

	class Solution {
	public:
	    vector<int> partitionLabels(string s) {
	        vector< vector<int> > a(26,vector<int>(2,-1));
	        for(int i = 0; i < s.size(); ++ i){
	            int t = s[i] - 'a';
	            if(a[t][0] == -1){
	                a[t][0] = i;
	            }
	            a[t][1] = max(a[t][1], i);
	        }
	        sort(a.begin(), a.end());
	        vector< vector<int> > ans;
	        vector<int> ret;
	        for(int i = 0; i < 26; ++ i){
	            if(a[i][0] == -1)continue;
	            if( ans.size() &&  ans.back()[1] > a[i][0] ){
	                ans.back()[1] = max(ans.back()[1], a[i][1]);
	            }else{
	                ans.push_back({a[i][0], a[i][1]});
	            }
	        }
	        for(auto i : ans){
	            
	            ret.push_back(i[1] - i[0] + 1);
	        }
	        return ret;
	    }
	};

## n ##
其实可以不用，类似跳跃的贪心做法，每次维护end值，当遍历i到达end值时，即一个区间完成。更新start和end。

	class Solution {
	public:
	    vector<int> partitionLabels(string s) {
	       
	        vector<int> a(26);
	        for(int i = 0; i < s.size(); ++ i)
	             a[s[i] - 'a'] = i;
	
	        vector<int> ret; 
	        int start = 0, end = 0;
	        for(int i = 0; i < s.size(); ++ i){
	            end = max(end, a[ s[i] - 'a']);
	            if(i == end){
	                ret.push_back(end - start + 1);
	                start = end + 1;
	                continue;
	            }        
	        }
	        return ret;
	    }
	};


# 395. 至少有 K 个重复字符的最长子串 #
[https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/](https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/)

> 给你一个字符串 s 和一个整数 k ，请你找出 s 中的最长子串， 要求该子串中的每一字符出现次数都不少于 k 。返回这一子串的长度。

可以枚举字符类型的数量。这样可以使用滑动窗口。右坐标往右走的字符类型数量一定会大于等于当前情况，左坐标往右走的字符类型数量一定小于等于当前的情况。

	class Solution {
	public:
	    int longestSubstring(string s, int k) {
	        int n = s.size(),ans = 0;
	        for(int l = 1; l <= 26; ++ l){
	            vector<int> vis(26,0);
	            int tot = 0,diff = 0;
	            int i = 0,j = 0;
	            while(i < n){
	                int x = s[i] - 'a';
	                vis[ x ]++;
	                if(vis[ x ]  == 1){
	                    tot ++;
	                    diff++;
	                }
	                if(vis[x] == k){
	                    diff--;
	                }
	                while(tot > l){
	                    int y = s[j] - 'a';
	                    vis[ y ]--;
	                    if(vis[y] == 0){
	                        tot--;
	                        diff--;
	                    }
	                    if(vis[y] == k-1)diff++;
	                    ++ j;
	
	                }
	                ++ i;
	                if(diff == 0)ans = max(ans, i-j);
	            }
	        }
	        return ans;
	    }
	};

# 1360.日期之间隔几天 #
[https://leetcode-cn.com/problems/number-of-days-between-two-dates/](https://leetcode-cn.com/problems/number-of-days-between-two-dates/)

> 请你编写一个程序来计算两个日期之间隔了多少天。
> 
> 日期以字符串形式给出，格式为 YYYY-MM-DD，如示例所示。

一个很直接的做法，先获取日期靠后的那一段。算出该如期到当年1月1日有多少天，用同样的方法，求出日期考前有多少天，用当年的总日期（365/366）减去即为到下一年有多少天。再算出中间有几年有多少天。累加起来。

## 简单模拟 ##

	class Solution {
	public:
	
	    vector<int> dict = {31,28,31,30,31,30,31,31,30,31,30,31};
	    int pr(int y){
	        return (y % 400 == 0 || (y % 100 && y % 4 == 0));
	    }
	    int getDay(int y, int m, int d){
	        int ans = 0;
	        for(int i = 1; i < m; ++ i){
	            ans += dict[i-1];
	            if(i == 2 && pr(y))ans += 1;
	        }
	        ans += d;
	        return ans;
	    }
	    int big(int y1,int y2,int m1,int m2,int d1,int d2){
	        if(y1 > y2)return 1;
	        if(y1 < y2)return 0;
	        if(m1 > m2)return 1;
	        if(m1 < m2)return 0;
	        if(d1 > d2)return 1;
	        if(d1 < d2)return 0;
	        return 0;
	    }
	    int daysBetweenDates(string date1, string date2) {
	        
	        
	        int y1 = stoi(date1.substr(0, 4));
	        int y2 = stoi(date2.substr(0, 4));
	        int m1 = stoi(date1.substr(5, 2));
	        int m2 = stoi(date2.substr(5, 2));
	        int d1 = stoi(date1.substr(8, 2));
	        int d2 = stoi(date2.substr(8, 2));
	        
	        printf("%d %d %d\n", y1,m1,d1);
	        printf("%d %d %d\n", y2,m2,d2);
	        if(big(y1,y2,m1,m2,d1,d2)){
	            swap(y1,y2);
	            swap(m1,m2);
	            swap(d1,d2);
	        }
	        int ans ;
	        
	        if(y1 == y2){
	            ans = getDay(y2,m2,d2) - getDay(y1,m1,d1);
	            return ans;
	        }
	        ans = getDay(y2,m2,d2);
	        ans += 365 + pr(y1) - getDay(y1,m1,d1);
	        if(y2 > y1){
	            int now = y2 - y1 - 1;
	            ans += 365 * (y2 - y1 - 1);
	            printf("%d\n",ans);
	            ans += (y2-1) / 4 - y1 / 4;
	             printf("%d\n",ans);
	            ans -= (y2-1) / 100 - (y1) / 100;
	             printf("%d\n",ans);
	            ans += (y2-1) / 400 - (y1) / 400;
	             printf("%d\n",ans);
	        
	        }
	       
	        
	        return ans;
	    }
	};
 
## 以0年1月1日为标准 ##
也可以找出一个标准。比如0年1月1日。来计算两者的差。

	class Solution {
	public:
	
	    vector<int> dict = {31,28,31,30,31,30,31,31,30,31,30,31};
	    int pr(int y){
	        return (y % 400 == 0 || (y % 100 && y % 4 == 0));
	    }
	    int getDay(int y, int m, int d){
	        int ans = 0;
	        for(int i = 1; i < m; ++ i){
	            ans += dict[i-1];
	            if(i == 2 && pr(y))ans += 1;
	        }
	        ans += d;
	        return ans;
	    }
	
	    int daysBetweenDates(string date1, string date2) {
	        
	        
	        int y1 = stoi(date1.substr(0, 4));
	        int y2 = stoi(date2.substr(0, 4));
	        int m1 = stoi(date1.substr(5, 2));
	        int m2 = stoi(date2.substr(5, 2));
	        int d1 = stoi(date1.substr(8, 2));
	        int d2 = stoi(date2.substr(8, 2));
	        
	        printf("%d %d %d\n", y1,m1,d1);
	        printf("%d %d %d\n", y2,m2,d2);
	       
	        int ans1,ans2 ;
	       
	        ans2 = getDay(y2,m2,d2);
	        ans1 = getDay(y1,m1,d1);
	        printf("%d %d\n", ans1, ans2);
	        ans1 += (y1) * 365;
	        ans1 += (y1-1) / 4;
	        ans1 -= (y1-1) / 100;
	        ans1 += (y1-1) / 400;
	
	        ans2 += (y2) * 365;
	        ans2 += (y2-1) / 4;
	        ans2 -= (y2-1) / 100;
	        ans2 += (y2-1) / 400;
	        printf("%d %d\n", ans1, ans2);
	        return abs(ans1-ans2);
	    }
	};

### 计算两个年份之间有多少天 ###
可以年份日期 * 365；
加上(x-1) / 4。这样里面包括了有这么多个可以整除4的。
减去(x-1) /100。去除能整除100的，缺少整除400的。
加上(x-1) /400。 补上完成

# 151.反转字符串 #
[https://leetcode-cn.com/problems/reverse-words-in-a-string/submissions/](https://leetcode-cn.com/problems/reverse-words-in-a-string/submissions/)


> 给定一个字符串，逐个翻转字符串中的每个单词。
> 
> 说明：
> 
> 无空格字符构成一个 单词 。
> 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
> 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
 

## O(1)的空间 ##
在本地字符串上操作。
先把字符串整体反转，在把对应的单词反转回来。

	class Solution {
	public:
	    string reverseWords(string s) {
	        int n = s.size();
	        reverse(s.begin(), s.end());
	        
	        int idx = 0, end = 0;
	        
	        for(int i = 0; i < s.size(); ++ i){
	            if(s[i] != ' '){
	                if( idx )s[idx++] = ' ';
	                end = i;
	                while(end < n && s[end] != ' ')s[idx++] = s[end++];
	                reverse(s.begin()+ idx -(end-i), s.begin() + idx); 
	                i = end;
	            }              
	        }
	        s.erase(s.begin() + idx, s.end());
	        
	
	        return s;
	    }
	};

## 保留对应括号版 ##

	class Solution {
	public:
	    string reverseWords(string s) {
	        int n = s.size();
	        reverse(s.begin(), s.end());
	        
	        int idx = 0, end = 0;
	        
	        for(int i = 0; i < s.size(); ++ i){
	            if(s[i] == ' ')s[idx++] = ' ';
	            if(s[i] != ' '){
	                
	                end = i;
	                while(end < n && s[end] != ' ')s[idx++] = s[end++];
	                reverse(s.begin()+ idx -(end-i), s.begin() + idx); 
	                i = end-1;
	            }              
	        }
			
	         //while(s.back() == ' ')s.pop_back();
        	//while(*s.begin() == ' ')s.erase(s.begin());
	        
	
	        return s;
	    }
	};

**395，151，253，1360**
----------
# 类型题目 #
## 区间调度 ##
总体感受，区间问题有两种
1. 尾排序：给一些[start, end]的区间，求最多有几个互不相交的区间。取部分区间。
2. 首排序：求包括所有区间的最多不相交区间的分组数。合并区间，


# 二叉树的遍历 #

## 94.二叉树的中序遍历 ##

### 递归 ###

	class Solution {
	public:
	    void dfs(vector<int> &ans, TreeNode* root){
	        if( !root )return ;
	        dfs(ans, root->left);
	        ans.push_back(root->val);
	        dfs(ans, root->right);
	    }
	    vector<int> inorderTraversal(TreeNode* root) {
	        vector<int> ans;
	       dfs(ans, root);
	       return ans;
	        
	    }
	};

### 迭代 ###
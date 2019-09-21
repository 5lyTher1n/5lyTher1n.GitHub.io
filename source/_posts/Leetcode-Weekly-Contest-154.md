---
title: Leetcode Weekly Contest 154
date: 2019-09-21 22:42:45
tags:
- algorithms 
- Leetcode 
categories:
- algorithms  
- 算法习题  
- Leetcode  
---
# 前情提要 #
本来早上打算起来参加Weekly Contest的，结果起来发现，时间是晚上10点30开始，也就是现在这个时间正在进行，但是，1个小时前就已经困了，刚刚把BugKu的内容整理出来，实在没精力再去做了，以后做模拟吧。临睡前把今天上午模拟的周赛154的四题整理一下。

# 1189. Maximum Number of Balloons #

[Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

题目大意：
给一个字符串，询问能从这个字符串中取字符，每个字符至多取一次，可以拼成多少个"balloon"

<!-- more -->

## 解法 ##
直接模拟，找木板最短的那一块。

	class Solution {
	public:
    int maxNumberOfBalloons(string text) {
        map <char,int> ha;
        for(int i = 0; i < text.size(); i ++){
            ++ ha[text[i]];
        }

        int ans = 1100000;
        ans = min(ans, ha['b']);
        ans = min(ans, ha['a']);
        ans = min(ans, ha['n']);
        ans = min(ans, (ha['l'] / 2) );
        ans = min(ans, (ha['o'] / 2) );
        return ans;    
    	}
	};


----------

# 1190. Reverse Substrings Between Each Pair of Parentheses #

[Reverse Substrings Between Each Pair of Parentheses](https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses/)

题目大意：

给一个字符串s，里面只有英文字符和小括号。现在要做的是根据平时的运算规则，对每个括号内的字符串进行反转。
数据规模：0 <= s.length <= 2000

## 解法1： 暴力##
这数据规模可以放心的暴力.即，每次读入一个'(',遇到一个')'时，进行反转。最后将小括号过滤掉。
	
	class Solution {
	public:
	    
	    string reverseParentheses(string s) {
	        vector<int> brack;
	        for(int i = 0; i < s.size();  ++ i){
	            if(s[i] == '(')
	                brack.emplace_back(i);
	            else if(s[i] == ')'){
	                int j = brack[ brack.size() - 1];
	                brack.pop_back();
	                for(int l = j,r = i; l < r; ++ l,--r){
	
	                    swap(s[l], s[r]);
	                }
	            }
	        }
	        string ans = fin(s);
	        return ans;
	    }
	    string fin(string s){
	        string ans = "";
	        for(auto it : s ){
	            if(it != '(' && it != ')')
	                ans += it;
	        }
	        return ans;
	    }
	};

代码有点乱，没有整理，顺便我也不知道当时怎么想起来还写一个fin函数。0.0

## 解法2 优化 ##
写完后，感觉这道题应该还可以优化，于是去翻Discuss，果然看到dalao提出了一个O(len)的写法。

详情见链接
[解法2 优化](https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses/discuss/383670/JavaC%2B%2BPython-Why-not-O(N))

思路挺清晰的，记录每一对的对应位置(左括号对应右括号，右括号对应左括号)。首先从左往右搜索，当遇到一个括号，即切到对应括号位置，因为要反转，直接将搜索顺序取反。直到遇到下一个括号，继续切到对应括号位置，再将搜索方向取反。最终完成。


----------

# 1191. K-Concatenation Maximum Sum #
[1191. K-Concatenation Maximum Sum ](https://leetcode.com/problems/k-concatenation-maximum-sum/)

题目大意：
给一个数组arr，将K个arr数组首尾先连，求最大连续串和。

解法：

首先，如果发现对一个arr求和，发现和大于0。

那可以从第2个到第k-1(k > 2)个，全取了，正数相加肯定越来越大，然后对第1个求最大后缀和，对第k个求最大前缀和，相加去摸即可。

如果arr求和小于0，那最大连续串和肯定在一个arr里或者在两个arr的连接处。那么对两个arr求最大连续串和即可。

注意前边k>2，我提前算了一个arr的最大连续和，进行了特判。感觉，我的代码还是可以再优化下。。。

	typedef long long ll;
	const ll Mod = 1e9 + 7;
	class Solution {
	public:
	    int kConcatenationMaxSum(vector<int>& arr, int k) {
	        ll sum,now,ans;
	        sum = now = ans = 0ll;
	        for(int i = 0; i < arr.size(); ++ i){
	            sum += arr[i];
	            now += arr[i];
	            if(now > ans)ans = now;
	            if(now < 0)now = 0;
	        }
	        if(k == 1)return ans % Mod;
	        
	        if(sum > 0 && k > 2){
	            ans = sum;
	            ans %= Mod;
	            ans = ans * (k-2) % Mod;
	            int m,now,i;
	            for(i = 0,now = 0,m = 0; i < arr.size(); ++ i){
	                now += arr[i];
	                if(now > m)m = now;
	            }
	            ans += m;
	            ans %= Mod;
	             for(i = arr.size()-1,now = 0,m = 0; i >= 0 ; -- i){
	                now += arr[i];
	                if(now > m)m = now;
	            } 
	            ans += m;
	            ans %= Mod;
	            return ans;
	        }
	        if(sum <= 0 || k == 2){
	            for(int i = 0,now = 0ll; i < arr.size() * 2; ++ i){
	            now += arr[i % arr.size()];
	            if(now > ans)ans = now;
	            if(now < 0)now = 0;
	            }
	            return ans % Mod;
	        }
	        
	        
	        return ans % Mod;
	    }
	};


----------

# 1192. Critical Connections in a Network #

[Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/)

题目大意：
图论问题，切除一条边，增加联通图个数。应该算是裸的模板题。
然后，模板没带来0.0，加上要么学了的图论早就忘完，要么有些没学。。。哎，补吧。。。

查了下资料，Tarjan算法可以解决。

大致思路：

- Tarjan在解决无向图的割边时，差不多就是DFS，只是同时维护着两个数组。

- 对n个点，点从0到n-1的图来说。

- 一个数组是dfn，这个数组是不变化的，dfn[i]表示以某一点为起始点，走到i点时的位置或者说是序数。

- 一个数组是val，这个数组是会变化的，val[i]表示能到达i的最小序数点。比如按DFS走过的路，第二个点和第五个点都可以走到i，则val[i] = 2;

算法过程

1. 首先任意选一个点u进行DFS，判断该点的联通点v是否没有走过，认为u是v的父节点。
2. 如果联通点v没有走过，则进入
3. 如果联通点v已经走过，且联通点v非该点u的父节点，则判断val[u]与val[v]大小来维护更新val[u]，然后回溯。如果不存在v点也回溯。
4. 回溯时更新u点，寻找u点所有联通点v的val[v]最小值，并更新。
5. 如果发现，dfn[u]的值大于等于val[u]的值，表示该点在环内，即至少两个点可以以不同方向到达u。否则为环外点，即为题目中要求的点。

	
		
		class Solution {
		public:
		    vector< vector<int> > edg,ans;
		vector< int > val,dfn;
		
		
		    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
		        edg.resize(n);
		        val.resize(n);
		        dfn.resize(n);
		        for(auto it : connections){
		            edg[it[0]].push_back(it[1]);
		            edg[it[1]].push_back(it[0]);
		        }
		        for(int i = 0; i < n; ++ i){
		            if( !dfn[i] )
		                dfs(i,-1);
		        }
		      
		        return ans;
		    }
		    
		    void dfs(int u,int p){
		        static int cnt = 0;
		        val[u] = dfn[u] = ++cnt;
		        for(auto v : edg[u]){
		            if( !dfn[v] ){
		                dfs(v,u);
		                val[u] = min(val[u], val[v]);
		                if(val[v] > dfn[u]){
		                    ans.push_back({v,u});
		                }
		            }
		            else if(v != p)
		                val[u] = min(val[u], val[v]);
		        
		        }
		    }
		};


----------

# 后记 #
感觉Leetcode还是要比CF简单点，但对我这种长时间没写过代码，以前喜欢拍模板的人来说，能收获不少东西。就是这个表达，不知道能不能锻炼起来。0.0

加油吧~
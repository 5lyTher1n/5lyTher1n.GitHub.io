---
title: Leetcode Weekly Test 155
date: 2019-09-22 20:28:45
tags:
- algorithms 
- Leetcode 
categories:
- algorithms  
- 算法习题  
- Leetcode

---

# 序幕 #

早上起来做Leetcode Contest 155，四题就做了两题出来，太逊了。。。以后的打算是争Rank的，现在才做两题。。。先整理下吧

# 1200. Minimum Absolute Difference #


> [1200. Minimum Absolute Difference](https://leetcode.com/problems/minimum-absolute-difference/)

题目大意：
在一个元素不等的数组里，输出绝对值最小的所有对。

解法：
按理说，第一题一般都是可以暴力的。直接对数组排序，然后比较相邻的绝对值，如果小于记录值则更新，等于记录值则添加。最终返回答案。

<!-- more -->


代码：

	class Solution {
	    int minn = 1e7;
	    vector <vector<int>> now;
	public:
	    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
	        sort(arr.begin(), arr.end());
	        for(int i = 1; i < arr.size(); i ++){
	            int c = abs(arr[i] - arr[i-1]);
	            if(c < minn){
	                minn = c;
	                now.clear();
	                now.push_back({arr[i-1], arr[i]});
	            }
	            else if(c == minn){
	                now.push_back({arr[i-1], arr[i]});
	            }
	        }
	        return now;
	    }
	};


----------

#1201. Ugly Number III #

> [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/)

题目大意：
寻找第n个，能被a**或**被b**或**被c整除的数。

- 1 <= n, a, b, c <= 10^9
- 1 <= a * b * c <= 10^18
- It's guaranteed that the result will be in range [1, 2 * 10^9]

没做出来。。。这个数据范围是不可能暴力的，那么一般降低数据范围的手段有二分，可怎么二分呢？容斥这个点我是真的当时没想到，看到了题解才想起来，之前学的概率全还回去了。。。哎，菜啊。。。

看了题解后的代码：

	typedef long long ll;
	
	class Solution {
	    ll ab,ac,bc,abc;
	    ll gcd(ll x, ll y){
	        if(x % y == 0)return y;
	        return gcd(y, x%y);
	    }    
	    ll lcm(ll x,ll y){
	        return x / gcd(x,y) * y;
	    }
	    ll getNum(ll n,ll a,ll b,ll c){
	        return n/a + n/b + n/c -n/ab - n/ac - n/bc + n/abc;
	    }
	    
	public:
	    int nthUglyNumber(int n, int a, int b, int c) {
	        if(a == 1 || b == 1 || c == 1 )return n;
	        ab = lcm(a,b);
	        ac = lcm(a,c);
	        bc = lcm(c,b);
	        abc = lcm(ab,c);
	        
	        ll r = n * min(min(a,b),c);
	        ll l = n;
	        
	        while(l <= r){
	            ll mid = l + (r - l) / 2;
	            if(getNum(mid, a, b, c) < n)
	                l = mid + 1;
	            else r = mid - 1;
	        }
	        return l;
	    }
	        
	        
	    
	};



----------

# 1202. Smallest String With Swaps #

> [https://leetcode.com/problems/smallest-string-with-swaps/](https://leetcode.com/problems/smallest-string-with-swaps/)

题目大意：
给一个字符串，和一些下标数值对。可以将下标对的元素交换位置。
例如:

    Input: s = "cba", pairs = [[0,1],[1,2]]
    Output: "abc"
    Explaination: 
    Swap s[0] and s[1], s = "bca"
    Swap s[1] and s[2], s = "bac"
    Swap s[0] and s[1], s = "abc"

解法:

我的想法可能比较复杂，通过并查集来做。我看有使用不同方法的，这里通过样例可以看出，(0,1)(1,2)联通则(0，2)也是联通的。那么并查集可以处理这种问题。但我的处理方式可能有些复杂。

首先用并查集找到哪些节点在一棵树上的，然后把这些节点对应的字符整理出来进行排序，之后再扫描一遍字符串查询到所属输，依次取出排序后的字符。

代码：
	
	class Solution {
	    int f[100010],cnt[100010];
	    vector < string > k;
	    int p[100010];
	public:
	    
	    int Find(int t){
	        return f[t] == t ?  t : f[t] = Find(f[t]);
	    }
	    
	    void init(int n){
	        for(int i = 0; i < n; ++ i)f[i] = i;
	    }
	    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
	             int n = s.size();    
	            k.resize(n);
	            init(n);
	            for(auto it : pairs){
	                int l = min(it[0],it[1]);
	                int r = max(it[1],it[0]);
	                 l = Find(l);
	                 r = Find(r);
	                if(l == r)continue;
	                f[l] = r;
	            }
	            
	            for(int i = 0; i < n; ++ i)f[i] = Find(i);
	           
	            for(int i = 0; i < n; ++ i)k[f[i]] += s[i];
	            
	            for(int i = 0; i < n; ++ i)
	                    if(k[i].size() > 1)
	                        sort(k[i].begin(), k[i].end());
	       
	            string ans = "";
	
	            for(int i = 0; i < n; ++ i){
	                int t = f[i];
	                int num = p[t] ++;
	                ans += k[t][num];
	            }
	            return ans;
	    }
	};

重新交了下，看效率好像还行。。。就是感觉写的好繁杂，不简洁。

    Accepted	164 ms	49.1 MB	cpp
    Runtime: 164 ms, faster than 90.00% of C++ online submissions for Smallest String With Swaps.
    Memory Usage: 49.1 MB, less than 100.00% of C++ online submissions for Smallest String With Swaps.



----------

# 1203. Sort Items by Groups Respecting Dependencies #
> [1203. Sort Items by Groups Respecting Dependencies
> ](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

Todo:看都没看。。。之后补吧

# 最后 #
感觉还是太菜了，尽管也有些客观原因，但是还是觉得不该写这么差。同时，这种记录方式觉得不太好，总觉得有种为了记录而记录，没有那么强的总结的效果。走着看吧~

加油~话说，可以在算法专题里套用一句bin神(kuangbin大佬)的话。

**人一我百，人十我万，永不放弃，加油！**
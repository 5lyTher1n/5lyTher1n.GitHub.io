---
title: < LeetCode 313 >. Super Ugly Number
date: 2019-09-24 22:21:00
tags:   
- algorithms 
- Leetcode 
- DP
categories:
- algorithms  
- 算法习题  
- Leetcode  
---

# 题目信息 #
> [https://leetcode.com/problems/super-ugly-number/](https://leetcode.com/problems/super-ugly-number/)

题目大意：
[264 Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)的变形，其实能弄明白264的原理后，这题就不难了，无外乎原来3个因数2，3，5 变成了k个质因数保存在primes里了。

依然可以用264的思路，维护t[i]使primes[i]*ans[t[i]]最小，遍历i产生的最小值加入ans里，保证了加入到ans里的数的质因数都只存在于primes里，同时也保证每次加入进来的是当前最小的，不会遗漏的同时保证ans呈升序排列。

<!-- more -->

*PS：话说，这两天和Ugly Number干上了。。。把这个系列都做了下0.0*

代码：

	class Solution {
	public:
	    int nthSuperUglyNumber(int n, vector<int>& p) {
	         int k = p.size();
	        
	        vector<int> ans(n),t(k);
	
	        ans[0] = 1;
	        
	        for(int i = 1; i < n; ++ i){
	            int m = p[0] * ans[ t[0] ];
	            
	            for(int j = 1; j < k; j ++)
	                m = min(p[j] * ans[ t[j] ], m); 
	            ans[i] = m;
	             for(int j = 0; j < k; j ++)
	                if(m == p[j] * ans[ t[j] ] ) ++ t[j];
	        }
	        return ans[n-1];
	             
	    }
	    
	};

# 总结 #

在做了264后能做出来313，说明智商没退化到原始人地步0.0
讲道理如果还做不出来就太不应该了。
碰到题目要多思考看看能不能扩展，能不能形成模板，换些条件是什么样吧，尽管不太容易能做到，也要多尝试吧~加油~

**人一我百，人十我万，永不放弃，加油！**
---
title: < LeetCode 264 >.Ugly Number II
date: 2019-09-24 22:21:17
tags:   
- algorithms 
- Leetcode 
- DP
categories:
- algorithms  
- 算法习题  
- Leetcode  
---

# 题面信息 #

> [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)

题目大意：
找到第n个质因数只有2、3、5其中一个或几个的正数。

<!-- more -->

# 思路过程 #

先写了这题的退化版本，[Ugly Number 263](https://leetcode.com/problems/ugly-number) 即判断一个数是否是Ugly Number，然后写这一题，很自然的会想到暴力，然后发现错误的估计了时间复杂度。

再加上写过[878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/)又去想二分，细想下，不对，878是只要可以整除即可，但这题是最多只能有2，3，5这三个质因数。比如28，对878来说，可以被2整除，符合。但在264里28 = 2 * 2 * 7,质因数还有一个7，所以不符合。

要换别的做法，在暴力时打了下表，发现数字中间空隙逐渐变大，直接枚举数需要跳过很多非Ugly数。那么考虑，三重循环，枚举n个2，3，5的乘积，放入set里排序，当set.size等于n时结束返回答案。但很明显，1*5 > 2*2,直接写可能会把更大的数直接拉进set里，导致output大于exp出错。

思路在这里卡住了，看了下Discuss，很棒的一种处理方式：
1. 根据题意数字1符合要求。在list里的1，2，3，5都是符合要求的，可以利用list里的元素去乘2，3，5能保证乘出来的数依然是符合要求的。
2. 利用三个变量选择list里对应乘上2，3，5最小的数。
3. 将乘上后满足要求最小的数加入list里，同时，哪个变量产生的最小数进行更新。

*表达不好啊，不知道怎么说。。。:(*

直接看代码可能更明白些。。。

	class Solution {
	public:
	    int nthUglyNumber(int n) {
	        vector <int> ans(n);
	        ans[0] = 1;
	        int a,b,c;
	        a = b = c = 0;
	        for(int i = 1; i < n; ++ i){
	            ans[i] = min(ans[a] * 2, min(ans[b] * 3, ans[c] * 5));
	            if(ans[i] == ans[a] * 2)++ a;
	            if(ans[i] == ans[b] * 3)++ b;
	            if(ans[i] == ans[c] * 5)++ c;
	        }
	        return ans[n-1];
	        
	    }
	};

# 总结 #

解这种题总是容易卡住在某个点上，觉得现在代码实现能力太差了，而且思路太窄，常见的题型也都没见全，多刷刷吧~

加油吧~

**人一我百，人十我万，永不放弃，加油！**
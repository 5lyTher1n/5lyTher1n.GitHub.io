---
title: < Leetcode 598 >. Range Addition II
date: 2019-09-25 12:42:11
tags:   
- algorithms 
- Leetcode 
- 算法杂项实现
categories:
- algorithms  
- 算法习题  
- Leetcode  
---

# 题目描述 #
> [Range Addition II](https://leetcode.com/problems/range-addition-ii/)

题目大意：
给一个m*n的矩阵，初始全为0。然后给k个操作。每个操作给两个数字a与b，使M[i][j]加一，其中0 <= i < a,0<=j<b。
求最大数的个数

<!-- more -->

# 解体思路 #

看到这题想到以前做的NOIP铺地毯，都是区间覆盖问题。但是又不一样，这题要求的是求最大覆盖层的和。仔细看下，覆盖方式可以类比成，以(1,1)为定点，拉扯鼠标画范围，也就是无论什么区间，都是必然经过起点(1,1)的，可以扩展出来，离起点越近的点也就被覆盖的可能越大，那么，只要统计每次(a,b)的最小值即可。

	class Solution {
	public:
	    int maxCount(int m, int n, vector<vector<int>>& ops) {
	        for(auto it : ops){
	            m = min(m,it[0]);
	            n = min(n,it[1]);
	        }
	        return n * m;
	        
	    }
	};

# 总结 #

今天有点颓0.0
加油吧~

**人一我百，人十我万，永不放弃，加油！**
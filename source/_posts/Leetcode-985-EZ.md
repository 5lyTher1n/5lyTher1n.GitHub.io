---
title: < Leetcode 985 >. Sum of Even Numbers After Queries
date: 2019-09-25 10:30:13
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
> [Sum of Even Numbers After Queries](https://leetcode.com/problems/sum-of-even-numbers-after-queries/)

题目大意：给一个数组queries，每个query有两个元素，idx = query[1],表示A的位置，val = query[0]，表示给A[idx] += val。idx从0开始计数，值改变一直有效。每次计算每项值是偶数的和，放入ans里，最后返回ans

<!-- more -->
----------

# 解体思路 #
看数据范围暴力不现实。发现一些规律
1. odd -> even  : += A[idx] + val
2. odd -> odd   : Ignore
3. even -> even : += val
4. even -> odd  : -= A[idx]
5. A[idx] += val

每次修改后，都可以直接对sum进行对应修改，那么直接先计算出初始偶数和，然后判断对A[idx]加上val的值情况，对sum进行更新就可以了

*话说，单点修改，如果是求区间和，这题就是树状数组或线段树问题了。*

	class Solution {
	public:
	    vector<int> sumEvenAfterQueries(vector<int>& A, vector<vector<int>>& queries) {
	        int sum = 0;
	        vector <int> ans;
	        for(auto it : A)if(it  % 2 == 0)sum += it;
	        for(auto it : queries){
	            int val = it[0];
	            int idx = it[1];
	            int t = A[idx];
	            if(t & 1){
	                if((val + t) % 2 == 0)
	                    sum += val+t;
	            }else {
	                if ((val + t ) & 1) sum -= t;
	                else sum += val;
	            }
	            A[idx] += val; 
	            ans.push_back(sum);
	        }
	        return ans;
	    }
	};

# 总结 #
分析下还是能分析出来的，但看标签式Eazy，哎，总感觉难度分级不太合理，但细想好像也不难~~加油吧~ 


**人一我百，人十我万，永不放弃，加油！**
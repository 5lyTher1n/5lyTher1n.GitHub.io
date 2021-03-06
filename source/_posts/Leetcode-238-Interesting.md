---
title: < Leetcode 238 >. Product of Array Except Self
date: 2019-09-23 22:52:33
tags:   
- algorithms 
- Leetcode 
- Interesting_Interview
categories:
- algorithms  
- 算法习题  
- Leetcode  
---
# 题目描述 #

给一个含有n个数的数组nums，求一个新的数组ans，

1. ans[i] = $\prod_{j=1}^{n，i != j}$ nums[j]

2. 时间复杂度O(n)，不能用除法，不考虑nums的情况下空间复杂度O(1).


# 解析 #

想起来一句话，如果一道题附加了条件，或者改变数据量，会变成另一题。这个题如果数据量小，没有附加要求的话，可以直接暴力。时间复杂度是O($n^2$)。

<!-- more -->

但是增加了时间复杂度，数据量小依然可以用类似前缀和的方式，计算全部整体的积m，然后对`ans[i] = m / nums[i];`

如果如题要求，不能用除法后，上述方法就都不能用了。看样子这应该是面试题目，但觉得挺有趣而且应该是有一定考虑的。只是我一时没想到怎么写0.0，去翻Discuss。。。

首先，之前用的除法方式是一把抓然后扣掉不要的那个。现在可以换个方法，根据不要的那个，分成左右两边，ans[i] = 分割线左积 * 分割线右积。相关代码：

		l[0] = r[0] = 1;
		for(i = 1; i < n; ++ i){
			l[i] = l[i-1] * nums[i-1];
		    r[i] = r[i-1] * nums[n-i];
		}
		
		for(i = 0; i < n; ++ i)
			ans[i] = l[i] * r[n-i-1]; 


现在满足了时间复杂度，但是空间复杂度依然不对。进一步优化空间，可以发现，两个数组其实没有存在的必要性，他们只在一次运算后就抛弃了，可以用两个数来进行这种运算。变量l用来计算左端乘积，而r用来计算右端乘积，对每个ans[i]会乘一次l，乘一次r。相关代码：

	l = r = 1;
	for(i = 0; i < n; ++ i){
		ans[i] *= l;
		l *= nums[i];
		ans[n-i-1] *= r;
		r *= nums[n-i-1];
	}

这里会发现，每个元素会先后乘两次，乘一次l，乘一次r。像是两个点，一个从左往右走，一个从右往左走。假设i在前半部分，在第一次走到i时，l已经是前i-1个元素的乘积，在第二次走到i时，r已经是后边从i+1到n的成绩。

也可以把循环拆开写，无外乎只是左边计算一次，右边计算一次。AC代码：

	class Solution {
	public:
	    vector<int> productExceptSelf(vector<int>& nums) {
	        int n = nums.size();
	        vector<int> ans(n,1);
	        
	        for(int i = 0,t = 1; i < n; i ++)ans[i] *= t, t *= nums[i];
	        for(int i = n-1,t = 1; i >= 0; i --)ans[i] *= t, t *= nums[i];
	        
	        return ans;
	    }
	};


# 总结 #

写惯了暴力后写带思路的题面有点僵硬，思维钝化了，需要多见见多想想这种题，多总结好，算是多种思路多种见识吧。挺有趣的，加油！


**人一我百，人十我万，永不放弃，加油！**
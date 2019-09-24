---
title: < Leetcode 263 >. Ugly Number
date: 2019-09-24 10:30:11
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
>  [Ugly Number](https://leetcode.com/problems/ugly-number/)

题目大意：
判断一个数是否只能被2，3，5整除，即不存在其他因数。

<!-- more -->
# 原始解法 #
直接实现。

	
		class Solution {
		public:
	    bool isUgly(int num) {
	         if( !num )return false;
	         while( !(num % 2))num /= 2;
	         while( !(num % 3))num /= 3;
	         while( !(num % 5))num /= 5;
	        if(num == 1)return true;
	        return false;
	    }
	};

# 修改解法 #
看了Discuss的一种解法，觉得看着挺简洁舒服的。。。
	
	class Solution {
	    public:
	        bool isUgly(int num) {
	             for(int i = 2; i <= 5 && num ; ++ i)
	                    while(num % i == 0)num /= i;
	            return num == 1;
	        }
	};

# 总结 #
简单题也尽量写的简洁舒适，要可以在Notepad写完交上去也能对的程度。

加油~

**人一我百，人十我万，永不放弃，加油！**
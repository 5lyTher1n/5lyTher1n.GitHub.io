---
title: < Leetcode 985 >. Sort Array By Parity II
date: 2019-09-26 22:51:23
tags:   
- algorithms 
- Leetcode 
- 算法杂项实现
categories:
- algorithms  
- 算法习题  
- Leetcode  
---

# 题目信息 #
> [Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii/)

题目大意：
给一个数组A，里面有偶数个元素，一半为奇数，一半为偶数，要求使偶数位置为偶数，奇数位置为奇数，从0计数。

<!-- more -->

# 解题思路 #
时空要求不高，可以直接写。同时因为偶数个元素，奇偶数量相等，不存在偶数放完后还有大量奇数没有放。应该是同时放完。

第一反应，新建3个ans、odd、even数组，然后扫一遍A，把偶数放在even，奇数放在odd里。然后分别放入ans。

	class Solution {
	public:
	    vector<int> sortArrayByParityII(vector<int>& A) {
	        vector<int> ans,odd,even;
	        for(auto it : A){
	            if(it & 1)odd.push_back(it);
	            else even.push_back(it);
	        }
	        
	        for(int k=0,i = 0,j = 0; j < even.size() || i < odd.size(); )
	            if(k++ & 1)
	                ans.push_back(odd[i++]);
	            else ans.push_back(even[j++]);
	        
	        return ans;
	    }
	};

思考，觉得可以不用odd和even。扫描时找奇偶直接放进ans

	class Solution {
	public:
	    vector<int> sortArrayByParityII(vector<int>& a) {
	        vector<int> ans,odd,even;
	        int n = a.size();
	        for(int i = 0,j = 0; i < n && j < n; ){
	            while(i < n){
	                if(a[i] % 2 == 0)break;
	                else ++ i;
	            }
	              while(j < n){
	                if(a[j] % 2 == 1)break;
	                else ++ j;
	            }
	            if(i == n || j == n)break;
	            ans.push_back(a[i]);
	            ans.push_back(a[j]);
	            ++ j;
	            ++ i;
	        }
	       // ans.push_back(a[n-1]);
	        return ans;
	    }
	};

感觉时间复杂度不是很好降了，最差也是O(n)，这个数据量常数影响应该不大，提交几次发现时间上下浮动20ms差不多就这样了。
看了下Discuss有没有什么好玩的，发现降低空间复杂度和简洁的写法。可以直接使用swap

	class Solution {
	public:
	    vector<int> sortArrayByParityII(vector<int>& A) {
	        for(int i = 0,j = 1; i < A.size() ;i+=2,j+=2){
	            while(i < A.size() && A[i] % 2 == 0) i += 2;
	             while(j < A.size() && A[j] % 2 == 1) j += 2;
	            if(i < A.size() )swap(A[i], A[j]);
	        }
	        return A;
	    }
	};

不难理解，直接定位终态，寻找初态的转换。很简洁也没浪费空间。

# 总结 #
继续颓。。。尽快把乱七八糟的事处理完吧，加油~


**人一我百，人十我万，永不放弃，加油！**
---
title: <LeetCode> 878. Nth Magical Number

date: 2019-09-23 22:07:53
tags:   
- algorithms 
- Leetcode 
- BinarySearch
categories:
- algorithms  
- 算法习题  
- Leetcode  
---

# 题目信息 #
> [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/)

题目大意：

搜索第n个能被A或者B整除的数。

# 解析 #

这个题让我想起了Weekly Contest 155的第二题，就是当时没写出来的容斥那题，这题算是那题的简化，将第n个同时被三个数整除，改为同时被两个数整除。

顺手吐槽下难度分级方式，这题是 **Hard** 而，1201是 **Medium**。

<!-- more -->

整体思路及相似，首先计算一个数字x可以被A整除，加上可以被B整除，然后减去可以被A、B的最小公倍数整除，即可算出x里有k个数满足能被A或B整除。然后，很自然的对任意x'大于x的数，必有k'>=k。满足二分的要求，然后利用二分把最优解转化成求可行解。

----------

# 代码 #

	typedef long long ll;
	
	class Solution {
	    const ll Mod = 1e9+7;
	    ll ab;
	    ll gcd(ll a,ll b){
	        return a % b == 0 ? b : gcd(b,a % b);
	    }
	    ll lcm(ll a, ll b){
	        return a / gcd(a,b) * b;
	    }
	    ll getNum(ll n,ll a, ll b){
	        return n / a + n / b - n / ab;
	    }
	    
	public:
	    int nthMagicalNumber(int N, int A, int B) {
	        ab = lcm(A,B);
	        ll l = N * 1ll;
	        ll r = N * (ll)min(A,B);
	        ll mid;
	        while(l < r){
	            mid = l + (r - l) / 2;
	            if(getNum(mid,A,B) < N)l = mid + 1;
	            else r = mid;
	        }
	        return l % Mod;
	    }
	};

----------

# 总结 #

总体来说，就是容斥加二分。如果在做过WC155后仍然对这题写不出来，那就太不应该了。

可以关注下写二分查找的方法，利用左闭右开的特点来写二分。


**人一我百，人十我万，永不放弃，加油！**
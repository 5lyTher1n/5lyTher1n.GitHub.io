---
title: < Leetcode 28 >. Implement strStr() 
date: 2019-09-19 18:39:15
tags:   
- algorithms 
- Leetcode 
categories:
- algorithms  
- 算法习题  
- Leetcode  
---

[题目链接](https://leetcode.com/problems/implement-strstr/ "[28. Implement strStr() 题目链接]")

算是比较直接简单的模板题。

直接简单修改[KMP算法学习笔记](https://5lyther1n.github.io/2019/09/18/kmp0/#more "KMP算法学习笔记")后附的代码段落就可以了。

	
	class Solution {
	public:
	    
	    vector<int> getNext(string n) {
	        int len = n.size();
	        vector<int> next(len+1,0) ;
	        int i,j;
	        next[0] = -1;
	        i = 0,j = 0;
	        while(i < len){
	
	            if(j == -1 || n[i] == n[j]){
	                ++ i;
	                ++ j;
	                next[i] = j;
	            }
	            else j = next[j];
	        }
	        return next;
	    }
	    
	    int strStr(string h, string n) {
	        if(n.size() == 0) return 0;
	        int i,j,lenh,lenn;
	        lenh = h.size();
	        lenn = n.size();
	        i = 0,j = 0;
	        vector<int> next = getNext(n);
	        while(i < lenh && j < lenn){
	            if(j == -1 || h[i] == n[j]){
	                ++ i;
	                ++ j;
	            }
	            else j = next[j];
	        }
	        if(j == lenn)return i-j;
	        return -1;
	   }
	};

	Accepted	8 ms	9.4 MB	cpp

*bin神的模板代码感觉是不错，可我的代码风格感觉没那么简洁0.0，哎，~~ 扶脑壳 ~~*

*(PS:好像这个主题默认不能使用方括号作为标题啊0.0)*

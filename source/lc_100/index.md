目标和（01背包）
逆序对、拓扑排序
三种遍历的递归非递归
----------

# 1 #
# 2 #

注意进位

	class Solution {
	public:
	    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
	        int flg = 0;
	        ListNode* ans = new ListNode();
	        auto now = ans;
	        int v1,v2,v;
	        while(l1 || l2){
	            v1 = l1 ? l1->val : 0;
	            v2 = l2 ? l2->val : 0;
	            v = v1 + v2 + flg;
	            if(l1 != nullptr)l1 = l1->next;
	            if(l2 != nullptr)l2 = l2->next;
	            flg = v / 10;
	            now->next = new ListNode(v % 10);
	            now = now->next;
	        }
	        if(flg)now->next = new ListNode(flg);
	        return ans->next;
	
	    }
	};

# 3 #
# 4 #
# 5 #
# 10 #
# 11. 盛最多水的容器 #
[盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

做过两题单调栈的问题后，再看这一题，总感觉似曾相识。但又有些不一样的地方。单调栈一般选取的是往左/往右，最先大于/小于该值的位置/数。

如果用矩形面积和接雨水的解法来处理这题，这题的预处理应该是往左/往右最后大于等于该值的位置。

一时没想到该怎么预处理合适。看了下题解，发现不同之处。这题，只需要确定左右两个边界就行。里面无论是高了冒个头，还是矮了被淹了。都不会影响最后的答案：容纳的水量。

最后可以双指针，用i，j来确定左右边界。找到左右相对小的一边，更新答案值，并移动小的一边。

证明很有趣：假设左边i对应的是x，右边j对应的值是y，不妨假设x<y。
此时的值为 (j-i)*x;

假如固定i，来移动j，来到j-1.若j-1对应的值是yy;
1. 如果yy >= y，因为x<y，yy并不被考虑。最后的值是(j-1-i) * x
2. 如果yy < y，此时最小值为min(x,yy) <= x.最后求值 <= (j-i-1)*x

发现值都在变小，应该去移动i。

	class Solution {
	public:
	    int maxArea(vector<int>& height) {
	        int i = 0,n = height.size(), j = n - 1, ans = -1,cnt = n-1;
	        
	        while(cnt){
	           // printf("%d\n",cnt);
	            if(height[i] < height[j]){
	                ans = max(ans, height[i] * cnt);
	                i ++;
	            }
	            else{
	                ans = max(ans, height[j] * cnt);
	                j --;
	            }
	            cnt --;
	        }
	        return ans;

    }
};


# 15 #
# 17 #
# 19 #

num=结点数
删除第n个，比较n 和 num的大小？

[0] - 1 - 2 - 3 - 4 - （5） - 6
ans      cur
   
让cur往前走n个
cur->next == nullptr

ans->next = ans->next->next;

	class Solution {
	public:
	    ListNode* removeNthFromEnd(ListNode* head, int n) {
	        ListNode* ans = new ListNode(0,head);
	        auto tmp = ans, cur = ans;
	        int nn = n;
	        while(nn --){
	            cur = cur -> next;
	        }
	        while(cur->next){
	            cur = cur -> next;
	            tmp = tmp -> next;
	        }
	        tmp->next = tmp->next->next;
	        return ans->next;
	    }
	};




# 20 #
# 21 #

while(l1 && l2) !empty
v1 = l1->val
v2 = l2->val
if(v1<v2)
	ans->next = l1;
	l1 = l1->next;
}
ans->next = l1 ? l1 : l2;

## 递推 ##
	
	class Solution {
	public:
	    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
	        ListNode* ans = new ListNode();
	        auto tmp = ans;
	        int v1,v2;
	        while(l1 && l2){
	            v1 = l1 -> val;
	            v2 = l2 -> val;
	            if(v1 < v2){
	                tmp -> next = l1;
	                l1 = l1 -> next;
	            }else{
	                tmp -> next = l2;
	                l2 = l2 -> next;
	            }
	            tmp = tmp -> next;
	        }
	        tmp -> next = l1 ? l1 : l2;
	        return ans->next;
	    }
	};

## 递归 ##


	class Solution {
	public:
	    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
	        if( !l1 )return l2;
	        if( !l2 )return l1;
	        if(l1->val < l2->val){
	            l1->next = mergeTwoLists(l1->next, l2);
	            return l1;
	        }else{
	            l2->next = mergeTwoLists(l1, l2->next);
	            return l2;
	        }
	    }
	};

# 22.括号生成 #
> 数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

给一个数字n，生成n对括号组合。

注意括号生成的规则。
1. 左右括号各n个。
2. 要有左括号才能加右括号。

	class Solution {
	vector<string> ans;
	public:
	    void dfs(string tmp, int l,int r,int cnt, int n){
	        if(cnt == 2 * n && l == r){
	            ans.push_back(tmp);
	            return ;
	        }
	        if(l > n || r > n)return ;
	        if(l < n){
	            tmp += '(';
	            dfs(tmp, l+1, r,cnt+1, n);
	            tmp.pop_back();
	        }
	        if(l > r){
	            tmp += ')';
	            dfs(tmp, l, r+1, cnt+1, n);
	            tmp.pop_back();
	        }
	    }
	    vector<string> generateParenthesis(int n) {
	        string tmp;
	        dfs(tmp, 0,0, 0 ,n);
	        return ans;
	    }
	};


# 23 #
# 31. 下一个排列 #
[下一个排列](https://leetcode-cn.com/problems/next-permutation/)
## 题意 ##：给定一个序列，求按字典序的下一个序列。
根据有点类似于实现全排列函数(next_permutation)。

## 解法 ##
按照官方题解，要找到一个较小的数，位置为x。然后找到一个比它较大而且位置靠右的数，交换两个数值，并对x位置后的数按升序放入。

怎么找到位置x？
从后往前找，找到第一个nums[x] < nums[x+1]的数。要找到字典序的下一个序列，会比现在大。对这个位置进行增大。
在寻找的过程中，x右边的序列为降序序列，找到第一个大于nums[x]的数。交换两个数值，依然是降序序列。转成升序，数值增大，但是只增大一下。
如果发现x没有/为-1.则为最大值。直接重排序成最小值就行。

	class Solution {
	public:
	    void nextPermutation(vector<int>& nums) {
	        int n = nums.size(),i,x = -1,j = n - 1;
	
	        for(i = n-1; i >= 1; -- i){
	            if(nums[i] > nums[i-1])
	            {
	                x = i - 1;
	                break;
	            }
	        }
	        if(x >= 0)
	            for(i = n - 1; i >= x; -- i)
	                if(nums[i] > nums[x]){
	                    swap(nums[i], nums[x]);
	                    //i = x + 1;
	                    break;
	                }
	        
	        reverse(nums.begin() + x + 1, nums.end());
	       
	    }
	};






# 32 #
# 33 #
# 34 排序数组种查找元素的第一个和最后一个位置#
> 给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
> 
> 如果数组中不存在目标值 target，返回 [-1, -1]。

标准二分题。
## lower_bound & upper_bound ##

	class Solution {
	public:
	    vector<int> searchRange(vector<int>& nums, int target) {
	        auto idx = lower_bound(nums.begin(), nums.end(), target);
	        vector<int> ans(2);
	        if(idx == nums.end() || *idx != target ){
	            ans[0] = ans[1] = -1;
	            return ans;
	        }
	        ans[0] = idx - nums.begin();
	        auto idy = upper_bound(nums.begin(), nums.end(), target) - nums.begin() ;
	        if(idy)--idy;
	        ans[1] = idy;
	        return ans;
	    }
	};

## 自己写二分 ##

	class Solution {
	public:
	
	    vector<int> searchRange(vector<int>& nums, int target) {
	        int mid, l = 0, r = nums.size(); 
	        vector<int> ans(2); 
	        while(l < r){
	            mid = l + (r - l)/2;
	            if(nums[mid] < target)
	                l = mid + 1;
	            else
	                r = mid;   
	        }
	
	        if(l != nums.size() && nums[l] == target)ans[0] = l;
	        else{
	            ans[0] = ans[1] = -1;
	            return ans;
	        }
	
	        l = 0, r = nums.size();
	        while(l < r){
	            mid = l + (r - l)/2;
	            if(nums[mid] <= target)
	                l = mid + 1;
	            else
	                r = mid;
	        }
	        ans[1] = l-1;
	       
	        return ans;
	    }
	};

# 39 #
# 42. 接雨水 #
关键点：可以这样算能够积累的水量。对i，如果h[i-x]>h[i]&&h[i+y]>h[i]。积累的水量为，底×高 

## 预处理+暴力 ##
预处理两个数组，分别代表着，
从0到i的最大值，即i往左走，能得到的最大值。
从i到n-1的最大值，i往右走，能得到的最大值。

	 int trap(vector<int>& height) {
	        
	        int n = height.size(),ans = 0;;
	        if(n < 3)return 0;
	        vector<int> l(n),r(n);
	        l[0] = height[0];
	        for(int i = 1; i < n; ++ i)l[i] = max(l[i-1], height[i]);
	        r[n-1] = height[n-1];
	        for(int i = n-2; i >= 0; -- i)r[i] = max(r[i+1], height[i]);
	
	        for(int i = 1; i < n-1; ++ i)
	            ans += min(l[i],r[i]) - height[i];
	        
	        return ans;
	    }
## 单调栈解法： ##

限定边界。左右边界的最小值*左右边界中间的距离。

	int trap(vector<int>& height) {
	        stack<int> s;
	        int ans = 0,i = 0,len = height.size();
	        
	        while(i < len && height[i] == 0 )i++;
	        if(i == len)return 0;
	
	        for(; i < len; ++ i){
	            while(!s.empty() && height[i] > height[s.top()]){
	                int t = s.top();
	                s.pop();
	                if(s.empty())break;
	                ans += (i - s.top() - 1)*(min(height[i], height[s.top()]) - height[t]);
	            }
	            s.push(i);
	        }
	
	        return ans;
	    }

## 双指针 ##


# 46 #
# 48.旋转图像 #
[https://leetcode-cn.com/problems/rotate-image/](https://leetcode-cn.com/problems/rotate-image/)


> 给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。
> 
> 你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

有趣的一题。官方题解中是循序渐进式的给出了解答方法。
假如可以用另一个数组：

	m[j][n-i-1] = m[i][j]

之后发现，可以先水平对称，再对角线对称可以起到相同的效果：

	m[i][j] => m[n-i-1][j] => m[j][n-i-1]

这样，代码就很好写了。
	
	class Solution {
	public:
	    void rotate(vector<vector<int>>& matrix) {
	        int n = matrix.size();
	        for(int i = 0; i < n/2; ++ i)
	            for(int j = 0; j < n; ++ j)
	                swap(matrix[i][j], matrix[n-i-1][j]);
	        for(int i = 0; i < n; ++ i)
	            for(int j = 0; j < i; ++ j)
	                swap(matrix[i][j], matrix[j][i]);
	        
	    }
	};

# 49.字母异位词分组 #
[https://leetcode-cn.com/problems/group-anagrams/](https://leetcode-cn.com/problems/group-anagrams/)

> 给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

感觉不是最优解。但是是可行解。
取单词后排序，形成唯一标识符。用map做hash处理

	class Solution {
	public:
	    unordered_map<string, vector<string>> mp;
	
	    vector<vector<string>> groupAnagrams(vector<string>& strs) {
	        for(auto i : strs){
	            string str = i;
	            sort(str.begin(), str.end());
	            mp[str].push_back(i);
	        }
	        vector<vector<string>> ans;
	        for(auto i : mp){
	            ans.push_back(i.second);
	        }
	        return ans;
	    }
	};


# 53 #
# 55 #
# 56.合并区间 #
[https://leetcode-cn.com/problems/merge-intervals/](https://leetcode-cn.com/problems/merge-intervals/)
> 以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

按照左端点排序，排序后，维护右端点的值。


	class Solution {
	public:
	    vector<vector<int>> merge(vector<vector<int>>& intervals) {
	        int n = intervals.size();
	        vector<vector<int>> ans;
	        if( n == 0)return ans;
	        sort(intervals.begin(), intervals.end());
	
	        ans.push_back({intervals[0][0], intervals[0][1]});
	
	        for(int i = 1; i < n; ++i){
	            auto now = ans.back();
	            if(intervals[i][0] <= now[1]){
	                ans.back()[1] = max(now[1], intervals[i][1]);
	            }else{
	                ans.push_back({intervals[i][0], intervals[i][1]});
	            }
	        } 
	        return ans;
	    }
	};


# 62 #
# 64 #
# 70 #
# 72 #
# 75 #
# 76 #
# 78 #
# 79 #
# 84 #
# 85 #
# 94 #
# 96 #
# 98 #
# 101 #
# 102 #
# 104 #
# 105 #
# 114 #
# 121 #
# 124 #
# 128 #
# 136 #
# 139 #
# 141 #
# 142 #
# 146 #
# 148 #
# 152 #

连续子数组的最大乘积

## DP ##

	class Solution {
	public:
	   
	    int maxProduct(vector<int>& nums) {
	        int n = nums.size(),t = INT_MIN;
	        //vector< vector<int> > dp(n+1, vector<int>(2));
	        vector<int> maxdp(n), mindp(n);
	        maxdp[0] = mindp[0] = nums[0];
	        for(int i = 1; i < n ;++ i){
	            maxdp[i] = max(max(maxdp[i-1] * nums[i],  nums[i]) , mindp[i-1] * nums[i]);
	             mindp[i] = min(min(mindp[i-1] * nums[i],  nums[i]) , maxdp[i-1] * nums[i]);
	        }
	        
	        return *max_element(maxdp.begin(), maxdp.end());
	    }
	};

## 滚动数组 ##

	class Solution {
	public:
	   
	    int maxProduct(vector<int>& nums) {
	        int n = nums.size(),t = INT_MIN; 
	        int l = nums[0],r = nums[0],ans = nums[0],ll,rr;
	        for(int i = 1; i < n ;++ i){
	           
	            ll = l;rr = r;
	            l = max(max(rr*nums[i],ll*nums[i]),nums[i]);
	            r = min(min(rr*nums[i],ll*nums[i]),nums[i]);
	            ans = max(ans,l);
	        }
	        
	        return ans;
	    }
	};

## 奇数个数 ##

	class Solution {
	public:
	    int maxProduct(vector<int>& nums) {
	        int n = nums.size();
	        vector<int> rnums(nums);
	        reverse(nums.begin(), nums.end());
	        
	        for(int i = 1; i < n; ++ i){
	            nums[i] *= nums[i-1] ? nums[i-1] : 1;
	            rnums[i] *= rnums[i-1] ? rnums[i-1] : 1;
	        }
	        return max(*max_element(nums.begin(), nums.end()) , *max_element(rnums.begin(), rnums.end()));
	    }
	};

# 155 #
# 160 #
# 169 #
# 198 #
# 200 #
# 206 #
# 207 #
# 208 #
# 215 #
# 221 #
# 226 #
# 234 #
# 236 #
# 238 除自身以外数组的乘积 #
不能用除法很难受。
类似于笔试题。预处理下。

## 预处理 -> 时间复杂度O（n), 空间 O（n) ##
	
	class Solution {
	public:
	    vector<int> productExceptSelf(vector<int>& nums) {
	
	        int n = nums.size();
	        vector<int> l(n),r(n);
	        l[0] = nums[0];
	        for(int i = 1; i < n; ++ i)l[i] = l[i-1] * nums[i];
	        r[n-1] = nums[n-1];
	        for(int i = n-2; i >= 0; -- i)r[i] = r[i+1] * nums[i];
	
	        vector<int> ans;
	        for(int i = 0; i < n; ++ i){
	            if(i == 0)
	                ans.push_back(r[1]);
	            else if(i == n-1)
	                ans.push_back(l[n-2]);
	            else{
	                ans.push_back(l[i-1] * r[i+1]);
	            }
	        }
	        return ans;
	
	    }
	};
	

## O（1） ##
	
	class Solution {
	public:
	    vector<int> productExceptSelf(vector<int>& nums) {
	
	        int n = nums.size();
	        vector<int> l(n);
	        if(n == 1){
	            l[0] = nums[0];
	            return l;
	        }
	        
	        int r = 1;
	        l[0] = nums[0];
	        for(int i = 1; i < n; ++ i)l[i] = l[i-1] * nums[i];
	        for(int i = n-1; i >= 1; -- i){
	            l[i] = l[i-1] * r;
	            r *= nums[i];
	        }
	        l[0] = r;
	        return l;
	
	    }
	};



# 239 #
# 240 #
# 253 #
# 279 #
# 283 #

移动0到后面，不动非零数的相对顺序

双指针。i表示零序列的开头，j是非零的开头。
i左边的都是非零数，i和j之间的是0，j之后的是待处理数据 

	class Solution {
	public:
	    void moveZeroes(vector<int>& nums) {
	        int n = nums.size(), i = 0,j = 0;
	        while(j < n){
	            if(nums[j]){
	                swap(nums[i], nums[j]);
	                ++ i;
	            }
	            ++ j;
	        }
	    }
	};


# 287 #
# 297 #
# 300 #
# 301 #
# 309 #
# 312 #
# 322 #
# 337 #
# 338 #
# 347 #
# 394 #
# 399 #
# 406 #
# 416 #
# 437 #
# 438 #
# 448 #
# 461 #
# 494 #
# 538 #
# 543 #
# 560 #
# 581 #
# 617 #
# 621 #
# 647 #
# 739. 每日温度   #
[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
基础的单调栈。
位置在i所能积水的深度是有i-1和i+1两个边界围成的。
维护一个单调栈。
当当前值大于栈顶值，需要当前栈顶指对应的高度。


	class Solution {
	public:
	    vector<int> dailyTemperatures(vector<int>& T) {
	        
	        int n = T.size();
	        vector<int> ans(n);
	        stack<int> s;
	        for(int i = 0; i < n; ++ i){
	            while( !s.empty() && T[s.top()] < T[i]){
	                ans[s.top()] = i - s.top();
	                s.pop();
	            }
	            s.push(i);
	        }
	        return ans;
	    }
};


----------

# MS 20210328 #

## 第一题 : ##  
level:E
很简单。背景是渲染，给一个数组D[i]，代表着位置i离原点的距离。给一个数组C[i]，表示渲染位置i所需要的算力。给定一个P。表示总共的算力，每渲染一个，P -= c[i]，要求，从原点出发，按照D[i]的大小，依次渲染，最多能渲染多少个。

排序后扫一遍。

	struct node{
	int d,c;
	};
	
	int cmp(node x,node y){
	if(x.d == y.d)return x.c < y.c;
	return x.d < y.d;
	}
	
	sort(a.begin(),a.end(),cmp);
	for(int i =0; i < n; ++ i){
		if(a[i].c > P)break;
		P -= a[i].c;
		ans ++;
	}
	return ans;

## 第二题： ##

level:M
给一个数组a[n]，里面有n个元素。给一个k，表示从数组a中删除连续的k个元素。剩下的元素里，最大值与最小值为d，求d的最小值是多少。
3 <= n <= 100000


对最值进行预处理：

1. lmin[i]表示，从a[0]到a[i]的最小值
2. lmax[i]表示，从a[0]到a[i]的最大值
3. rmin[i]表示，从a[i]到a[n-1]的最小值
4. rmax[i]表示，从a[i]到a[n-1]的最大值

			lmin[0] = lmax[0] = a[0];
			for(int i = 1; i < n; ++ i){
				lmin[i] = min(lmin[i-1],a[i])
				lmax[i] = max(lmax[i-1],a[i])
			}
			rmin[n-1] = rmax[n-1] = a[n-1];
			for(int i = n-2; i >= 0 ; -- i){
				rmin[i] = min(rmin[i+1],a[i])
				rmax[i] = max(rmax[i+1],a[i])
			}

再扫一遍：
	
	for(int i = 0; i < n; ++ i){
		j = i + k - 1;
		if(i == 0)ans = min(ans,(rmax[j+1] - rmin[j+1]));
		else if(j == n-1)ans = min(ans, lmax[i-1] - lmin[i-1]); 
		else{
			now = max(rmax[j+1],lmax[i-1]) - min(lmin[i-1],rmin[j+1]);
			ans = min(ans, now);
		}

## 第三题： ##

level:M
给一系列坐标点a[n]，和一个m值。组成一个m-集合。要求，该集合内，任意两个坐标点的距离，都能被m整除。

先把值都mod m;
	
	unordered_map<int,int> mp;
	for(int i = 0; i < n; ++ i){
		a[i] %= m;
		if(a[i] < 0)a[i] += m; //对负数进行处理.
		mp[a[i]]++;	
	}
	for(int i = 0; i < m; ++ i)
		ans = max(ans, mp[i]);
	






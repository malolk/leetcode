## NO.77. Combinations
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.
For example,
	If n = 4 and k = 2, a solution is:
	[
		[2,4],
		[3,4],
		[2,3],
		[1,2],
		[1,3],
		[1,4],
	]

## 思路
	使用递归，每次选定一个数，然后选择余下的k-1个数，下次循环时则不会考虑这个数。

## 实现

```
	class Solution {
		public:
			vector<vector<int>> combineImpl(vector<int> vec, int k)
			{
				vector<vector<int>> ret;
				if(k == 0) return ret;
				while(vec.size() >= k)
				{   
					int tmp = vec[0];
					vec.erase(vec.begin());
					vector<vector<int>> retTmp = combineImpl(vec, k - 1); 
					if(retTmp.empty())
						ret.push_back(vector<int>(1, tmp)); 
					for(int i = 0; i < retTmp.size(); ++i)
					{
						retTmp[i].push_back(tmp);
						ret.push_back(retTmp[i]);    
					}
				}   
				return ret;
			}
			vector<vector<int>> combine(int n, int k) {
				vector<int> in(n, 0);
				for(int i = 1; i <= n; ++i) in[i - 1] = i;
				return combineImpl(in, k);
			}
	};
```

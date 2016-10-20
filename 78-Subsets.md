## NO.78.Subsets

Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:

[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

## 思路
将产生的subset由元素个数多少进行分组，每一组都可以由前一组得到，比如{[1, 3], [1, 2], [2, 3]}可以由{[1],   [2], [3]}得到。第一个组为空集，所以只需要进行n次迭代就可以得到所有的subset，为了保证新生成的subset不重复，在上一个subset中加入新元素时只能加入比已有元素大的单个元素。比如，subset [1]则只能加入2, 3得到[1, 2], [1, 3], [2]则只能加入3 得到[2, 3], 不能由[3]得到下一个subset。
优化： 实际迭代的次数为（nums.size() + 1）/ 2。后续的subset可以通过求取前面subset的补集得到。注意边界值，迭代到（nums.size() + 1）/2可以保证所有的值都可以得到，但是注意只有在补集的大小大于现在生成的subset大小时才生成补集。

## 实现
```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ret;
        vector<vector<int>> retPart;
        ret.push_back({});
        int start = 0, end = 0, half = (nums.size() + 1) / 2;
        for(int i = 0; i < half; i++) 
        {
            for(; start <= end; ++start)
            {
                vector<int> tmp(ret[start].begin(), ret[start].end());
                set<int> checker(tmp.begin(), tmp.end());
                vector<int> tmpPart;
                bool flag = (nums.size() > 2 * tmp.size() + 1);
                for(int j = 0; j < nums.size(); ++j)
                {
                    if(tmp.empty() || nums[j] > tmp.back())
                    {
                        ret.push_back(tmp);
                        ret.back().push_back(nums[j]);
                    }
                    if(flag && !checker.count(nums[j]))
                        tmpPart.push_back(nums[j]);
                }
                if(flag) retPart.push_back(tmpPart);
            }
            start = ++end;
            end = ret.size() - 1;
        }
        ret.insert(ret.end(), retPart.begin(), retPart.end());
        return ret;
    }
};
```
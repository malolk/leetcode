## NO.96. Unique Binary Search Trees 

Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.
```

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```


## 思路
依次选取每一个数字作为根节点，然后对左边和右边的节点分别进行递归调用，则以此数字为根节点的树的个数为左边节点和右边节点生成树的乘积。
也可以使用公式推导来实现。

## 实现
```
class Solution {
public:
    int impl(int n, vector<int>& map)
    {
        if(map[n] >= 0) return map[n];
        int sum = 0;
        for(int i = 1; i <= n; i++)
        {
            int leftPart = impl(i - 1, map);
            leftPart = (leftPart == 0) ? 1 : leftPart;
            int rightPart = impl(n - i, map);
            rightPart = (rightPart == 0) ? 1 : rightPart;
            sum += leftPart * rightPart;
        }
        map[n] = sum;
        return sum;
    }
    int numTrees(int n) {
       if(n <= 2) return n;
       vector<int> map(n + 1, -1);
       map[0] = 0; map[1] = 1; map[2] = 2;
       return impl(n, map);
    }
};
```

```
class Solution {
public:
    int numTrees(int n) {
       if(n <= 2) return n;
       vector<int> map(n + 1, 0);
       map[0] = 1; map[1] = 1; map[2] = 2;
       for(int i = 3; i <= n; ++i)
          for(int j = 1; j <= i; ++j)
            map[i] += map[j - 1] * map[i - j];
       return map[n];
    }
};
```
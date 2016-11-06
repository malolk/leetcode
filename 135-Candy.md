## NO.135. Candy

There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:
```
    Each child must have at least one candy.
    Children with a higher rating get more candies than their neighbors.
```

What is the minimum candies you must give?

## 思路
1. ratings[i]和ratings[i - 1]有三种情况，
	- 大于，第i个小孩可以获得的candies为第（i-1）个小孩获得的candies数再加上1, 这样可能有点武断，因为后面可能有很多小孩，他们的ratings递减且都比第i个小孩的rating低，而且这些小孩的数目大于第i个小孩的candies数，此种情况属于第三种情况，可以将其修正。
	- 等于，第i个小孩可以获得的candies数为1,同样这样会比较武断，会遇到第一种情况遇到的问题，但是可以通过第三种情况将其修正。
	- 小于，因此形成了一个递减的序列，可以将其看作一个递增的序列，糖果数从1开始递增直到递减序列结束，递减序列结束时，此时的糖果数应该不大于ratings[i - 1]获得的糖果数，如果大于，说明之前给ratings[i - 1]的糖果数太少，应该将其修正。

2. 可以通过先进行左边遍历，保证每个小孩所得到的糖果数与其左边相比是满足题意的，然后进行右边遍历，保证每个小孩所得到的糖果数与其右边相比是满足题意的。

## 实现
```
time comlexity O(n), space complexity O(1)
class Solution {
public:
    int candy(vector<int>& ratings) {
        int size = ratings.size();
        int sum = 1;
        int curCandyNum = 1;
        for(int i = 1; i < size; )
        {
            if(ratings[i] > ratings[i - 1])
            {
                curCandyNum++;
                sum += curCandyNum;
                i++;
            }
            else if(ratings[i] == ratings[i - 1])
            {
                curCandyNum = 1;
                sum += curCandyNum;
                i++;
            }
            else
            {
                int prevCandy = curCandyNum;
                curCandyNum = 1;
                sum += curCandyNum;
                while(i < size && ratings[i] < ratings[i - 1])
                {
                    curCandyNum++;
                    sum += curCandyNum;
                    i++;
                }
                sum -= curCandyNum < prevCandy ? curCandyNum : prevCandy;
                curCandyNum = 1;
            }
        }
        return sum;
    }
};
```

```
time comlexity O(n), space complexity O(n)
class Solution {
public:
    int candy(vector<int>& ratings) {
        int size = ratings.size();
        int sum = 0;
        vector<int> candies(size, 1);
        for(int i = 1; i < size; ++i)
        {
            if(ratings[i] > ratings[i - 1])
                candies[i] = candies[i - 1] + 1;
        }
        for(int i = size - 2; i >= 0; --i)
        {
            if(ratings[i] > ratings[i + 1]) 
                candies[i] = max(candies[i], candies[i + 1] + 1);
        }
        for(int i = 0; i < size; i++)
            sum += candies[i];
        
        return sum;
    }
};
```


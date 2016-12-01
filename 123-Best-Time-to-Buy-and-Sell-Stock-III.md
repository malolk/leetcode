## 123. Best Time to Buy and Sell Stock III

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

## Solutions
__两次DP算法__
选取数组中的某一个分界点，比如说为i，那么可以计算在i左边的序列进行一次交易获得的最大收益leftM, 同时可以计算在i右边的序列进行一次交易获得的最大收益rightM，则通过i
点进行分割的两次交易的值为`localM = leftM + rightM`, 对数组中每个点进行类似的操作，则可以获得最大的localM。
值得注意的地方是，如果分割点取得是最左边的点，那么可以认为在分割点左边的最大收益为0, 同理当分割点在最右边时，在分割点右边的最大收益为0。另外每次针对每个分割点进行计算将会是`O(N^2)`的时间复杂度，可以使用两次遍历分别将每个分割点的左边收益和右边收益存储在数组中，其中每次遍历将会是`O(N)`的复杂度，为什么可已将时间复杂度降低，这是因为当在计算`i`分割点的最大左边收益`left[i]`时，可以利用已经计算的`i - 1`分割点的最大左边收益进行推导，`left[i] = max(cur_price - leftMin, left[i - 1])`，避免了重复进行运算。
算法中一个技巧是，将两种情况，即一次交易或两次交易，转换为统一的情况，即进行两次交易，其中一次交易的情况作为特殊情况也被涵盖在其中。
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if(!size) return 0;
        int sum = 0;
        vector<int> left(size, 0), right(size, 0);
        int leftMin = prices[0], rightMax = prices[size - 1];
        for(int i = 1; i < size; ++i)
        {
            leftMin = min(leftMin, prices[i]);
            left[i] = max(prices[i] - leftMin, left[i - 1]);
        }
        
        for(int i = size - 2; i >= 0; --i)
        {
            rightMax = max(rightMax, prices[i]);
            right[i] = max(rightMax - prices[i], right[i + 1]);
        }
        
        for(int i = 0; i < size; ++i)
        {
            int tmp = left[i] + right[i];
            if(tmp > sum) sum = tmp;
        }
        
        return sum;
    }
};
```

__使用状态轮转__
在数组中的每一天都会有四种情况发生，第一次买，第一次卖，第二次买，第二次卖，每次进行这四种操作时都应该查看进行交易后手中还有多少利润。第二次买基于第一次卖，第二次卖基于第二次买，为了计算每个点，必须预先设置初始值。

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if(!size) return 0;
        int oneBuy = INT_MIN, oneSell = 0, twoBuy = INT_MIN, twoSell = 0;
        for(int i = 0; i < prices.size(); ++i)
        {
            twoSell = max(twoBuy + prices[i], twoSell);
            twoBuy = max(oneSell - prices[i], twoBuy);
            oneSell = max(oneBuy + prices[i], oneSell);
            oneBuy = max(-prices[i], oneBuy);
        }
        return twoSell;
    }
};
```

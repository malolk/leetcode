## NO.122. Best Time to Buy and Sell Stock II
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

## 思路
方法与日常买股票的思路基本是一致的，即低买高卖，回到此题首先找到第一个最低点买入，然后找到相邻的最高点卖出即可得到这一次的利润，接着重复上诉的过程即可求得最佳的买进卖出策略。
其中值得注意的是，当确定最低点时一定要保证后续还有存在的较高的卖出点。

## 实现
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ret = 0;
        int buyPrice = 0;
        int i = 0, size = prices.size();
        while(i < size - 1)
        {
            while((i + 1 < size) && prices[i] >= prices[i + 1]) ++i;
            if(i == size - 1) break;
            buyPrice = prices[i];
            while((i + 1 < size) && prices[i] <= prices[i + 1]) ++i;
            ret += prices[i] - buyPrice;
        }
        return ret;
    }
};
```
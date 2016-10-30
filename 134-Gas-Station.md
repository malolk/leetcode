## NO.134 Gas Station

There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

Note:
The solution is guaranteed to be unique. 

## 思路
使用贪心算法进行求解。

	1.首先寻找第一个station A使得该站的汽油量足够使小汽车跑到下一个站点，如果所有的站点都不满足这个要求则返回-1。接着继续遍历后续站点B、C、D，使得之前累积的汽油量和当前站点的汽油量足够使得小汽车跑到下一个站点。
	2. 如果在A之后的站点都满足这些要求，还需要验证跑完这些站点之后的汽油量能否使得小汽车从0到A。
	3. 如果在A之后的站点E不能到达下一个站点，那么可以推导出从B、C、D分别开始都不能使得站点E到达下一个站点，因为A到B累积的汽油量是大于等于0的，因此从A到E累积的汽油量是比从B到E累积的汽油量更多，因此从B开始也不能使得站点E到下一个站点，同样分别从C或者D开始也不能使得站点E到达下一个站点。此时若E点是最后一个站点或者最后一个站点之前的站点，则重新从E点的下一个站点开始执行步骤1。

## 实现
```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int size = gas.size();
        int start = 0, i = 0;
        int left_sum = 0, right_sum = 0;
        while(true)
        {
            while(start < size && gas[start] < cost[start])
            {
                left_sum += gas[start] - cost[start];
                ++start;
            }
            if(start == size) break;;
            right_sum = 0; i = start;
            while(i < size && right_sum + gas[i] >= cost[i]) 
            {
                right_sum += gas[i] - cost[i];
                ++i;
            }
            left_sum += right_sum;
            if(i == size && left_sum >= 0) return start;
            else if(i == size && left_sum < 0) break;
            else start = i + 1;
        }
        return -1;
    }
};
```
	
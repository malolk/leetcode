## NO.69. Sqrt(x) 
Implement int sqrt(int x).

Compute and return the square root of x.

## 思路
使用牛顿逼近法。需要注意的是对于较大的整数要防止乘积溢出，使用long long进行存储。

## 实现
```
class Solution {
public:
    int mySqrt(int x) {
        if(x == 0 || x == 1) return x;
        long long k = x;
        while(true)
        {
            //k = k - (k*k - x)/(2*k);
            k = (k + x/k)/2;
            if(k*k <= x) break;
        }
        return k;
    }
};
```
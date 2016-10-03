## Problem
Divide two integers without using multiplication, division and mod operator.
If it is overflow, return MAX\_INT.
## 思路
dividend可以写成如下的式子:`dividend = divisor*(2^K + 2^M + 2^N...) + C `, 其中C是所除的余数。具体的接替步骤如下。
1. 判断dividend和divisor是否相等，若相等直接返回1。
2. 使用异或判断结果的正负。
3. 然后对要处理的dividend和divisor求取其绝对值，值得注意的是INT_MIN是无法使用abs直接转换的。首先判断若divisor为INT_MIN则返回0, 否则将其直接进行abs转换; 再判断若dividend为INT_MIN则将dividend加上divisor，在result中加1, 然后对dividend进行绝对值转换。
4. 找出不大于dividend的最大k使得`divisor*2^k<=dividend`。
5. 将dividend减去`divisor*2^k`, 等价于在dividend基础上减去了`1<<k`个divisor, 将此个数加入result中。
6. 判断此时的dividend是否大于等于divisor，若是则循环4,5步，直到dividend小于divisor。
## 出现的问题
首先应该明确有符号数之间进行相除时出现的溢出情况，只有两种，一种是除数为0, 另一种是被除数是INT_MIN, 除数为-1, 相当于是求INT_MIN的绝对值，由于INT_MIN的绝对值是比INT_MAX大1,所以会溢出。
可以优化的地方：先求出最大的k值，然后利用二分法求取0到k之间最大的k_next使得`divisor*2^k<=dividend`，在下一个循环使用k_next作为新的k值。
## 实现代码
`
class Solution {
	public:
		int divide(int dividend, int divisor) {
			if(divisor == 0 || (dividend == INT_MIN && divisor == -1)) return INT_MAX;
			else if(dividend == divisor) return 1;
			bool sign = (dividend < 0) ^ (divisor < 0);
			int extra = 0;
			if(divisor < 0 && divisor == INT_MIN) return 0;
			divisor = abs(divisor);
			if(dividend < 0 && dividend == INT_MIN)
				dividend += divisor,  extra = 1;
			dividend = abs(dividend);
			int tmp = divisor, step = 0, result = 0;
			for(; ;)
			{
				if((dividend - tmp) < tmp) break;
				tmp = divisor<< ++step;
			}
			for(int rem = dividend; rem >= divisor; --step)
			{
				if(divisor << step <= rem) 
					result += 1 << step, rem -= divisor << step;
			}
			return sign ? (-result - extra) : (result + extra);
		}
};
`

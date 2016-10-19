## NO.72. Edit Distance
Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character
b) Delete a character
c) Replace a character

## 思路
使用一个二维数组表示长度为i的字符串到长度为j的字符串的最小距离为dpMap[i][j], 它表示字符串(0...i - 1) 到字符串(0...j-1)的最小距离。对于边界情况，有一个字符串为空，则最小距离为另一个字符串的长度。假设给定字符串的长度分别为sizeN和sizeM，那么我们需要求得dpMap[sizeN][sizeM], 因此dpMap的维度为（sizeN + 1, sizeM + 1）。
对于边界情况，根据dpMap语义则可以将dpMap[0][i]==i, dpMap[i][0]==i。
对于一般情况，i >= 1 && j >= 1。假设我们已经知道dpMap[i - 1][j - 1], 那么接下来我们判断word1[i - 1]是否与word2[j - 1]相等, 以及判断转换到dpMap的各种可能。
	1. 相等， dpMap[i][j] == dpMap[i-1][j-1], 
					#note dpMap[i - 1][j - 1] <= dpMap[i][j - 1] + 1;
                    // && dpMap[i - 1][j - 1] <= dpMap[i - 1][j] + 1;
                    // otherwise, we could transfer to dpMap[i][j - 1], then dpMap[i - 1][j - 1], 
                    // but now the dpMap[i - 1][j - 1] won't be the minimum distance;
	2. 不相等，则有三种可能性。
		- 直接将对应字符替换， dpMap[i][j] == dpMap[i-1][j-1] + 1;
		- 将字符word1[i - 1]删除，为了得到dpMap[i][j], 则需要求得dpMap[i - 1][j]再加上删除操作，则可得出dpMap[i][j] = dpMap[i - 1][j] + 1， 它表示word1[i-1]是被删除的;
		- 将字符word1[i - 1]之前插入word2[j - 1], 为了得到dpMap[i][j], 则需要求得dpMap[i][j - 1]再加上插入操作，则可得出dpMap[i][j] = dpMap[i][j - 1] + 1, 它表示word[i - 1]之前被插入word[j - 1];

## 实现
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int sizeN = word1.size(), sizeM = word2.size();
        vector<vector<int>> dpMap(sizeN + 1, vector<int>(sizeM + 1, 0));
        for(int i = 1; i <= sizeN; ++i)
            dpMap[i][0] = i;
        for(int i = 1; i <= sizeM; ++i)
            dpMap[0][i] = i;
        for(int i = 1; i <= sizeN; ++i)
            for(int j = 1; j <= sizeM; ++j)
            {
                if(word1[i - 1] == word2[j - 1])
                    // dpMap[i - 1][j - 1] <= dpMap[i][j - 1] + 1;
                    // && dpMap[i - 1][j - 1] <= dpMap[i - 1][j] + 1;
                    // otherwise, we could transfer to dpMap[i][j - 1], then dpMap[i - 1][j - 1], 
                    // but now the dpMap[i - 1][j - 1] won't be the minimum distance
                    dpMap[i][j] = dpMap[i - 1][j - 1];
                else dpMap[i][j] = min(dpMap[i - 1][j - 1] + 1, 
                                    min(dpMap[i - 1][j] + 1, dpMap[i][j - 1] + 1));
            }
        return dpMap[sizeN][sizeM];
    }
};
```
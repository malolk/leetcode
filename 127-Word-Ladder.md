## NO.127 Word Ladder

Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:
	1. Only one letter can be changed at a time
    2. Each intermediate word must exist in the word list

For example,
Given:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.

Note:

    1. Return 0 if there is no such transformation sequence.
    2. All words have the same length.
    3. All words contain only lowercase alphabetic characters.

## 思路
1. 每次只能改变一个字符，因此可以将当前字符串改变其中一个字符串后且在wordList中的字符串当作当前字符串的邻节点，使用队列存储邻节点。每次将当前字符串的邻节点处理完后会将转换次数加1,在处理邻节点时，判断是否为最终节点，如果是则返回转换次数（转换次数以2开始），如果不是最终邻节点，则将此邻节点的邻节点存储在队列后面供下一轮回调用。如果邻节点队列为空，则说明不存在从beginWord到endWord的转换。

2. 对前一种方法进行加速，分别从两边开始进行搜寻，令左边的节点为beginSet, 右边的节点为endSet，每次对数目小的那端进行扩展，每扩展一个节点，检查节点是否在对面的节点集中。

## 实现
```
class Solution {
public:
    struct Elem
    {
        string val;
        int step;
        Elem(string str, int num): val(str), step(num) {}
    };
    int ladderLength(string beginWord, string endWord, unordered_set<string>& wordList) {
        wordList.insert(endWord);
        wordList.erase(beginWord);
        deque<Elem> unVisited;
        Elem item(beginWord, 1);
        unVisited.push_back(item);
        while(!unVisited.empty())
        {
            Elem cur = unVisited.front();
            unVisited.pop_front();
            for(int i = 0; i < beginWord.size(); ++i)
            {
                char old = cur.val[i];
                for(int j = 0; j < 26; ++j)
                {
                    cur.val[i] = 'a' + j;
                    unordered_set<string>::iterator it = wordList.find(cur.val);
                    if(it != wordList.end())
                    {
                        item.val = cur.val;
                        item.step = cur.step + 1;
                        if(item.val == endWord) return item.step;
                        unVisited.push_back(item);
                        wordList.erase(it);
                    }
                }
                cur.val[i] = old;
            }
        }
        return 0;
    }
};

class Solution {
public:
    int ladderLength(string beginWord, string endWord, unordered_set<string>& wordList) {
        wordList.erase(beginWord);
        wordList.erase(endWord);
        unordered_set<string> beginSet = {beginWord};
        unordered_set<string> endSet = {endWord};
        int ret = 1;
        while(!beginSet.empty() && !endSet.empty())
        {
            if(beginSet.size() > endSet.size())
                swap(beginSet, endSet);
            unordered_set<string> temp;
            ret++;
            for(unordered_set<string>::iterator it = beginSet.begin(); it != beginSet.end(); ++it)
            {
                string cur = *it;
                for(int i = 0; i < beginWord.size(); ++i)
                {
                    char old = cur[i];
                    for(int j = 0; j < 26; ++j)
                    {
                        cur[i] = 'a' + j;
                        if(endSet.count(cur)) return ret;
                        unordered_set<string>::iterator target = wordList.find(cur);
                        if(target != wordList.end())
                        {
                            wordList.erase(target);
                            temp.insert(cur);
                        }
                    }
                    cur[i] = old;
                }
            }
            swap(temp, beginSet);
        }
        return 0;
    }
};
```
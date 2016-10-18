## NO.68. Text Justification

 Given an array of words and a length L, format the text such that each line has exactly L characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly L characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

For example,
words: ["This", "is", "an", "example", "of", "text", "justification."]
L: 16.

Return the formatted lines as:

[
   "This    is    an",
   "example  of text",
   "justification.  "
]

Note: Each word is guaranteed not to exceed L in length. 

## 思路
1. 找出这一行能够放下哪些word
2. 判断该行是否是最后一行，如果是最后一行则只需要每个word之间有一个空格就可以，如果是普通行则需要将word之间填充多余的

## 实现
```
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        if(maxWidth == 0) return {""};
        int wSize = words.size(), start = 0, end = 0;
        vector<string> ret;
        while(end < wSize)
        {
            int sumLen = words[end].size();
            while(end < wSize - 1)
            {
                int tmp = words[end + 1].size() + 1;
                if(tmp + sumLen > maxWidth) break;
                sumLen += tmp;
                end++;
            }
            string line;
        
            if(end == wSize - 1)
            {
                line.append(words[start]);
                for(int i = start + 1; i <= end; ++i)
                {
                    line.append(" ");
                    line.append(words[i]);
                }
                line.append(maxWidth - sumLen, ' ');
                ret.push_back(line);
                break;
            }
            if(end == start) 
            {
                line.append(words[end]);
                line.append(maxWidth - words[end].size(), ' ');
                ret.push_back(line);
            }
            else
            {
                sumLen -= end - start;
                int spaceNum = (maxWidth - sumLen)/(end - start);
                int extraNum = (maxWidth - sumLen)%(end - start);
                line.append(words[start]);
                for(int i = start + 1; i <= end; ++i)
                {
                    int extra = extraNum > 0 ? (extraNum--, 1) : 0;
                    line.append(spaceNum + extra, ' ');
                    line.append(words[i]);
                }
                ret.push_back(line);
            }
            start = ++end;
        }
        return ret;
    }
};
```
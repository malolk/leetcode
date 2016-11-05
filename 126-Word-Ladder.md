## NO.126 Word Ladder II

Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

    1. Only one letter can be changed at a time
    2. Each intermediate word must exist in the word list

For example,
Given:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Return
```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```
Note:
    1. All words have the same length.
    2. All words contain only lowercase alphabetic characters.

## 思路
此题与Word Ladder II不同的是找出所有的最短转换路径，在生成下一层的邻节点时不能直接将匹配的word从wordList中删除，因为有可能两条不同的转换路径会转换为同一个中间节点。
首先使用BST生成从beginWord到endWord的最短路径图，然后使用DFS进行递归找出所有到endWord的转换路径。BST方法是逐层生成邻节点，记录每个节点的前缀节点，当每层邻节点生成完毕，将这些节点从wordList中删除，因为后面即使再遇到只会是更长的转换路径。使用一个map记录已经visited的map，记录相应的word和其对应的Node，Node中可以记录前缀节点。当判断当前节点为endWord时，则将touched置为true，当本层节点生成完毕后，则从endWord进行DFS生成转换路径。

## 实现
```
class Solution {
public:
    struct Node
    {
        string val;
        vector<Node*> prev;
        Node(string str) : val(str) {} 
    };

    void getAllPaths(Node* endNode, vector<string>& buf, vector<vector<string>>& ret)
    {
        if(endNode->prev.empty())
        {
            if(!buf.empty()) ret.push_back(vector<string>(buf.rbegin(), buf.rend()));
            return;
        }
        for(int i = 0; i < endNode->prev.size(); ++i)
        {
            Node* node = endNode->prev[i];
            buf.push_back(node->val);
            getAllPaths(node, buf, ret);
            buf.pop_back();
        }
    }
    vector<vector<string>> findLadders(string beginWord, string endWord, unordered_set<string> &wordList) {
        wordList.erase(beginWord);
        wordList.insert(endWord);
        unordered_map<string, Node*> map;
        Node* begin = new Node(beginWord);
        map.emplace(beginWord, begin);
        vector<vector<string>> ret;
        deque<Node*> unVisited = { begin };
        bool touched = false;
        while(unVisited.size())
        {
            int size = unVisited.size();
            vector<unordered_set<string>::iterator> visited;
            for(int i = 0; i < size; ++i)
            {
                Node* curNode = unVisited.front();
                unVisited.pop_front();
                string cur = curNode->val;
                for(int j = 0; j < beginWord.size(); ++j)
                {
                    char old = cur[j];  
                    for(int c = 0; c <  26; ++c)
                    {
                        cur[j] = 'a' + c;
                        unordered_set<string>::iterator it = wordList.find(cur);
                        if(it != wordList.end())
                        {
                            unordered_map<string, Node*>::iterator itN = map.find(cur);
                            if(itN == map.end())
                            {
                                Node* n = new Node(cur);
                                n->prev.push_back(curNode);
                                map.emplace(cur, n);
                                if(cur == endWord) touched = true;
                                else
                                    unVisited.push_back(n);
                                visited.push_back(it);
                            }
                            else
                                itN->second->prev.push_back(curNode);
                        }
                    }
                    cur[j] = old;
                }
            }
            if(touched)
            {
                vector<string> buf = { endWord };
                getAllPaths(map[endWord], buf, ret);
                return ret;
            }
            for(auto item : visited)
                wordList.erase(item);
        }
        return ret;
    }
};

```




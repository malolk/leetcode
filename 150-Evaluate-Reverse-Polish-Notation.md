## NO.150. Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Some examples:
```
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
```

## 思路
每当输入是操作符时就从stack中弹出两个操作数，stack中存放的string类型，因为要考虑复杂的数字类型，比如带负号的数字。当从stack弹出操作数时要考虑stack会将操作数的顺序颠倒过来，因此首先弹出来的是第二个操作数，其次弹出来的是第一个操作数。

## 实现
```
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        int size = tokens.size();
        stack<string> stk;
        for(int i = 0; i < size; ++i)
        {
            string str = tokens[i];
            if(str.size() == 1 && (str[0] == '+' || 
                str[0] == '-' || str[0] == '*' || str[0] == '/')) 
            {
                int operand1 = stoi(stk.top());
                stk.pop();
                int operand2 = stoi(stk.top());
                stk.pop();
                int result = 0;
                if(str[0] == '+') result = operand2 + operand1;
                else if(str[0] == '-') result = operand2 - operand1;
                else if(str[0] == '*') result = operand2 * operand1;
                else result = operand2 / operand1;
                stk.push(to_string(result));
            }
            else
                stk.push(str);
        }
        return stoi(stk.top());
    }
};
```

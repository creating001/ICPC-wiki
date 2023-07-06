# 表达式求值

> 板子题网址: https://www.acwing.com/problem/content/3305/

```cpp
stack<int> nums;
stack<char> ops;

unordered_map<char, int> table{
    {'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}
};

inline void eval() {
    int b = nums.top();
    nums.pop();
    int a = nums.top();
    nums.pop();
    int op = ops.top();
    ops.pop();
    int r;
    if (op == '+') r = a + b;
    if (op == '-') r = a - b;
    if (op == '*') r = a * b;
    if (op == '/') r = a / b;
    nums.push(r);
}

inline int solve(){
    string expression;
    cin >> expression;
    for (int i = 0; i < expression.size(); i++) {
        char ch = expression[i];
        if (isdigit(ch)) {
            int num = 0, j;
            for (j = i; isdigit(expression[j]); j++)
                num = num * 10 + expression[j] - '0';
            nums.push(num);
            i = j - 1;
        }
        else if (ch == '(') {
            ops.push('(');
        }
        else if (ch == ')') {
            while (ops.top() != '(') eval();
            ops.pop();
        }
        else {
            while (!ops.empty() && table[ops.top()] >= table[ch])
                eval();
            ops.push(ch);
        }
    }
    while (!ops.empty()) eval();
    return nums.top();
}
```

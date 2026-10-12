# 📌 Stack Data Structure

A stack is a linear data structure adhering to the Last-In, First-Out (LIFO) principle, where insertions and deletions happen exclusively at the top.

---

## 📖 Overview & Core Concepts

A stack models nested execution contexts (call stack), undo operations, and expression evaluation. In C++, `std::stack` is a container adaptor wrapping underlying sequential containers (default: `std::deque`).

### Core Operations:
- `push(x)`: Inserts element onto top ($O(1)$).
- `pop()`: Removes top element ($O(1)$).
- `top()`: Accesses top element without removal ($O(1)$).
- `empty()` / `size()`: State queries ($O(1)$).


---

## ⚡ Time & Space Complexity

| Operation | Time Complexity | Space Complexity |
|---|---|---|
| Push | $O(1)$ | $O(1)$ |
| Pop | $O(1)$ | $O(1)$ |
| Top (Peek) | $O(1)$ | $O(1)$ |
| Search by Value | $O(N)$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Valid Parentheses Matching
```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(') st.push(')');
        else if (c == '{') st.push('}');
        else if (c == '[') st.push(']');
        else {
            if (st.empty() || st.top() != c) return false;
            st.pop();
        }
    }
    return st.empty();
}
```

### 2. Monotonic Stack (Next Greater Element)
Maintains elements in strictly increasing or decreasing order. Solves range-extensibility in $O(N)$ total time:
```cpp
vector<int> nextGreaterElements(const vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n, -1);
    stack<int> st; // stores indices
    for (int i = 0; i < n; ++i) {
        while (!st.empty() && nums[i] > nums[st.top()]) {
            res[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    return res;
}
```

### 3. Min Stack ($O(1)$ Auxiliary Min)
Maintains a secondary stack or packs `pair<int, int>` to track the running minimum:
```cpp
class MinStack {
    stack<pair<int, int>> st; // {val, current_min}
public:
    void push(int val) {
        int currentMin = st.empty() ? val : min(val, st.top().second);
        st.push({val, currentMin});
    }
    void pop() { st.pop(); }
    int top() { return st.top().first; }
    int getMin() { return st.top().second; }
};
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <stack>
#include <vector>

std::stack<int> st;
st.push(10);
st.push(20);
int topVal = st.top(); // 20
st.pop();              // removes 20
bool empty = st.empty();

// Use std::vector as underlying container if contiguous buffer is preferred
std::stack<int, std::vector<int>> vecStack;
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Valid Parentheses | 🟢 Easy | LC 20 | Bracket Matching |
| Min Stack | 🟡 Medium | LC 155 | Dual Stack / Pair Stack |
| Evaluate Reverse Polish Notation | 🟡 Medium | LC 150 | Postfix Evaluation |
| Daily Temperatures | 🟡 Medium | LC 739 | Monotonic Decreasing Stack |
| Next Greater Element I | 🟢 Easy | LC 496 | Monotonic Stack + Hash Map |
| Online Stock Span | 🟡 Medium | LC 901 | Monotonic Stack with Span Weights |
| Largest Rectangle in Histogram | 🔴 Hard | LC 84 | Monotonic Stack Boundaries |
| Trapping Rain Water | 🔴 Hard | LC 42 | Monotonic Stack Boundary Fill |


---

## ⚠️ Common Pitfalls & C++ Tips

- Calling `st.pop()` or `st.top()` on an empty stack results in **Undefined Behavior**; always check `!st.empty()` first.
- Store indices instead of values in monotonic stacks when you need distance or span calculations.


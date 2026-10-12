# 📌 Recursion & Divide and Conquer

Recursion is a problem-solving technique where a function solves a problem by calling copies of itself on smaller instances of the same problem until a base condition is reached.

---

## 📖 Overview & Core Concepts

Every recursive solution requires:
1. **Base Case**: The simplest scenario that terminates recursion without further calls.
2. **Recursive Step**: Reduction of the input into subproblems that move toward the base case.
3. **Combination**: Aggregating subproblem results to solve the original instance.

### The Call Stack & Recurrence Relations:
Recursive calls utilize the system call stack. Recursion depth determines the space complexity.
- Master Theorem: $T(N) = aT(N/b) + O(N^d)$
  - If $d < \log_b a \implies O(N^{\log_b a})$
  - If $d = \log_b a \implies O(N^d \log N)$
  - If $d > \log_b a \implies O(N^d)$


---

## ⚡ Time & Space Complexity

| Paradigm | Time Complexity | Auxiliary Stack Space |
|---|---|---|
| Linear Recursion (Factorial, Fibonacci naive) | $O(N)$ to $O(2^N)$ | $O(N)$ |
| Divide & Conquer (Merge Sort) | $O(N \log N)$ | $O(\log N)$ |
| Binary Exponentiation | $O(\log N)$ | $O(\log N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Fast Power (Binary Exponentiation)
Computes $x^n$ in $O(\log n)$ time:
```cpp
double myPow(double x, long long n) {
    if (n == 0) return 1.0;
    if (n < 0) return 1.0 / myPow(x, -n);
    double half = myPow(x, n / 2);
    if (n % 2 == 0) return half * half;
    return half * half * x;
}
```

### 2. Tower of Hanoi
```cpp
void towerOfHanoi(int n, char from, char to, char aux) {
    if (n == 0) return;
    towerOfHanoi(n - 1, from, aux, to);
    cout << "Move disk " << n << " from " << from << " to " << to << "\n";
    towerOfHanoi(n - 1, aux, to, from);
}
```


---

## 🛠️ C++ STL & Implementations

C++ does not have a recursion container, but `std::function` or C++14 generic lambdas allow self-referential lambdas:
```cpp
auto dfs = [&](auto& self, int u) -> void {
    // base case
    self(self, u + 1);
};
dfs(dfs, 0);
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Pow(x, n) | 🟡 Medium | LC 50 | Binary Exponentiation |
| Fibonacci Number | 🟢 Easy | LC 509 | Base Case + Recurrence |
| Reverse String | 🟢 Easy | LC 344 | In-place Recursive Swap |
| K-th Symbol in Grammar | 🟡 Medium | LC 779 | Recursive Tree Traversal |


---

## ⚠️ Common Pitfalls & C++ Tips

- Ensure the base condition is reached in every branch to avoid infinite recursion and `Stack Overflow`.
- Beware of overlapping subproblems: naive recursion without memoization often leads to exponential $O(2^N)$ time.


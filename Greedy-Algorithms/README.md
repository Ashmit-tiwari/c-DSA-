# 📌 Greedy Algorithms

A greedy algorithm builds up a solution piece by piece, always choosing the next piece that offers the most immediate, local benefit, under the premise that local optimal choices lead to a global optimum.

---

## 📖 Overview & Core Concepts

Greedy algorithms succeed when a problem exhibits:
1. **Greedy Choice Property**: A globally optimal solution can be reached by making locally optimal decisions without having to reconsider past choices.
2. **Optimal Substructure**: An optimal solution to the problem contains optimal solutions to subproblems.

### Proving Correctness:
Greedy algorithms are fast ($O(N)$ or $O(N \log N)$), but not always applicable. Correctness is proven using:
- **Exchange Argument**: Show that any alternative optimal solution can be modified step-by-step into the greedy solution without degrading quality.
- **Greedy Stays Ahead**: Prove that at each step, the greedy choice is at least as good as any competing choice.


---

## ⚡ Time & Space Complexity

| Problem / Type | Time Complexity | Space Complexity |
|---|---|---|
| Activity Selection / Interval Scheduling | $O(N \log N)$ (sorting) | $O(1)$ |
| Fractional Knapsack | $O(N \log N)$ | $O(1)$ |
| Huffman Coding | $O(N \log N)$ | $O(N)$ |
| Jump Game | $O(N)$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Interval Scheduling (Non-Overlapping Intervals)
Sort intervals by **finish time** to maximize remaining room:
```cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return 0;
    sort(intervals.begin(), intervals.end(), [](const auto& a, const auto& b) {
        return a[1] < b[1]; // sort by end time
    });
    int count = 0, lastEnd = intervals[0][1];
    for (size_t i = 1; i < intervals.size(); ++i) {
        if (intervals[i][0] < lastEnd) {
            count++; // overlapping, remove this interval
        } else {
            lastEnd = intervals[i][1];
        }
    }
    return count;
}
```

### 2. Jump Game (Farthest Reachable Index)
```cpp
bool canJump(const vector<int>& nums) {
    int maxReach = 0;
    for (int i = 0; i < (int)nums.size(); ++i) {
        if (i > maxReach) return false;
        maxReach = max(maxReach, i + nums[i]);
    }
    return true;
}
```

### 3. Gas Station (Single Pass Circular Tour)
```cpp
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int totalGas = 0, currGas = 0, start = 0;
    for (int i = 0; i < (int)gas.size(); ++i) {
        int diff = gas[i] - cost[i];
        totalGas += diff;
        currGas += diff;
        if (currGas < 0) {
            start = i + 1;
            currGas = 0;
        }
    }
    return totalGas >= 0 ? start : -1;
}
```


---

## 🛠️ C++ STL & Implementations

Sorting (`std::sort`) and priority queues (`std::priority_queue`) are the foundation of greedy implementations in C++.

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Assign Cookies | 🟢 Easy | LC 455 | Two Pointers Greedy Matching |
| Jump Game | 🟡 Medium | LC 55 | Reachable Boundary Tracking |
| Jump Game II | 🟡 Medium | LC 45 | Current & Next Interval Boundaries |
| Gas Station | 🟡 Medium | LC 134 | Total Balance vs Current Segment |
| Hand of Straights | 🟡 Medium | LC 846 | Ordered Map / Min-Heap |
| Non-overlapping Intervals | 🟡 Medium | LC 435 | Sort by End Time |
| Merge Intervals | 🟡 Medium | LC 56 | Sort by Start Time |
| Candy | 🔴 Hard | LC 135 | Two-Pass Left & Right Greedy |


---

## ⚠️ Common Pitfalls & C++ Tips

- If a problem involves intervals, sorting by start or end time is almost always the first step.
- Verify whether fractional choices are allowed; 0/1 Knapsack CANNOT be solved greedily (requires Dynamic Programming).


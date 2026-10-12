# 📌 Bit Manipulation

Bit Manipulation involves algorithmic operations performed directly at the binary level using bitwise operators (`&`, `|`, `^`, `~`, `<<`, `>>`).

---

## 📖 Overview & Core Concepts

Operating directly on bits executes in single processor clock cycles, providing fast performance and low memory footprints.

### Bitwise Operators:
- `&` (AND): 1 if both bits are 1.
- `|` (OR): 1 if either bit is 1.
- `^` (XOR): 1 if bits differ ($x \oplus x = 0$, $x \oplus 0 = x$).
- `~` (NOT): Inverts all bits.
- `<<` (Left Shift): Multiplies by $2^k$.
- `>>` (Right Shift): Divides by $2^k$.

### Fundamental Bitwise Hacks:
| Hack | Formula | Purpose |
|---|---|---|
| Check $k$-th bit | `(n >> k) & 1` | Test if $k$-th bit is set |
| Set $k$-th bit | `n |= (1 << k)` | Set $k$-th bit to 1 |
| Clear $k$-th bit | `n &= ~(1 << k)` | Clear $k$-th bit to 0 |
| Toggle $k$-th bit | `n ^= (1 << k)` | Invert $k$-th bit |
| Check power of 2 | `(n > 0) && ((n & (n - 1)) == 0)` | True if only 1 bit set |
| Clear lowest set bit | `n &= (n - 1)` | Brian Kernighan's count |
| Isolate lowest set bit | `n & (-n)` | Extracts lowest set bit |


---

## ⚡ Time & Space Complexity

All primary bitwise operations execute in $O(1)$ time and $O(1)$ space.

---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Brian Kernighan's Algorithm (Count Set Bits)
Clears lowest set bit in each iteration; runs in $O(\text{number of set bits})$:
```cpp
int countSetBits(int n) {
    int count = 0;
    while (n) {
        n &= (n - 1);
        count++;
    }
    return count;
}
```

### 2. Single Number (XOR Property)
Since $a \oplus a = 0$ and $a \oplus 0 = a$, all duplicates cancel:
```cpp
int singleNumber(const vector<int>& nums) {
    int res = 0;
    for (int x : nums) res ^= x;
    return res;
}
```

### 3. Subset Generation using Bitmask ($O(2^N)$)
```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> res;
    for (int mask = 0; mask < (1 << n); ++mask) {
        vector<int> sub;
        for (int i = 0; i < n; ++i) {
            if ((mask >> i) & 1) sub.push_back(nums[i]);
        }
        res.push_back(sub);
    }
    return res;
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <bitset>

std::bitset<64> b(42);
int count = b.count(); // Number of set bits

// GCC/Clang built-ins
int popcount = __builtin_popcount(x);      // 32-bit set bit count
int popcountll = __builtin_popcountll(x);  // 64-bit set bit count
int leadingZeros = __builtin_clz(x);       // Count leading zeros
int trailingZeros = __builtin_ctz(x);      // Count trailing zeros
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Number of 1 Bits | 🟢 Easy | LC 191 | `n &= (n - 1)` / `__builtin_popcount` |
| Counting Bits | 🟢 Easy | LC 338 | DP Bit Shift `dp[i] = dp[i >> 1] + (i & 1)` |
| Single Number | 🟢 Easy | LC 136 | XOR Cancellation |
| Reverse Bits | 🟢 Easy | LC 190 | Bit-by-bit Shift |
| Missing Number | 🟢 Easy | LC 268 | XOR Index and Value |
| Sum of Two Integers | 🟡 Medium | LC 371 | Adder Simulation (`^` sum, `& << 1` carry) |
| Single Number II | 🟡 Medium | LC 137 | Modulo 3 Bit Counter |


---

## ⚠️ Common Pitfalls & C++ Tips

- Bitwise operators have **lower precedence than arithmetic operators and comparison operators**! Always write `(x & 1) == 0`, NOT `x & 1 == 0`.
- Use `1LL << shift` when shifting 64-bit quantities; `1 << 35` triggers undefined behavior on 32-bit integers.


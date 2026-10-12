# 📌 Mathematics & Number Theory

Mathematics and Number Theory form the mathematical foundation for competitive programming, cryptography, and algorithm analysis.

---

## 📖 Overview & Core Concepts

Essential mathematical competencies include:
1. **Greatest Common Divisor (GCD) & Euclidean Algorithm**:
   $$\gcd(a, b) = \gcd(b, a \pmod b)$$
2. **Sieve of Eratosthenes**: Finds all primes up to $N$ in $O(N \log \log N)$ time.
3. **Modular Arithmetic**:
   $$(a + b) \pmod M = ((a \pmod M) + (b \pmod M)) \pmod M$$
   $$(a \times b) \pmod M = ((a \pmod M) \times (b \pmod M)) \pmod M$$
   $$a / b \equiv a \times b^{M - 2} \pmod M \quad \text{(Fermat's Little Theorem when } M \text{ is prime)}$$
4. **Combinatorics**: $nCr = \frac{n!}{r!(n - r)!}$.
5. **Catalan Numbers**: $C_n = \frac{1}{n + 1} \binom{2n}{n}$ (counts valid parentheses pairs, binary search trees).


---

## ⚡ Time & Space Complexity

| Algorithm | Time Complexity | Auxiliary Space |
|---|---|---|
| Euclidean GCD | $O(\log(\min(a, b)))$ | $O(1)$ |
| Sieve of Eratosthenes | $O(N \log \log N)$ | $O(N)$ |
| Modular Exponentiation | $O(\log \text{power})$ | $O(1)$ |
| Prime Factorization (Trial Division) | $O(\sqrt{N})$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Sieve of Eratosthenes
```cpp
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int p = 2; p * p <= n; ++p) {
        if (isPrime[p]) {
            for (int i = p * p; i <= n; i += p)
                isPrime[i] = false;
        }
    }
    return isPrime;
}
```

### 2. Fast Modular Exponentiation ($(a^b) \pmod M$)
```cpp
long long modPow(long long base, long long exp, long long mod) {
    long long res = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) res = (res * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return res;
}
```

### 3. Modular Multiplicative Inverse
When $M$ is prime, by Fermat's Little Theorem:
$$b^{-1} \equiv b^{M - 2} \pmod M$$
```cpp
long long modInverse(long long b, long long mod) {
    return modPow(b, mod - 2, mod);
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <numeric>

int g = std::gcd(24, 36); // 12 (C++17)
int l = std::lcm(24, 36); // 72 (C++17)
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Count Primes | 🟡 Medium | LC 204 | Sieve of Eratosthenes |
| Pow(x, n) | 🟡 Medium | LC 50 | Binary Exponentiation |
| GCD of Strings | 🟢 Easy | LC 1071 | String Euclidean Algorithm |
| Factorial Trailing Zeroes | 🟡 Medium | LC 172 | Count Factors of 5 |
| Unique Paths | 🟡 Medium | LC 62 | Combinatorics $\binom{m+n-2}{m-1}$ |
| Happy Number | 🟢 Easy | LC 202 | Fast & Slow Pointers on Digits |


---

## ⚠️ Common Pitfalls & C++ Tips

- Watch out for overflow when computing `(a * b) % mod`. In C++, cast operands to `long long`: `(1LL * a * b) % mod`.
- In C++, `%` operator with negative numbers returns a negative result (`-5 % 3 = -2`). To get positive modulo: `(x % M + M) % M`.


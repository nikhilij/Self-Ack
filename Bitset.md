# Deep Dive into C++ STL `std::bitset`

`std::bitset` is a fixed-size sequence of bits, providing efficient bit-level operations. It is defined in the `<bitset>` header and is useful for compact storage and manipulation of binary data.

This guide covers usage, operations, and common competitive problems with heavily code examples and comments for explanation.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Header and Declaration](#1-header-and-declaration)
    - [Initialization](#2-initialization)
    - [Access and Modification](#3-access-and-modification)
    - [Size and Capacity](#4-size-and-capacity)
2.  [**Intermediate**](#intermediate)
    - [Bit Operations](#1-bit-operations)
    - [Comparison and Conversion](#2-comparison-and-conversion)
    - [Input/Output](#3-input-output)
3.  [**Advanced**](#advanced)
    - [Performance Characteristics](#1-performance-characteristics)
    - [Common Pitfalls](#2-common-pitfalls)
4.  [**Examples**](#examples)
    - [Set and Reset Bits](#1-set-and-reset-bits)
    - [Count Set Bits](#2-count-set-bits)
    - [Check Power of Two](#3-check-power-of-two)
    - [Bit Manipulation for Sum](#4-bit-manipulation-for-sum)
    - [Find Single Number](#5-find-single-number)
    - [Bitset for Subsets](#6-bitset-for-subsets)
    - [Prime Sieve with Bitset](#7-prime-sieve-with-bitset)
    - [Bitset for Graph](#8-bitset-for-graph)
    - [Hamming Distance](#9-hamming-distance)
    - [Bitset Operations](#10-bitset-operations)
5.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases)

---

## Basics

### 1. Header and Declaration

```cpp
#include <bitset>  // Include the bitset header
#include <iostream> // For output

int main() {
    // Declare a bitset of size 8
    std::bitset<8> bs;

    // Fixed size, specified at compile-time
    // Bits are indexed from 0 (LSB) to N-1 (MSB)

    return 0;
}
```

### 2. Initialization

```cpp
#include <bitset>
#include <iostream>
#include <string>

int main() {
    // Default: all bits 0
    std::bitset<8> bs1;

    // From unsigned long
    std::bitset<8> bs2(42);  // Binary of 42

    // From string
    std::bitset<8> bs3("10101010");  // From binary string

    // From string with offset
    std::bitset<8> bs4("1111", 4);  // "11110000"

    // Copy
    std::bitset<8> bs5 = bs2;

    std::cout << "bs2: " << bs2 << '\n';  // 00101010
    std::cout << "bs3: " << bs3 << '\n';  // 10101010

    return 0;
}
```

### 3. Access and Modification

```cpp
#include <bitset>
#include <iostream>

int main() {
    std::bitset<8> bs("10101010");

    // Access with operator[]
    bool bit = bs[0];  // LSB
    std::cout << "Bit 0: " << bit << '\n';  // 0

    // Set bit
    bs[1] = 1;  // Set bit 1

    // Test bit
    if (bs.test(2)) {  // Check bit 2
        std::cout << "Bit 2 is set\n";
    }

    // Set, reset, flip
    bs.set(3);   // Set bit 3
    bs.reset(4); // Reset bit 4
    bs.flip(5);  // Flip bit 5

    // Set/reset all
    bs.set();   // All 1s
    bs.reset(); // All 0s
    bs.flip();  // Flip all

    std::cout << "Final: " << bs << '\n';

    return 0;
}
```

### 4. Size and Capacity

```cpp
#include <bitset>
#include <iostream>

int main() {
    std::bitset<16> bs;

    // Size
    std::cout << "Size: " << bs.size() << '\n';  // 16

    // Count set bits
    bs.set(0);
    bs.set(5);
    std::cout << "Set bits: " << bs.count() << '\n';  // 2

    // Any, none, all
    std::cout << "Any set: " << bs.any() << '\n';  // true
    std::cout << "None set: " << bs.none() << '\n'; // false
    std::cout << "All set: " << bs.all() << '\n';   // false

    return 0;
}
```

## Intermediate

### 1. Bit Operations

```cpp
#include <bitset>
#include <iostream>

int main() {
    std::bitset<8> bs1("10101010");
    std::bitset<8> bs2("11001100");

    // Bitwise AND
    std::bitset<8> and_result = bs1 & bs2;
    std::cout << "AND: " << and_result << '\n';  // 10001000

    // Bitwise OR
    std::bitset<8> or_result = bs1 | bs2;
    std::cout << "OR: " << or_result << '\n';   // 11101110

    // Bitwise XOR
    std::bitset<8> xor_result = bs1 ^ bs2;
    std::cout << "XOR: " << xor_result << '\n'; // 01100110

    // NOT
    std::bitset<8> not_result = ~bs1;
    std::cout << "NOT: " << not_result << '\n';

    // Shift left
    std::bitset<8> left_shift = bs1 << 2;
    std::cout << "Left shift: " << left_shift << '\n';

    // Shift right
    std::bitset<8> right_shift = bs1 >> 2;
    std::cout << "Right shift: " << right_shift << '\n';

    return 0;
}
```

### 2. Comparison and Conversion

```cpp
#include <bitset>
#include <iostream>

int main() {
    std::bitset<8> bs1("10101010");
    std::bitset<8> bs2("10101010");
    std::bitset<8> bs3("11001100");

    // Equality
    if (bs1 == bs2) std::cout << "Equal\n";

    // Inequality
    if (bs1 != bs3) std::cout << "Not equal\n";

    // To string
    std::string str = bs1.to_string();
    std::cout << "String: " << str << '\n';

    // To unsigned long
    unsigned long ul = bs1.to_ulong();
    std::cout << "ULong: " << ul << '\n';

    // To unsigned long long (if fits)
    unsigned long long ull = bs1.to_ullong();
    std::cout << "ULLong: " << ull << '\n';

    return 0;
}
```

### 3. Input/Output

```cpp
#include <bitset>
#include <iostream>
#include <sstream>

int main() {
    std::bitset<8> bs;

    // Input from stream
    std::istringstream iss("10101010");
    iss >> bs;
    std::cout << "Input: " << bs << '\n';

    // Output
    std::cout << bs << '\n';  // Direct output

    // Using stringstream
    std::stringstream ss;
    ss << bs;
    std::string s = ss.str();
    std::cout << "Stringstream: " << s << '\n';

    return 0;
}
```

## Advanced

### 1. Performance Characteristics

```cpp
#include <bitset>
#include <iostream>
#include <chrono>

int main() {
    const int N = 1000000;
    std::bitset<N> bs;

    // Set bits
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < N; i += 2) {
        bs.set(i);
    }
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> time = end - start;
    std::cout << "Set time: " << time.count() << "s\n";

    // Count
    start = std::chrono::high_resolution_clock::now();
    size_t cnt = bs.count();
    end = std::chrono::high_resolution_clock::now();
    time = end - start;
    std::cout << "Count time: " << time.count() << "s, Count: " << cnt << '\n';

    // Bitset operations are very fast, O(1) or O(N) depending on operation

    return 0;
}
```

### 2. Common Pitfalls

```cpp
#include <bitset>
#include <iostream>

int main() {
    // Pitfall: Fixed size
    // std::bitset<8> bs;  // Size must be compile-time constant

    // Pitfall: Overflow in to_ulong()
    std::bitset<64> bs("1111111111111111111111111111111111111111111111111111111111111111");
    try {
        unsigned long ul = bs.to_ulong();  // May overflow
    } catch (std::overflow_error& e) {
        std::cout << "Overflow\n";
    }

    // Pitfall: Bit indexing
    // bs[0] is LSB, bs[size()-1] is MSB

    // Pitfall: String length mismatch
    // std::bitset<8> bs("101");  // Pads with zeros on left

    return 0;
}
```

## Examples

### 1. Set and Reset Bits

```cpp
#include <bitset>
#include <iostream>

int main() {
    std::bitset<8> bs;

    // Set specific bits
    bs.set(0);
    bs.set(3);
    bs.set(7);

    std::cout << "After set: " << bs << '\n';  // 10001001

    // Reset bits
    bs.reset(3);
    std::cout << "After reset: " << bs << '\n';  // 10000001

    return 0;
}
```

### 2. Count Set Bits

```cpp
#include <bitset>
#include <iostream>

// Count set bits in a number
int countBits(unsigned int n) {
    std::bitset<32> bs(n);
    return bs.count();
}

int main() {
    unsigned int n = 29;  // 11101
    std::cout << "Set bits in " << n << ": " << countBits(n) << '\n';  // 4

    return 0;
}
```

### 3. Check Power of Two

```cpp
#include <bitset>
#include <iostream>

// Check if power of two
bool isPowerOfTwo(int n) {
    if (n <= 0) return false;
    std::bitset<32> bs(n);
    return bs.count() == 1;
}

int main() {
    int n = 16;
    std::cout << n << " is power of 2: " << isPowerOfTwo(n) << '\n';  // 1

    return 0;
}
```

### 4. Bit Manipulation for Sum

```cpp
#include <bitset>
#include <iostream>

// Add two numbers using bits
int add(int a, int b) {
    while (b != 0) {
        int carry = a & b;
        a = a ^ b;
        b = carry << 1;
    }
    return a;
}

int main() {
    int a = 5, b = 3;
    std::cout << a << " + " << b << " = " << add(a, b) << '\n';  // 8

    return 0;
}
```

### 5. Find Single Number

```cpp
#include <bitset>
#include <vector>
#include <iostream>

// LeetCode 136: Single Number (XOR approach)
int singleNumber(const std::vector<int>& nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}

int main() {
    std::vector<int> nums = {2, 2, 1};
    std::cout << "Single: " << singleNumber(nums) << '\n';  // 1

    return 0;
}
```

### 6. Bitset for Subsets

```cpp
#include <bitset>
#include <iostream>
#include <vector>

// Generate all subsets using bitset
void generateSubsets(const std::vector<int>& set) {
    int n = set.size();
    for (int i = 0; i < (1 << n); ++i) {
        std::bitset<32> bs(i);
        std::cout << "{ ";
        for (int j = 0; j < n; ++j) {
            if (bs[j]) {
                std::cout << set[j] << " ";
            }
        }
        std::cout << "}\n";
    }
}

int main() {
    std::vector<int> set = {1, 2, 3};
    generateSubsets(set);

    return 0;
}
```

### 7. Prime Sieve with Bitset

```cpp
#include <bitset>
#include <iostream>

// Sieve of Eratosthenes with bitset
const int MAX = 100;
std::bitset<MAX + 1> is_prime;

void sieve() {
    is_prime.set();  // All true
    is_prime[0] = is_prime[1] = 0;
    for (int i = 2; i * i <= MAX; ++i) {
        if (is_prime[i]) {
            for (int j = i * i; j <= MAX; j += i) {
                is_prime[j] = 0;
            }
        }
    }
}

int main() {
    sieve();
    std::cout << "Primes up to " << MAX << ":\n";
    for (int i = 2; i <= MAX; ++i) {
        if (is_prime[i]) std::cout << i << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

### 8. Bitset for Graph

```cpp
#include <bitset>
#include <iostream>
#include <vector>

const int N = 5;
std::vector<std::bitset<N>> adj;

// Add edge
void addEdge(int u, int v) {
    adj[u][v] = 1;
    adj[v][u] = 1;  // Undirected
}

// Check edge
bool hasEdge(int u, int v) {
    return adj[u][v];
}

int main() {
    adj.resize(N);
    addEdge(0, 1);
    addEdge(1, 2);
    std::cout << "Edge 0-1: " << hasEdge(0, 1) << '\n';  // 1

    return 0;
}
```

### 9. Hamming Distance

```cpp
#include <bitset>
#include <iostream>

// LeetCode 461: Hamming Distance
int hammingDistance(int x, int y) {
    std::bitset<32> bs(x ^ y);
    return bs.count();
}

int main() {
    int x = 1, y = 4;  // 1: 01, 4: 100, XOR: 101
    std::cout << "Hamming: " << hammingDistance(x, y) << '\n';  // 2

    return 0;
}
```

### 10. Bitset Operations

```cpp
#include <bitset>
#include <iostream>

// Example: Bitset for permissions
enum Permission { READ = 0, WRITE = 1, EXECUTE = 2 };

void setPermission(std::bitset<3>& perms, Permission p) {
    perms.set(p);
}

bool hasPermission(const std::bitset<3>& perms, Permission p) {
    return perms.test(p);
}

int main() {
    std::bitset<3> perms;
    setPermission(perms, READ);
    setPermission(perms, WRITE);

    std::cout << "Has read: " << hasPermission(perms, READ) << '\n';    // 1
    std::cout << "Has execute: " << hasPermission(perms, EXECUTE) << '\n'; // 0

    return 0;
}
```

## Summary

### Best Practices
- Use `std::bitset` for fixed-size bit operations.
- Prefer for compact storage when size is known.
- Use for bit-level algorithms like sieves.
- Be careful with conversions to avoid overflow.

### Common Use Cases
- Bit manipulation problems.
- Prime sieves.
- Graph representations.
- Permission systems.
- Subset generation.
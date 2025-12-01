# Deep Dive into C++ `<bit>` Header

The `<bit>` header (C++20) provides utility functions for bit manipulation and operations on integers. It includes functions like `bit_ceil`, `bit_floor`, `popcount`, etc.

This guide covers usage and common competitive problems with heavily code examples and comments for explanation.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Header and Functions](#1-header-and-functions)
    - [Bit Counting](#2-bit-counting)
    - [Bit Manipulation](#3-bit-manipulation)
2.  [**Intermediate**](#intermediate)
    - [Power of Two Operations](#1-power-of-two-operations)
    - [Bit Scanning](#2-bit-scanning)
3.  [**Advanced**](#advanced)
    - [Performance Notes](#1-performance-notes)
    - [Common Pitfalls](#2-common-pitfalls)
4.  [**Examples**](#examples)
    - [Count Set Bits](#1-count-set-bits)
    - [Check Power of Two](#2-check-power-of-two)
    - [Next Power of Two](#3-next-power-of-two)
    - [Bit Length](#4-bit-length)
    - [Rotate Bits](#5-rotate-bits)
    - [Parity Check](#6-parity-check)
    - [Bit Extraction](#7-bit-extraction)
    - [Bit Reversal](#8-bit-reversal)
    - [Gray Code](#9-gray-code)
    - [Bitwise Operations](#10-bitwise-operations)
5.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases)

---

## Basics

### 1. Header and Functions

```cpp
#include <bit>      // Include the bit header (C++20)
#include <iostream> // For output

int main() {
    // std::bit functions are constexpr and work on unsigned integers
    unsigned int x = 42;  // 101010

    // Popcount: count set bits
    int count = std::popcount(x);
    std::cout << "Popcount: " << count << '\n';  // 3

    // Bit width: number of bits needed
    int width = std::bit_width(x);
    std::cout << "Bit width: " << width << '\n';  // 6

    return 0;
}
```

### 2. Bit Counting

```cpp
#include <bit>
#include <iostream>

int main() {
    unsigned int x = 29;  // 11101

    // popcount: number of 1s
    std::cout << "popcount(29): " << std::popcount(x) << '\n';  // 4

    // has_single_bit: check if power of 2
    std::cout << "has_single_bit(16): " << std::has_single_bit(16U) << '\n';  // 1

    // bit_ceil: smallest power of 2 >= x
    std::cout << "bit_ceil(5): " << std::bit_ceil(5U) << '\n';  // 8

    // bit_floor: largest power of 2 <= x
    std::cout << "bit_floor(5): " << std::bit_floor(5U) << '\n';  // 4

    return 0;
}
```

### 3. Bit Manipulation

```cpp
#include <bit>
#include <iostream>

int main() {
    unsigned int x = 0b10101010;

    // rotl: rotate left
    unsigned int rotated_left = std::rotl(x, 2);
    std::cout << "rotl: " << std::bitset<8>(rotated_left) << '\n';

    // rotr: rotate right
    unsigned int rotated_right = std::rotr(x, 2);
    std::cout << "rotr: " << std::bitset<8>(rotated_right) << '\n';

    // countl_zero: leading zeros
    std::cout << "countl_zero: " << std::countl_zero(x) << '\n';

    // countl_one: leading ones (for inverted)
    unsigned int y = ~x;
    std::cout << "countl_one: " << std::countl_one(y) << '\n';

    // countr_zero: trailing zeros
    std::cout << "countr_zero: " << std::countr_zero(x) << '\n';

    // countr_one: trailing ones
    unsigned int z = 0b111;
    std::cout << "countr_one: " << std::countr_one(z) << '\n';

    return 0;
}
```

## Intermediate

### 1. Power of Two Operations

```cpp
#include <bit>
#include <iostream>

int main() {
    // bit_ceil: next power of 2
    for (unsigned int i = 1; i <= 10; ++i) {
        std::cout << i << " -> " << std::bit_ceil(i) << '\n';
    }

    // bit_floor: previous power of 2
    for (unsigned int i = 1; i <= 10; ++i) {
        std::cout << i << " -> " << std::bit_floor(i) << '\n';
    }

    // has_single_bit
    std::cout << "16: " << std::has_single_bit(16U) << '\n';  // true
    std::cout << "18: " << std::has_single_bit(18U) << '\n';  // false

    return 0;
}
```

### 2. Bit Scanning

```cpp
#include <bit>
#include <iostream>

int main() {
    unsigned int x = 0b1001000;

    // countl_zero: leading zeros in full width
    std::cout << "Leading zeros: " << std::countl_zero(x) << '\n';

    // countr_zero: trailing zeros
    std::cout << "Trailing zeros: " << std::countr_zero(x) << '\n';

    // For bit scanning, can use these to find positions

    return 0;
}
```

## Advanced

### 1. Performance Notes

```cpp
#include <bit>
#include <iostream>
#include <chrono>

// These functions are highly optimized, often using hardware instructions

int main() {
    unsigned int x = 123456789;

    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < 1000000; ++i) {
        volatile int p = std::popcount(x);
    }
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> time = end - start;
    std::cout << "Popcount time: " << time.count() << "s\n";

    // Very fast, O(1) for most operations

    return 0;
}
```

### 2. Common Pitfalls

```cpp
#include <bit>
#include <iostream>

int main() {
    // Pitfall: Signed vs unsigned
    // Functions expect unsigned types
    int signed_x = -1;
    // std::popcount(signed_x);  // Undefined behavior

    // Use unsigned
    unsigned int x = 42U;

    // Pitfall: bit_ceil(0) undefined
    // std::bit_ceil(0U);  // Undefined

    // Pitfall: Overflow in large shifts
    // Be careful with large rotations

    return 0;
}
```

## Examples

### 1. Count Set Bits

```cpp
#include <bit>
#include <iostream>

// Count set bits
int countSetBits(unsigned int n) {
    return std::popcount(n);
}

int main() {
    unsigned int n = 29;  // 11101
    std::cout << "Set bits: " << countSetBits(n) << '\n';  // 4

    return 0;
}
```

### 2. Check Power of Two

```cpp
#include <bit>
#include <iostream>

// Check if power of two
bool isPowerOfTwo(unsigned int n) {
    return n > 0 && std::has_single_bit(n);
}

int main() {
    unsigned int n = 16;
    std::cout << n << " is power of 2: " << isPowerOfTwo(n) << '\n';  // 1

    return 0;
}
```

### 3. Next Power of Two

```cpp
#include <bit>
#include <iostream>

// Next power of two
unsigned int nextPowerOfTwo(unsigned int n) {
    if (n == 0) return 1;
    return std::bit_ceil(n);
}

int main() {
    unsigned int n = 5;
    std::cout << "Next power of 2 for " << n << ": " << nextPowerOfTwo(n) << '\n';  // 8

    return 0;
}
```

### 4. Bit Length

```cpp
#include <bit>
#include <iostream>

// Bit length
int bitLength(unsigned int n) {
    return std::bit_width(n);
}

int main() {
    unsigned int n = 42;  // 101010
    std::cout << "Bit length: " << bitLength(n) << '\n';  // 6

    return 0;
}
```

### 5. Rotate Bits

```cpp
#include <bit>
#include <iostream>

// Rotate left
unsigned int rotateLeft(unsigned int n, int shift) {
    return std::rotl(n, shift);
}

// Rotate right
unsigned int rotateRight(unsigned int n, int shift) {
    return std::rotr(n, shift);
}

int main() {
    unsigned int n = 0b10101010;
    std::cout << "Original: " << std::bitset<8>(n) << '\n';
    std::cout << "Rotate left 2: " << std::bitset<8>(rotateLeft(n, 2)) << '\n';
    std::cout << "Rotate right 2: " << std::bitset<8>(rotateRight(n, 2)) << '\n';

    return 0;
}
```

### 6. Parity Check

```cpp
#include <bit>
#include <iostream>

// Even parity
bool evenParity(unsigned int n) {
    return std::popcount(n) % 2 == 0;
}

int main() {
    unsigned int n = 7;  // 111
    std::cout << "Even parity: " << evenParity(n) << '\n';  // 0 (odd number of 1s)

    return 0;
}
```

### 7. Bit Extraction

```cpp
#include <bit>
#include <iostream>

// Extract bits from position
unsigned int extractBits(unsigned int n, int start, int length) {
    return (n >> start) & ((1U << length) - 1);
}

int main() {
    unsigned int n = 0b11010110;
    unsigned int extracted = extractBits(n, 2, 3);  // Bits 2,3,4
    std::cout << "Extracted: " << std::bitset<3>(extracted) << '\n';  // 101

    return 0;
}
```

### 8. Bit Reversal

```cpp
#include <bit>
#include <iostream>

// Reverse bits (for 32-bit)
unsigned int reverseBits(unsigned int n) {
    unsigned int result = 0;
    for (int i = 0; i < 32; ++i) {
        result |= ((n >> i) & 1) << (31 - i);
    }
    return result;
}

int main() {
    unsigned int n = 0b1011;
    std::cout << "Reversed: " << std::bitset<4>(reverseBits(n)) << '\n';

    return 0;
}
```

### 9. Gray Code

```cpp
#include <bit>
#include <iostream>

// Binary to Gray code
unsigned int binaryToGray(unsigned int n) {
    return n ^ (n >> 1);
}

// Gray to binary
unsigned int grayToBinary(unsigned int n) {
    unsigned int result = n;
    while (n >>= 1) {
        result ^= n;
    }
    return result;
}

int main() {
    unsigned int n = 3;  // 11
    unsigned int gray = binaryToGray(n);
    std::cout << "Gray: " << std::bitset<4>(gray) << '\n';
    unsigned int back = grayToBinary(gray);
    std::cout << "Back: " << back << '\n';

    return 0;
}
```

### 10. Bitwise Operations

```cpp
#include <bit>
#include <iostream>

// Example: Find position of highest set bit
int highestSetBit(unsigned int n) {
    if (n == 0) return -1;
    return 31 - std::countl_zero(n);
}

// Find position of lowest set bit
int lowestSetBit(unsigned int n) {
    if (n == 0) return -1;
    return std::countr_zero(n);
}

int main() {
    unsigned int n = 0b1001000;
    std::cout << "Highest set bit pos: " << highestSetBit(n) << '\n';  // 6
    std::cout << "Lowest set bit pos: " << lowestSetBit(n) << '\n';   // 3

    return 0;
}
```

## Summary

### Best Practices
- Use `<bit>` functions for efficient bit operations.
- Prefer unsigned types to avoid undefined behavior.
- Use for competitive programming bit problems.
- Combine with bitset for complex operations.

### Common Use Cases
- Bit counting and manipulation.
- Power of two checks.
- Bit scanning and extraction.
- Competitive coding problems.
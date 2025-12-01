# Deep Dive into C++ STL `std::array`

`std::array` is a fixed-size array container in C++ STL, providing a thin wrapper around C-style arrays with additional features like size tracking and iterators. It is defined in the `<array>` header and is useful for fixed-size collections in competitive programming and DSA.

This guide covers usage, operations, algorithms, and common competitive problems with heavily code examples and comments for explanation.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Header and Declaration](#1-header-and-declaration)
    - [Initialization and Assignment](#2-initialization-and-assignment)
    - [Access and Modification](#3-access-and-modification)
    - [Size and Capacity](#4-size-and-capacity)
2.  [**Intermediate**](#intermediate)
    - [Iterators and Traversal](#1-iterators-and-traversal)
    - [Comparison and Swap](#2-comparison-and-swap)
    - [Filling and Operations](#3-filling-and-operations)
3.  [**Advanced**](#advanced)
    - [Algorithms with Arrays](#1-algorithms-with-arrays)
    - [Performance Considerations](#2-performance-considerations)
    - [Common Pitfalls](#3-common-pitfalls)
4.  [**Examples**](#examples)
    - [Linear Search](#1-linear-search)
    - [Binary Search](#2-binary-search)
    - [Sort Array](#3-sort-array)
    - [Find Maximum/Minimum](#4-find-maximum-minimum)
    - [Two Sum](#5-two-sum)
    - [Rotate Array](#6-rotate-array)
    - [Remove Duplicates](#7-remove-duplicates)
    - [Merge Sorted Arrays](#8-merge-sorted-arrays)
    - [Kadane's Algorithm (Max Subarray Sum)](#9-kadanes-algorithm-max-subarray-sum)
    - [Dutch National Flag (Sort 0s, 1s, 2s)](#10-dutch-national-flag-sort-0s-1s-2s)
5.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases)

---

## Basics

### 1. Header and Declaration

```cpp
#include <array>    // Include the array header
#include <iostream> // For output

int main() {
    // Declare a fixed-size array of 5 integers
    std::array<int, 5> arr;

    // std::array is a class template: std::array<T, N>
    // Size N is part of the type, fixed at compile-time
    // No dynamic memory allocation

    return 0;
}
```

### 2. Initialization and Assignment

```cpp
#include <array>
#include <iostream>

int main() {
    // Default initialization (uninitialized for built-ins)
    std::array<int, 5> arr1;

    // Aggregate initialization
    std::array<int, 5> arr2 = {1, 2, 3, 4, 5};

    // Partial initialization (remaining elements zero-initialized)
    std::array<int, 5> arr3 = {1, 2};  // {1, 2, 0, 0, 0}

    // Copy initialization
    std::array<int, 5> arr4 = arr2;

    // Assignment
    arr1 = arr2;  // Copy assignment

    // Fill with value
    arr1.fill(10);  // All elements become 10

    // Output
    for (int x : arr2) std::cout << x << ' ';  // 1 2 3 4 5
    std::cout << '\n';

    return 0;
}
```

### 3. Access and Modification

```cpp
#include <array>
#include <iostream>

int main() {
    std::array<int, 5> arr = {10, 20, 30, 40, 50};

    // Access with operator[]
    int val = arr[0];  // 10
    arr[0] = 100;      // Modify

    // Access with at() - bounds checked
    try {
        val = arr.at(1);  // 20
        arr.at(1) = 200;  // Modify
    } catch (std::out_of_range& e) {
        std::cout << "Index out of range\n";
    }

    // Front and back
    int first = arr.front();  // 100
    int last = arr.back();    // 50

    // Data pointer (for C-style access)
    int* ptr = arr.data();
    ptr[2] = 300;  // Modify via pointer

    // Output modified array
    for (int x : arr) std::cout << x << ' ';  // 100 200 30 300 50
    std::cout << '\n';

    return 0;
}
```

### 4. Size and Capacity

```cpp
#include <array>
#include <iostream>

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    // Size (always N)
    std::cout << "Size: " << arr.size() << '\n';  // 5

    // Max size (same as size for array)
    std::cout << "Max size: " << arr.max_size() << '\n';  // 5

    // Empty check
    std::cout << "Empty: " << arr.empty() << '\n';  // false

    // Note: std::array has no capacity() or reserve() like vector
    // Size is fixed

    return 0;
}
```

## Intermediate

### 1. Iterators and Traversal

```cpp
#include <array>
#include <iostream>

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    // Range-based for loop
    std::cout << "Forward: ";
    for (int x : arr) std::cout << x << ' ';
    std::cout << '\n';

    // Using iterators
    std::cout << "Using iterators: ";
    for (auto it = arr.begin(); it != arr.end(); ++it) {
        std::cout << *it << ' ';
    }
    std::cout << '\n';

    // Reverse iterators
    std::cout << "Reverse: ";
    for (auto it = arr.rbegin(); it != arr.rend(); ++it) {
        std::cout << *it << ' ';
    }
    std::cout << '\n';

    // Const iterators
    const auto& carr = arr;
    for (auto it = carr.cbegin(); it != carr.cend(); ++it) {
        std::cout << *it << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

### 2. Comparison and Swap

```cpp
#include <array>
#include <iostream>

int main() {
    std::array<int, 3> arr1 = {1, 2, 3};
    std::array<int, 3> arr2 = {1, 2, 3};
    std::array<int, 3> arr3 = {3, 2, 1};

    // Equality comparison
    if (arr1 == arr2) std::cout << "arr1 == arr2\n";  // true

    // Inequality
    if (arr1 != arr3) std::cout << "arr1 != arr3\n";  // true

    // Lexicographical comparison
    if (arr1 < arr3) std::cout << "arr1 < arr3\n";    // true (1 < 3)

    // Swap
    arr1.swap(arr3);  // arr1 = {3,2,1}, arr3 = {1,2,3}

    for (int x : arr1) std::cout << x << ' ';
    std::cout << '\n';

    return 0;
}
```

### 3. Filling and Operations

```cpp
#include <array>
#include <iostream>
#include <algorithm>  // For std::fill, std::sort, etc.

int main() {
    std::array<int, 5> arr;

    // Fill with value
    arr.fill(42);  // All elements 42

    // Manual fill using algorithm
    std::fill(arr.begin(), arr.end(), 10);  // All 10

    // Partial fill
    std::fill_n(arr.begin() + 2, 2, 99);  // Indices 2,3 become 99

    // Copy from another array
    std::array<int, 5> arr2 = {1, 2, 3, 4, 5};
    arr = arr2;  // Copy assignment

    // Sort
    std::sort(arr.begin(), arr.end());  // Already sorted

    // Reverse
    std::reverse(arr.begin(), arr.end());  // {5,4,3,2,1}

    for (int x : arr) std::cout << x << ' ';
    std::cout << '\n';

    return 0;
}
```

## Advanced

### 1. Algorithms with Arrays

```cpp
#include <array>
#include <iostream>
#include <algorithm>
#include <numeric>  // For accumulate

int main() {
    std::array<int, 6> arr = {1, 2, 3, 4, 5, 6};

    // Find element
    auto it = std::find(arr.begin(), arr.end(), 3);
    if (it != arr.end()) std::cout << "Found 3 at index: " << it - arr.begin() << '\n';

    // Count occurrences
    int count = std::count(arr.begin(), arr.end(), 2);
    std::cout << "Count of 2: " << count << '\n';

    // Sum using accumulate
    int sum = std::accumulate(arr.begin(), arr.end(), 0);
    std::cout << "Sum: " << sum << '\n';

    // Min/Max element
    auto [min_it, max_it] = std::minmax_element(arr.begin(), arr.end());
    std::cout << "Min: " << *min_it << ", Max: " << *max_it << '\n';

    // Binary search (requires sorted array)
    std::sort(arr.begin(), arr.end());
    bool found = std::binary_search(arr.begin(), arr.end(), 4);
    std::cout << "4 found: " << found << '\n';

    return 0;
}
```

### 2. Performance Considerations

```cpp
#include <array>
#include <vector>
#include <iostream>
#include <chrono>

// std::array has zero overhead compared to C-arrays
// Fixed size allows optimizations

int main() {
    const int N = 100000;
    std::array<int, N> arr;
    std::vector<int> vec(N);

    // Fill array
    for (int i = 0; i < N; ++i) {
        arr[i] = i;
        vec[i] = i;
    }

    // Measure access time
    auto start = std::chrono::high_resolution_clock::now();
    volatile int sum = 0;
    for (int x : arr) sum += x;
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> arr_time = end - start;

    start = std::chrono::high_resolution_clock::now();
    sum = 0;
    for (int x : vec) sum += x;
    end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> vec_time = end - start;

    std::cout << "Array time: " << arr_time.count() << "s\n";
    std::cout << "Vector time: " << vec_time.count() << "s\n";

    // std::array often faster due to no dynamic allocation

    return 0;
}
```

### 3. Common Pitfalls

```cpp
#include <array>
#include <iostream>

int main() {
    // Pitfall 1: Fixed size - cannot resize
    std::array<int, 5> arr;
    // arr.resize(10);  // Error! No resize

    // Pitfall 2: Uninitialized elements
    std::array<int, 5> arr2;  // Elements uninitialized for built-ins
    // std::cout << arr2[0];  // Undefined behavior

    // Pitfall 3: Out of bounds access
    // arr[10] = 5;  // Undefined behavior, no check

    // Pitfall 4: Cannot assign arrays of different sizes
    // std::array<int, 5> a5;
    // std::array<int, 10> a10;
    // a5 = a10;  // Error! Different types

    // Pitfall 5: Forgetting const for read-only
    const std::array<int, 3> carr = {1, 2, 3};
    // carr[0] = 5;  // Error

    return 0;
}
```

## Examples

### 1. Linear Search

```cpp
#include <array>
#include <iostream>

// LeetCode style: Search in array
int linearSearch(const std::array<int, 10>& arr, int target) {
    for (size_t i = 0; i < arr.size(); ++i) {
        if (arr[i] == target) {
            return i;  // Return index
        }
    }
    return -1;  // Not found
}

int main() {
    std::array<int, 10> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int target = 5;
    int index = linearSearch(arr, target);
    std::cout << "Index of " << target << ": " << index << '\n';  // 4

    return 0;
}
```

### 2. Binary Search

```cpp
#include <array>
#include <iostream>
#include <algorithm>

// LeetCode 704: Binary Search
int binarySearch(std::array<int, 10> arr, int target) {
    std::sort(arr.begin(), arr.end());  // Ensure sorted
    int left = 0, right = arr.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}

int main() {
    std::array<int, 10> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int target = 7;
    int index = binarySearch(arr, target);
    std::cout << "Index of " << target << ": " << index << '\n';  // 6

    return 0;
}
```

### 3. Sort Array

```cpp
#include <array>
#include <iostream>
#include <algorithm>

// LeetCode 912: Sort an Array (simplified)
void sortArray(std::array<int, 10>& arr) {
    std::sort(arr.begin(), arr.end());
}

int main() {
    std::array<int, 10> arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
    sortArray(arr);
    for (int x : arr) std::cout << x << ' ';  // 1 1 2 3 3 4 5 5 6 9
    std::cout << '\n';

    return 0;
}
```

### 4. Find Maximum/Minimum

```cpp
#include <array>
#include <iostream>
#include <algorithm>

// Find max and min in array
std::pair<int, int> findMinMax(const std::array<int, 10>& arr) {
    auto [min_it, max_it] = std::minmax_element(arr.begin(), arr.end());
    return {*min_it, *max_it};
}

int main() {
    std::array<int, 10> arr = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
    auto [min_val, max_val] = findMinMax(arr);
    std::cout << "Min: " << min_val << ", Max: " << max_val << '\n';  // 1, 9

    return 0;
}
```

### 5. Two Sum

```cpp
#include <array>
#include <iostream>
#include <unordered_map>

// LeetCode 1: Two Sum (adapted for fixed array)
std::array<int, 2> twoSum(const std::array<int, 10>& arr, int target) {
    std::unordered_map<int, int> map;
    for (int i = 0; i < arr.size(); ++i) {
        int complement = target - arr[i];
        if (map.count(complement)) {
            return {map[complement], i};
        }
        map[arr[i]] = i;
    }
    return {-1, -1};  // No solution
}

int main() {
    std::array<int, 10> arr = {2, 7, 11, 15, 3, 6, 9, 1, 8, 4};
    int target = 9;
    auto result = twoSum(arr, target);
    std::cout << "Indices: " << result[0] << ", " << result[1] << '\n';  // e.g., 0, 1

    return 0;
}
```

### 6. Rotate Array

```cpp
#include <array>
#include <iostream>
#include <algorithm>

// LeetCode 189: Rotate Array
void rotate(std::array<int, 10>& arr, int k) {
    int n = arr.size();
    k %= n;
    std::reverse(arr.begin(), arr.end());
    std::reverse(arr.begin(), arr.begin() + k);
    std::reverse(arr.begin() + k, arr.end());
}

int main() {
    std::array<int, 10> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int k = 3;
    rotate(arr, k);
    for (int x : arr) std::cout << x << ' ';  // 8 9 10 1 2 3 4 5 6 7
    std::cout << '\n';

    return 0;
}
```

### 7. Remove Duplicates

```cpp
#include <array>
#include <iostream>

// LeetCode 26: Remove Duplicates from Sorted Array
int removeDuplicates(std::array<int, 10>& arr) {
    if (arr.empty()) return 0;
    int j = 0;
    for (int i = 1; i < arr.size(); ++i) {
        if (arr[i] != arr[j]) {
            ++j;
            arr[j] = arr[i];
        }
    }
    return j + 1;
}

int main() {
    std::array<int, 10> arr = {1, 1, 2, 2, 3, 3, 4, 5, 5, 6};
    int newLength = removeDuplicates(arr);
    std::cout << "New length: " << newLength << '\n';  // 6
    for (int i = 0; i < newLength; ++i) std::cout << arr[i] << ' ';
    std::cout << '\n';

    return 0;
}
```

### 8. Merge Sorted Arrays

```cpp
#include <array>
#include <iostream>

// Merge two sorted arrays into one
void merge(std::array<int, 10>& arr1, int m, const std::array<int, 5>& arr2, int n) {
    int i = m - 1, j = n - 1, k = m + n - 1;
    while (i >= 0 && j >= 0) {
        if (arr1[i] > arr2[j]) {
            arr1[k--] = arr1[i--];
        } else {
            arr1[k--] = arr2[j--];
        }
    }
    while (j >= 0) {
        arr1[k--] = arr2[j--];
    }
}

int main() {
    std::array<int, 10> arr1 = {1, 3, 5, 7, 9, 0, 0, 0, 0, 0};
    std::array<int, 5> arr2 = {2, 4, 6, 8, 10};
    merge(arr1, 5, arr2, 5);
    for (int x : arr1) std::cout << x << ' ';  // 1 2 3 4 5 6 7 8 9 10
    std::cout << '\n';

    return 0;
}
```

### 9. Kadane's Algorithm (Max Subarray Sum)

```cpp
#include <array>
#include <iostream>
#include <algorithm>

// LeetCode 53: Maximum Subarray
int maxSubArray(const std::array<int, 10>& arr) {
    int max_so_far = arr[0], max_ending_here = arr[0];
    for (size_t i = 1; i < arr.size(); ++i) {
        max_ending_here = std::max(arr[i], max_ending_here + arr[i]);
        max_so_far = std::max(max_so_far, max_ending_here);
    }
    return max_so_far;
}

int main() {
    std::array<int, 10> arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4, 0};
    int max_sum = maxSubArray(arr);
    std::cout << "Max subarray sum: " << max_sum << '\n';  // 6

    return 0;
}
```

### 10. Dutch National Flag (Sort 0s, 1s, 2s)

```cpp
#include <array>
#include <iostream>

// LeetCode 75: Sort Colors
void sortColors(std::array<int, 10>& arr) {
    int low = 0, mid = 0, high = arr.size() - 1;
    while (mid <= high) {
        if (arr[mid] == 0) {
            std::swap(arr[low], arr[mid]);
            ++low;
            ++mid;
        } else if (arr[mid] == 1) {
            ++mid;
        } else {
            std::swap(arr[mid], arr[high]);
            --high;
        }
    }
}

int main() {
    std::array<int, 10> arr = {2, 0, 2, 1, 1, 0, 2, 1, 0, 1};
    sortColors(arr);
    for (int x : arr) std::cout << x << ' ';  // 0 0 0 1 1 1 1 2 2 2
    std::cout << '\n';

    return 0;
}
```

## Summary

### Best Practices
- Use `std::array` for fixed-size arrays to avoid dynamic allocation overhead.
- Prefer `at()` for bounds checking in development.
- Use range-based loops for traversal.
- Combine with `<algorithm>` for powerful operations.

### Common Use Cases
- Fixed-size buffers in embedded systems.
- Arrays in competitive programming problems.
- When size is known at compile-time.
- As building blocks for more complex data structures.
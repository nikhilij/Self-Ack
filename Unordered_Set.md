# Deep Dive into C++ STL `std::unordered_set`

`std::unordered_set` is an associative container that stores unique elements in no particular order, implemented as a hash table. It provides average-case constant-time complexity for insertion, deletion, and lookup operations (O(1)). Defined in the `<unordered_set>` header.

This guide covers usage, operations, customization, and common competitive problems with heavily code examples and comments for explanation.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Header and Declaration](#1-header-and-declaration)
    - [Insertion and Deletion](#2-insertion-and-deletion)
    - [Search and Access](#3-search-and-access)
    - [Size and Capacity](#4-size-and-capacity)
2.  [**Intermediate**](#intermediate)
    - [Iterators and Traversal](#1-iterators-and-traversal)
    - [Buckets and Rehashing](#2-buckets-and-rehashing)
    - [Custom Hash and Equality](#3-custom-hash-and-equality)
3.  [**Advanced**](#advanced)
    - [Performance Characteristics](#1-performance-characteristics)
    - [Memory Usage and Tuning](#2-memory-usage-and-tuning)
    - [Concurrency Notes](#3-concurrency-notes)
    - [Common Pitfalls](#4-common-pitfalls)
4.  [**Examples**](#examples)
    - [Insert and Check Existence](#1-insert-and-check-existence)
    - [Remove Duplicates from Array](#2-remove-duplicates-from-array)
    - [Intersection of Two Arrays](#3-intersection-of-two-arrays)
    - [Union of Two Sets](#4-union-of-two-sets)
    - [Check for Duplicates](#5-check-for-duplicates)
    - [First Unique Character](#6-first-unique-character)
    - [Happy Number](#7-happy-number)
    - [Jewels and Stones](#8-jewels-and-stones)
    - [Unique Email Addresses](#9-unique-email-addresses)
    - [Longest Consecutive Sequence](#10-longest-consecutive-sequence)
5.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases)

---

## Basics

### 1. Header and Declaration

```cpp
#include <unordered_set>  // Include the unordered_set header
#include <iostream>       // For output

int main() {
    // Declare an unordered_set for integers
    std::unordered_set<int> uset;

    // Stores unique elements, no duplicates allowed
    // Order is unspecified, depends on hash

    return 0;
}
```

### 2. Insertion and Deletion

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset;

    // Insert elements
    uset.insert(10);  // Insert 10
    uset.insert(20);  // Insert 20
    uset.insert(10);  // Duplicate, ignored

    // Insert with hint (iterator position, for optimization)
    auto it = uset.insert(uset.begin(), 30);  // Insert 30

    // Insert range
    std::vector<int> vec = {40, 50, 40};
    uset.insert(vec.begin(), vec.end());  // Inserts 40, 50 (40 duplicate)

    // Erase by value
    uset.erase(20);  // Remove 20

    // Erase by iterator
    it = uset.find(30);
    if (it != uset.end()) {
        uset.erase(it);  // Remove 30
    }

    // Erase range
    // uset.erase(uset.begin(), uset.end());  // Clear all

    // Clear all
    uset.clear();

    return 0;
}
```

### 3. Search and Access

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset = {10, 20, 30, 40, 50};

    // Find element
    auto it = uset.find(30);
    if (it != uset.end()) {
        std::cout << "Found: " << *it << '\n';  // 30
    }

    // Count (0 or 1 for sets)
    int count = uset.count(20);
    std::cout << "Count of 20: " << count << '\n';  // 1

    // Check if contains
    if (uset.contains(40)) {  // C++20
        std::cout << "Contains 40\n";
    }

    // Alternative: use find
    if (uset.find(40) != uset.end()) {
        std::cout << "Contains 40\n";
    }

    return 0;
}
```

### 4. Size and Capacity

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset;

    // Size
    std::cout << "Initial size: " << uset.size() << '\n';  // 0

    uset.insert({1, 2, 3, 4, 5});
    std::cout << "Size after inserts: " << uset.size() << '\n';  // 5

    // Max size
    std::cout << "Max size: " << uset.max_size() << '\n';

    // Empty
    std::cout << "Empty: " << uset.empty() << '\n';  // false

    // Bucket info
    std::cout << "Bucket count: " << uset.bucket_count() << '\n';
    std::cout << "Load factor: " << uset.load_factor() << '\n';

    // Reserve
    uset.reserve(100);  // Preallocate for 100 elements
    std::cout << "Bucket count after reserve: " << uset.bucket_count() << '\n';

    return 0;
}
```

## Intermediate

### 1. Iterators and Traversal

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset = {1, 2, 3, 4, 5};

    // Range-based for loop
    std::cout << "Elements: ";
    for (int x : uset) {
        std::cout << x << ' ';
    }
    std::cout << '\n';

    // Using iterators
    std::cout << "Using iterators: ";
    for (auto it = uset.begin(); it != uset.end(); ++it) {
        std::cout << *it << ' ';
    }
    std::cout << '\n';

    // Const iterators
    const auto& cuset = uset;
    for (auto it = cuset.cbegin(); it != cuset.cend(); ++it) {
        std::cout << *it << ' ';
    }
    std::cout << '\n';

    // Note: Order is unspecified

    return 0;
}
```

### 2. Buckets and Rehashing

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset;
    for (int i = 0; i < 20; ++i) uset.insert(i);

    // Bucket count
    std::cout << "Buckets: " << uset.bucket_count() << '\n';

    // Bucket size
    for (size_t i = 0; i < uset.bucket_count(); ++i) {
        std::cout << "Bucket " << i << " size: " << uset.bucket_size(i) << '\n';
    }

    // Bucket for key
    std::cout << "Bucket for 5: " << uset.bucket(5) << '\n';

    // Rehash
    uset.rehash(50);  // Set bucket count to at least 50
    std::cout << "Buckets after rehash: " << uset.bucket_count() << '\n';

    // Reserve
    uset.reserve(100);
    std::cout << "Buckets after reserve: " << uset.bucket_count() << '\n';

    return 0;
}
```

### 3. Custom Hash and Equality

```cpp
#include <unordered_set>
#include <iostream>

// Custom key struct
struct Point {
    int x, y;
};

// Custom hash
struct PointHash {
    std::size_t operator()(const Point& p) const noexcept {
        return std::hash<int>()(p.x) ^ (std::hash<int>()(p.y) << 1);
    }
};

// Custom equality
struct PointEqual {
    bool operator()(const Point& a, const Point& b) const noexcept {
        return a.x == b.x && a.y == b.y;
    }
};

int main() {
    // Custom unordered_set
    std::unordered_set<Point, PointHash, PointEqual> point_set;

    point_set.insert({1, 2});
    point_set.insert({3, 4});
    point_set.insert({1, 2});  // Duplicate, ignored

    for (const auto& p : point_set) {
        std::cout << "(" << p.x << ", " << p.y << ") ";
    }
    std::cout << '\n';

    return 0;
}
```

## Advanced

### 1. Performance Characteristics

```cpp
#include <unordered_set>
#include <iostream>
#include <chrono>

int main() {
    std::unordered_set<int> uset;

    // Insert
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < 100000; ++i) {
        uset.insert(i);
    }
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> insert_time = end - start;
    std::cout << "Insert time: " << insert_time.count() << "s\n";

    // Lookup
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < 100000; ++i) {
        volatile bool found = uset.count(i);
    }
    end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> lookup_time = end - start;
    std::cout << "Lookup time: " << lookup_time.count() << "s\n";

    // Average O(1), worst O(n) for bad hash

    return 0;
}
```

### 2. Memory Usage and Tuning

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset;

    // Reserve to optimize
    uset.reserve(10000);
    std::cout << "Buckets after reserve: " << uset.bucket_count() << '\n';

    // Max load factor
    uset.max_load_factor(0.7f);
    std::cout << "Max load factor: " << uset.max_load_factor() << '\n';

    // Memory overhead due to buckets and nodes

    return 0;
}
```

### 3. Concurrency Notes

```cpp
#include <unordered_set>
#include <mutex>
#include <thread>
#include <iostream>

// Thread-safe wrapper
class ThreadSafeUnorderedSet {
    std::unordered_set<int> set_;
    std::mutex mutex_;

public:
    void insert(int val) {
        std::lock_guard<std::mutex> lock(mutex_);
        set_.insert(val);
    }

    bool contains(int val) {
        std::lock_guard<std::mutex> lock(mutex_);
        return set_.count(val);
    }
};

int main() {
    ThreadSafeUnorderedSet ts_set;

    std::thread t1([&]() { ts_set.insert(1); });
    std::thread t2([&]() { std::cout << ts_set.contains(1) << '\n'; });

    t1.join();
    t2.join();

    return 0;
}
```

### 4. Common Pitfalls

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> uset = {1, 2, 3};

    // Pitfall: Assuming order
    // Elements are not ordered

    // Pitfall: Modifying during iteration
    // for (auto it = uset.begin(); it != uset.end(); ++it) {
    //     if (*it == 2) uset.erase(it);  // Invalidates iterator
    // }

    // Safe way: collect to erase
    std::vector<int> to_erase;
    for (int x : uset) {
        if (x == 2) to_erase.push_back(x);
    }
    for (int x : to_erase) uset.erase(x);

    return 0;
}
```

## Examples

### 1. Insert and Check Existence

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<std::string> uset;

    // Insert
    uset.insert("apple");
    uset.insert("banana");

    // Check existence
    if (uset.find("apple") != uset.end()) {
        std::cout << "Apple exists\n";
    }

    // Count
    std::cout << "Count of banana: " << uset.count("banana") << '\n';

    return 0;
}
```

### 2. Remove Duplicates from Array

```cpp
#include <unordered_set>
#include <vector>
#include <iostream>

// LeetCode 26: Remove Duplicates (adapted)
int removeDuplicates(std::vector<int>& nums) {
    std::unordered_set<int> seen;
    std::vector<int> unique;
    for (int num : nums) {
        if (seen.insert(num).second) {  // Insert succeeds
            unique.push_back(num);
        }
    }
    nums = unique;
    return nums.size();
}

int main() {
    std::vector<int> nums = {1, 1, 2, 2, 3};
    int len = removeDuplicates(nums);
    std::cout << "Length: " << len << '\n';  // 3
    for (int x : nums) std::cout << x << ' ';
    std::cout << '\n';

    return 0;
}
```

### 3. Intersection of Two Arrays

```cpp
#include <unordered_set>
#include <vector>
#include <iostream>

// LeetCode 349: Intersection of Two Arrays
std::vector<int> intersection(const std::vector<int>& nums1, const std::vector<int>& nums2) {
    std::unordered_set<int> set1(nums1.begin(), nums1.end());
    std::unordered_set<int> result_set;
    for (int num : nums2) {
        if (set1.count(num)) {
            result_set.insert(num);
        }
    }
    return std::vector<int>(result_set.begin(), result_set.end());
}

int main() {
    std::vector<int> nums1 = {1, 2, 2, 1}, nums2 = {2, 2};
    auto res = intersection(nums1, nums2);
    for (int x : res) std::cout << x << ' ';  // 2
    std::cout << '\n';

    return 0;
}
```

### 4. Union of Two Sets

```cpp
#include <unordered_set>
#include <vector>
#include <iostream>

std::unordered_set<int> setUnion(const std::unordered_set<int>& s1, const std::unordered_set<int>& s2) {
    std::unordered_set<int> result = s1;
    result.insert(s2.begin(), s2.end());
    return result;
}

int main() {
    std::unordered_set<int> s1 = {1, 2, 3}, s2 = {3, 4, 5};
    auto res = setUnion(s1, s2);
    for (int x : res) std::cout << x << ' ';  // 1 2 3 4 5
    std::cout << '\n';

    return 0;
}
```

### 5. Check for Duplicates

```cpp
#include <unordered_set>
#include <vector>
#include <iostream>

// LeetCode 217: Contains Duplicate
bool containsDuplicate(const std::vector<int>& nums) {
    std::unordered_set<int> seen;
    for (int num : nums) {
        if (!seen.insert(num).second) {
            return true;  // Duplicate found
        }
    }
    return false;
}

int main() {
    std::vector<int> nums = {1, 2, 3, 1};
    std::cout << containsDuplicate(nums) << '\n';  // 1

    return 0;
}
```

### 6. First Unique Character

```cpp
#include <unordered_set>
#include <string>
#include <iostream>

// LeetCode 387: First Unique Character in a String
int firstUniqChar(const std::string& s) {
    std::unordered_set<char> seen;
    std::unordered_set<char> duplicates;
    for (char c : s) {
        if (seen.count(c)) {
            duplicates.insert(c);
        } else {
            seen.insert(c);
        }
    }
    for (int i = 0; i < s.size(); ++i) {
        if (duplicates.find(s[i]) == duplicates.end()) {
            return i;
        }
    }
    return -1;
}

int main() {
    std::string s = "leetcode";
    std::cout << firstUniqChar(s) << '\n';  // 0 ('l')

    return 0;
}
```

### 7. Happy Number

```cpp
#include <unordered_set>
#include <iostream>

// LeetCode 202: Happy Number
bool isHappy(int n) {
    std::unordered_set<int> seen;
    while (n != 1 && seen.find(n) == seen.end()) {
        seen.insert(n);
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        n = sum;
    }
    return n == 1;
}

int main() {
    int n = 19;
    std::cout << isHappy(n) << '\n';  // 1

    return 0;
}
```

### 8. Jewels and Stones

```cpp
#include <unordered_set>
#include <string>
#include <iostream>

// LeetCode 771: Jewels and Stones
int numJewelsInStones(const std::string& jewels, const std::string& stones) {
    std::unordered_set<char> jewel_set(jewels.begin(), jewels.end());
    int count = 0;
    for (char stone : stones) {
        if (jewel_set.count(stone)) {
            ++count;
        }
    }
    return count;
}

int main() {
    std::string jewels = "aA", stones = "aAAbbbb";
    std::cout << numJewelsInStones(jewels, stones) << '\n';  // 3

    return 0;
}
```

### 9. Unique Email Addresses

```cpp
#include <unordered_set>
#include <string>
#include <iostream>

// LeetCode 929: Unique Email Addresses
int numUniqueEmails(const std::vector<std::string>& emails) {
    std::unordered_set<std::string> unique;
    for (const std::string& email : emails) {
        size_t at_pos = email.find('@');
        std::string local = email.substr(0, at_pos);
        std::string domain = email.substr(at_pos);

        // Remove dots and everything after +
        size_t plus_pos = local.find('+');
        if (plus_pos != std::string::npos) {
            local = local.substr(0, plus_pos);
        }
        local.erase(std::remove(local.begin(), local.end(), '.'), local.end());

        unique.insert(local + domain);
    }
    return unique.size();
}

int main() {
    std::vector<std::string> emails = {"test.email+alex@leetcode.com", "test.e.mail+bob.cathy@leetcode.com", "testemail+david@lee.tcode.com"};
    std::cout << numUniqueEmails(emails) << '\n';  // 2

    return 0;
}
```

### 10. Longest Consecutive Sequence

```cpp
#include <unordered_set>
#include <vector>
#include <iostream>

// LeetCode 128: Longest Consecutive Sequence
int longestConsecutive(const std::vector<int>& nums) {
    std::unordered_set<int> num_set(nums.begin(), nums.end());
    int longest = 0;
    for (int num : nums) {
        if (num_set.find(num - 1) == num_set.end()) {  // Start of sequence
            int current = num;
            int streak = 1;
            while (num_set.find(current + 1) != num_set.end()) {
                current++;
                streak++;
            }
            longest = std::max(longest, streak);
        }
    }
    return longest;
}

int main() {
    std::vector<int> nums = {100, 4, 200, 1, 3, 2};
    std::cout << longestConsecutive(nums) << '\n';  // 4

    return 0;
}
```

## Summary

### Best Practices
- Use `reserve()` for expected size to avoid rehashing.
- Prefer `unordered_set` for fast lookups when order doesn't matter.
- Use custom hash/equality for complex keys.
- Avoid modifying during iteration.

### Common Use Cases
- Checking for duplicates.
- Fast membership testing.
- Set operations (union, intersection).
- Caching unique elements.
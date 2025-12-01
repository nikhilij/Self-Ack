# Deep Dive into C++ STL `std::unordered_map`

`std::unordered_map` is an associative container that provides average-case constant-time complexity for lookup, insertion, and removal of elements (O(1)), implemented as a hash table. It stores elements formed by the combination of a key value and a mapped value, and the keys are unique. The container is defined in the `<unordered_map>` header.

This guide mirrors the style of `Vector.md` and covers usage, performance, customization (hash and equality), and common pitfalls.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Header and Declaration](#1-header-and-declaration)
    - [Insert and Access](#2-insert-and-access)
    - [Erase and Clear](#3-erase-and-clear)
    - [Size and Load Factor](#4-size-and-load-factor)
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
    - [Frequency Counting](#1-frequency-counting)
    - [Custom Key Type & Hash](#2-custom-key-type--hash)
5.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases)

---

## Basics

### 1. Header and Declaration

```cpp
#include <unordered_map>  // Include the unordered_map header for hash table functionality
#include <string>         // For string keys
#include <iostream>       // For output

int main() {
    // Declare an unordered_map with string keys and int values
    std::unordered_map<std::string, int> freq;

    // By default, uses std::hash<std::string> for hashing keys
    // and std::equal_to<std::string> for comparing keys for equality
    // Keys must be unique; attempting to insert a duplicate key does nothing

    return 0;
}
```

### 2. Insert and Access

```cpp
#include <unordered_map>
#include <string>
#include <iostream>

int main() {
    std::unordered_map<std::string, int> m;

    // operator[]: Inserts a default-initialized mapped value if the key is missing, then returns a reference
    m["apple"] = 2;  // Inserts "apple" -> 2
    m["banana"] += 1; // "banana" not present, default to 0, then +=1, so 1

    // insert(): Inserts a pair<key, value>, returns pair<iterator, bool>
    auto res = m.insert({"cherry", 5});  // Inserts "cherry" -> 5, res.second = true
    if (!res.second) std::cout << "cherry existed\n";  // If key existed, second is false

    // at(): Access with bounds checking, throws std::out_of_range if key not found
    try {
        std::cout << m.at("apple") << "\n";  // Outputs 2
    } catch (std::out_of_range& e) {
        std::cout << "Key not found\n";
    }

    // try_emplace() (C++17): Constructs mapped value in-place only if key does not exist
    m.try_emplace("banana", 10);  // "banana" exists, does not overwrite, stays 1
    m.try_emplace("date", 7);     // "date" not exist, inserts "date" -> 7

    // find(): Returns iterator to element or end() if not found
    auto it = m.find("apple");
    if (it != m.end()) {
        std::cout << "Found apple: " << it->second << '\n';  // Outputs 2
    }

    // count(): Returns 1 if key exists, 0 otherwise
    if (m.count("cherry")) {
        std::cout << "Cherry is present\n";
    }

    return 0;
}
```

### 3. Erase and Clear

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    std::unordered_map<int, int> m = {{1, 10}, {2, 20}, {3, 30}};

    // erase(key): Erases element with the given key, returns number of removed elements (0 or 1)
    size_t removed = m.erase(1);  // Removes key 1, removed = 1
    std::cout << "Removed: " << removed << '\n';  // Outputs 1

    // erase(iterator): Erase by iterator, returns next iterator
    auto it = m.find(2);
    if (it != m.end()) {
        it = m.erase(it);  // Erase key 2, it now points to next element (3)
    }

    // erase(first, last): Erase range
    // For example, erase all elements with value < 25
    for (auto it = m.begin(); it != m.end(); ) {
        if (it->second < 25) {
            it = m.erase(it);  // erase returns next iterator
        } else {
            ++it;
        }
    }

    // clear(): Removes all elements
    m.clear();  // Now m is empty
    std::cout << "Size after clear: " << m.size() << '\n';  // 0

    return 0;
}
```

### 4. Size and Load Factor

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    std::unordered_map<int, int> m;

    // size(): Number of elements stored
    std::cout << "Initial size: " << m.size() << '\n';  // 0

    // Insert some elements
    for (int i = 0; i < 10; ++i) {
        m[i] = i * 10;
    }
    std::cout << "Size after inserts: " << m.size() << '\n';  // 10

    // bucket_count(): Number of buckets in the table
    std::cout << "Buckets: " << m.bucket_count() << '\n';

    // max_load_factor(): Maximum average number of elements per bucket before rehashing
    std::cout << "Max load factor: " << m.max_load_factor() << '\n';  // Default ~1.0

    // load_factor(): size() / bucket_count()
    std::cout << "Current load factor: " << m.load_factor() << '\n';

    // Set max_load_factor to 0.7 for more buckets, less collisions
    m.max_load_factor(0.7f);
    std::cout << "New max load factor: " << m.max_load_factor() << '\n';

    // reserve(n): Preallocates buckets to handle at least n elements without rehash
    m.reserve(1000);  // Optimize for ~1000 elements
    std::cout << "Buckets after reserve: " << m.bucket_count() << '\n';
    std::cout << "Load factor after reserve: " << m.load_factor() << '\n';

    return 0;
}
```

## Intermediate

### 1. Iterators and Traversal

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    std::unordered_map<std::string, int> m = {{"apple", 1}, {"banana", 2}, {"cherry", 3}};

    // Traversal order is unspecified (depends on hashing and bucket layout)
    // Do not rely on any ordering
    std::cout << "Traversal:\n";
    for (const auto& p : m) {
        std::cout << p.first << " => " << p.second << '\n';
    }

    // Using iterators
    std::cout << "Using iterators:\n";
    for (auto it = m.begin(); it != m.end(); ++it) {
        std::cout << it->first << " => " << it->second << '\n';
    }

    // Reverse iterators (cbegin, cend for const)
    std::cout << "Reverse traversal:\n";
    for (auto it = m.rbegin(); it != m.rend(); ++it) {
        std::cout << it->first << " => " << it->second << '\n';
    }

    // Note: Modifying the container (insertion/erasure) may invalidate iterators
    // only for erased elements; insertions do not invalidate unless rehash occurs
    auto it = m.begin();
    m.insert({"date", 4});  // May cause rehash, invalidating it
    // it may now be invalid!

    return 0;
}
```

### 2. Buckets and Rehashing

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    std::unordered_map<int, int> m;
    for (int i = 0; i < 20; ++i) m[i] = i;

    // bucket_count(): Current number of buckets
    std::cout << "Buckets: " << m.bucket_count() << '\n';

    // bucket_size(n): Number of elements in bucket n
    for (size_t i = 0; i < m.bucket_count(); ++i) {
        std::cout << "Bucket " << i << " has " << m.bucket_size(i) << " elements\n";
    }

    // bucket(key): Bucket index for a specific key
    std::cout << "Key 5 is in bucket: " << m.bucket(5) << '\n';

    // rehash(n): Sets number of buckets to at least n, forces rehashing
    // Iterators and references may be invalidated
    m.rehash(50);  // Set at least 50 buckets
    std::cout << "Buckets after rehash: " << m.bucket_count() << '\n';

    // reserve(n): Ensure space for n elements, preferred over manual rehash
    m.reserve(100);  // Prepare for 100 elements
    std::cout << "Buckets after reserve: " << m.bucket_count() << '\n';

    // Rehashing causes iterator invalidation
    // All iterators, pointers, references invalidated during rehash

    return 0;
}
```

### 3. Custom Hash and Equality

```cpp
#include <unordered_map>
#include <iostream>

// Custom key struct
struct MyKey {
    int a, b;
};

// Custom hash function
struct MyKeyHash {
    std::size_t operator()(MyKey const& k) const noexcept {
        // Combine fields using XOR and shift for simple hashing
        // Better hash functions distribute keys more uniformly
        return std::hash<int>()(k.a) ^ (std::hash<int>()(k.b) << 1);
    }
};

// Custom equality comparator
struct MyKeyEq {
    bool operator()(MyKey const& x, MyKey const& y) const noexcept {
        // Define equality: keys equal if both fields equal
        return x.a == y.a && x.b == y.b;
    }
};

int main() {
    // Use custom hash and equality in template parameters
    std::unordered_map<MyKey, int, MyKeyHash, MyKeyEq> custom_map;

    // Insert elements
    custom_map[{1, 2}] = 10;
    custom_map[{3, 4}] = 20;

    // Access
    std::cout << "Value for {1,2}: " << custom_map[{1, 2}] << '\n';

    // Note: Prefer structs with operator() marked noexcept
    // Ensure hash function is fast and uniform to avoid collisions

    return 0;
}
```

## Advanced

### 1. Performance Characteristics

```cpp
#include <unordered_map>
#include <iostream>
#include <chrono>  // For timing

int main() {
    std::unordered_map<int, int> m;

    // Insert many elements
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < 100000; ++i) {
        m[i] = i * 2;
    }
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> insert_time = end - start;
    std::cout << "Insert time: " << insert_time.count() << " seconds\n";

    // Lookup
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < 100000; ++i) {
        volatile int val = m[i];  // Prevent optimization
    }
    end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> lookup_time = end - start;
    std::cout << "Lookup time: " << lookup_time.count() << " seconds\n";

    // Average-case: O(1) for find, insert, erase
    // Worst-case: O(n) if many keys collide (bad hash)
    // Performance depends on:
    // - Quality of Hash function (uniform distribution reduces collisions)
    // - Current load_factor() and bucket_count()
    // - Cost of computing hash and equality

    return 0;
}
```

### 2. Memory Usage and Tuning

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    std::unordered_map<int, int> m;

    // unordered_map uses more memory than ordered containers like std::map
    // Due to bucket arrays and node allocation overhead

    // Tip 1: Use reserve(n) when you know expected number of elements
    m.reserve(10000);  // Preallocate for 10k elements
    std::cout << "Buckets after reserve: " << m.bucket_count() << '\n';

    // Tip 2: Adjust max_load_factor() in memory-constrained scenarios
    // Lower value -> more buckets -> less collisions -> more memory
    m.max_load_factor(0.5f);  // More buckets, more memory
    std::cout << "Max load factor: " << m.max_load_factor() << '\n';

    // Tip 3: For small keys and many elements, consider third-party libraries
    // like Abseil's flat_hash_map or robin_hood_hashing for better memory/performance

    return 0;
}
```

### 3. Concurrency Notes

```cpp
#include <unordered_map>
#include <mutex>
#include <thread>
#include <iostream>

// Thread-safe wrapper
class ThreadSafeUnorderedMap {
    std::unordered_map<int, int> map_;
    std::mutex mutex_;

public:
    void insert(int key, int value) {
        std::lock_guard<std::mutex> lock(mutex_);
        map_[key] = value;
    }

    int get(int key) {
        std::lock_guard<std::mutex> lock(mutex_);
        auto it = map_.find(key);
        return it != map_.end() ? it->second : -1;
    }
};

int main() {
    ThreadSafeUnorderedMap tsm;

    // Concurrent reads from different threads are safe if no modifications
    // Any modifying operation must be externally synchronized (e.g., mutex)
    // Rehash causes widespread invalidation, be careful in concurrent setups

    std::thread t1([&]() { tsm.insert(1, 10); });
    std::thread t2([&]() { std::cout << tsm.get(1) << '\n'; });

    t1.join();
    t2.join();

    return 0;
}
```

### 4. Common Pitfalls

```cpp
#include <unordered_map>
#include <iostream>

int main() {
    std::unordered_map<std::string, int> m;

    // Pitfall 1: Relying on iteration order
    m["zebra"] = 1;
    m["apple"] = 2;
    m["banana"] = 3;
    std::cout << "Order is unspecified:\n";
    for (auto& p : m) std::cout << p.first << '\n';  // May not be insertion order

    // Pitfall 2: operator[] inserts default values accidentally
    int val = m["missing"];  // Inserts "missing" -> 0, then val = 0
    std::cout << "Accidental insert: " << m.size() << '\n';  // Size increased

    // Better: Use find or at
    auto it = m.find("apple");
    if (it != m.end()) {
        std::cout << "Found: " << it->second << '\n';
    }

    // Pitfall 3: Bad hash leading to collisions (simulate with poor hash)
    // Worst-case O(n) if many collisions

    // Pitfall 4: Forgetting reserve for large inserts
    std::unordered_map<int, int> large;
    // Without reserve, multiple rehashes during inserts
    for (int i = 0; i < 10000; ++i) large[i] = i;  // Inefficient

    return 0;
}
```

## Examples

### 1. Frequency Counting

```cpp
#include <iostream>
#include <unordered_map>
#include <string>
#include <vector>

int main() {
    std::vector<std::string> words = {"apple", "banana", "apple", "cherry", "banana", "apple"};

    std::unordered_map<std::string, int> freq;
    for (const auto& word : words) {
        ++freq[word];  // Count occurrences
    }

    std::cout << "Frequencies:\n";
    for (const auto& p : freq) {
        std::cout << p.first << ": " << p.second << '\n';
    }

    return 0;
}
```

### 2. Custom Key Type & Hash

```cpp
#include <iostream>
#include <unordered_map>

struct Point {
    int x, y;
};

struct PointHash {
    std::size_t operator()(const Point& p) const noexcept {
        // Better hash: use a standard hash combiner
        size_t h1 = std::hash<int>()(p.x);
        size_t h2 = std::hash<int>()(p.y);
        return h1 ^ (h2 << 1);  // Simple combine
    }
};

struct PointEq {
    bool operator()(const Point& a, const Point& b) const noexcept {
        return a.x == b.x && a.y == b.y;
    }
};

int main() {
    std::unordered_map<Point, std::string, PointHash, PointEq> point_map;

    point_map[{0, 0}] = "Origin";
    point_map[{1, 2}] = "Point A";
    point_map[{3, 4}] = "Point B";

    // Lookup
    auto it = point_map.find({1, 2});
    if (it != point_map.end()) {
        std::cout << "Found: " << it->second << '\n';
    }

    return 0;
}
```

### 3. Two Sum Problem

```cpp
#include <unordered_map>
#include <vector>
#include <iostream>

std::vector<int> twoSum(const std::vector<int>& nums, int target) {
    std::unordered_map<int, int> map;  // value -> index

    for (int i = 0; i < nums.size(); ++i) {
        int complement = target - nums[i];
        if (map.count(complement)) {
            return {map[complement], i};  // Found pair
        }
        map[nums[i]] = i;  // Store current
    }
    return {};  // No solution
}

int main() {
    std::vector<int> nums = {2, 7, 11, 15};
    int target = 9;
    auto result = twoSum(nums, target);
    if (!result.empty()) {
        std::cout << "Indices: " << result[0] << ", " << result[1] << '\n';
    }

    return 0;
}
```

### 4. Simple Cache (Memoization)

```cpp
#include <unordered_map>
#include <iostream>
#include <functional>

// Simple memoization for Fibonacci
std::unordered_map<int, long long> fib_cache;

long long fibonacci(int n) {
    if (n <= 1) return n;

    if (fib_cache.count(n)) {
        return fib_cache[n];  // Return cached value
    }

    long long result = fibonacci(n - 1) + fibonacci(n - 2);
    fib_cache[n] = result;  // Cache the result
    return result;
}

int main() {
    std::cout << "Fib(10): " << fibonacci(10) << '\n';
    std::cout << "Cache size: " << fib_cache.size() << '\n';

    return 0;
}
```

## Summary

### Best Practices
- Use `reserve(n)` when you expect many insertions.
- Use `try_emplace` / `emplace` to avoid unnecessary copies/moves.
- Provide a good `Hash` for custom key types; keep it fast and uniform.
- Avoid `operator[]` if you do not want implicit insertions.
- Consider `flat_hash_map` or other third-party implementations for performance-sensitive workloads.

### Common Use Cases
- Frequency counting and histograms.
- Memoization / caches.
- Fast key-value lookup where ordering is unnecessary.

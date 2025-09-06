# Deep Dive into C++ STL `std::vector`

`std::vector` is a sequence container that encapsulates dynamic size arrays. It's one of the most fundamental and widely used components of the C++ Standard Template Library (STL). Vectors are defined in the `<vector>` header.

This guide provides a comprehensive look at `std::vector`, from basic operations to advanced performance considerations.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Declaration and Initialization](#1-declaration-and-initialization)
    - [Accessing Elements](#2-accessing-elements)
    - [Adding Elements](#3-adding-elements)
    - [Removing Elements](#4-removing-elements)
    - [Size and Capacity](#5-size-and-capacity)
2.  [**Intermediate**](#intermediate)
    - [Iterators](#1-iterators)
    - [Algorithms](#2-using-algorithms-with-vectors)
    - [Data Pointer Access](#3-data-pointer-access)
3.  [**Advanced**](#advanced)
    - [Memory Management](#1-memory-management)
    - [Modern C++ Features](#2-modern-c-features-c11-and-beyond)
    - [Multi-Dimensional Vectors](#3-multi-dimensional-vectors)
    - [Common Pitfalls](#4-common-pitfalls-and-special-cases)
    - [Thread Safety](#5-thread-safety)
4.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases-in-ds-problems)

---

## Basics

### 1. Declaration and Initialization
Vectors can be declared empty or initialized in various ways.

```cpp
#include <iostream>
#include <vector>
#include <list>

void print_vector(const std::string& name, const std::vector<int>& v) {
    std::cout << name << ": ";
    for (int i : v) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
}

int main() {
    // Empty vector
    std::vector<int> v1;

    // Size 5, default-initialized to 0
    std::vector<int> v2(5);
    print_vector("v2", v2); // Output: v2: 0 0 0 0 0

    // Size 3, all elements initialized to 10
    std::vector<int> v3(3, 10);
    print_vector("v3", v3); // Output: v3: 10 10 10

    // From an initializer list (C++11)
    std::vector<int> v4 = {1, 2, 3};
    print_vector("v4", v4); // Output: v4: 1 2 3

    // Copy from another vector
    std::vector<int> v5(v4);
    print_vector("v5", v5); // Output: v5: 1 2 3

    // From a C-style array
    int arr[] = {4, 5, 6};
    std::vector<int> v6(arr, arr + sizeof(arr) / sizeof(int));
    print_vector("v6", v6); // Output: v6: 4 5 6

    // From another container (e.g., std::list)
    std::list<int> l = {7, 8};
    std::vector<int> v7(l.begin(), l.end());
    print_vector("v7", v7); // Output: v7: 7 8

    return 0;
}
```

### 2. Accessing Elements
- `[]`: Direct access. Fastest, but performs no bounds checking.
- `at()`: Access with bounds checking. Throws `std::out_of_range` if the index is invalid.
- `front()`: Access the first element.
- `back()`: Access the last element.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {10, 20, 30};
    std::cout << "v[1]: " << v[1] << std::endl;          // Output: v[1]: 20
    std::cout << "v.at(2): " << v.at(2) << std::endl;    // Output: v.at(2): 30
    std::cout << "v.front(): " << v.front() << std::endl;// Output: v.front(): 10
    std::cout << "v.back(): " << v.back() << std::endl;  // Output: v.back(): 30

    // v.at(3); // This would throw std::out_of_range
    // v[3];    // This is undefined behavior
    return 0;
}
```

### 3. Adding Elements
- `push_back()`: Adds an element to the end. Amortized O(1) complexity.
- `insert()`: Inserts elements at a specific position. O(n) complexity due to shifting existing elements.

```cpp
#include <iostream>
#include <vector>

void print_vector(const std::vector<int>& v) {
    for (int i : v) std::cout << i << " ";
    std::cout << std::endl;
}

int main() {
    std::vector<int> v = {10, 40};

    // Insert a single element
    v.insert(v.begin() + 1, 20);
    print_vector(v); // Output: 10 20 40

    // Insert multiple copies of the same element
    v.insert(v.begin() + 2, 2, 30);
    print_vector(v); // Output: 10 20 30 30 40

    // Insert elements from another container
    std::vector<int> to_insert = {50, 60};
    v.insert(v.end(), to_insert.begin(), to_insert.end());
    print_vector(v); // Output: 10 20 30 30 40 50 60
    return 0;
}
```

### 4. Removing Elements
- `pop_back()`: Removes the last element. O(1) complexity.
- `erase()`: Removes an element or a range of elements. O(n) complexity.
- `clear()`: Removes all elements, leaving the vector with a size of 0.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {10, 20, 30, 40, 50};
    v.pop_back(); // Removes 50
    // v is now {10, 20, 30, 40}

    v.erase(v.begin() + 1); // Removes 20
    // v is now {10, 30, 40}

    v.erase(v.begin(), v.begin() + 2); // Removes 10 and 30
    // v is now {40}

    std::cout << "Final element: " << v[0] << std::endl; // Output: 40

    v.clear();
    std::cout << "Size after clear: " << v.size() << std::endl; // Output: 0
    return 0;
}
```

### 5. Size and Capacity
- `size()`: The number of elements currently in the vector.
- `capacity()`: The number of elements the vector can hold before it must reallocate memory.
- `empty()`: Returns `true` if `size()` is 0.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v;
    std::cout << "Size: " << v.size() << ", Capacity: " << v.capacity() << std::endl;

    for (int i = 0; i < 10; ++i) {
        v.push_back(i);
        std::cout << "Size: " << v.size() << ", Capacity: " << v.capacity() << std::endl;
    }
    // Capacity typically grows exponentially (e.g., 0, 1, 2, 4, 8, 16...)
    // This makes push_back an amortized O(1) operation.
    return 0;
}
```

---

## Intermediate

### 1. Iterators
Iterators provide a way to access vector elements sequentially.
- `begin()` / `cbegin()`: Iterator to the first element.
- `end()` / `cend()`: Iterator to the theoretical element that follows the last element.
- `rbegin()` / `crbegin()`: Reverse iterator to the last element.
- `rend()` / `crend()`: Reverse iterator to the theoretical element preceding the first element.

> **Warning:** Modifying a vector (e.g., with `insert`, `erase`, `push_back`) can invalidate existing iterators, pointers, and references. See the [pitfalls section](#4-common-pitfalls-and-special-cases) for more details.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {10, 20, 30};

    std::cout << "Forward: ";
    for (auto it = v.begin(); it != v.end(); ++it) {
        std::cout << *it << " "; // Output: 10 20 30
    }
    std::cout << std::endl;

    std::cout << "Reverse: ";
    for (auto it = v.rbegin(); it != v.rend(); ++it) {
        std::cout << *it << " "; // Output: 30 20 10
    }
    std::cout << std::endl;
    return 0;
}
```

### 2. Using Algorithms with Vectors
Vectors work seamlessly with STL algorithms from the `<algorithm>` header.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>

int main() {
    std::vector<int> v = {40, 20, 30, 10};

    // Sort the vector
    std::sort(v.begin(), v.end()); // v is now {10, 20, 30, 40}

    // Find an element
    auto it = std::find(v.begin(), v.end(), 30);
    if (it != v.end()) {
        std::cout << "Found 30 at index: " << std::distance(v.begin(), it) << std::endl;
    }

    // Check for existence (on a sorted range)
    bool exists = std::binary_search(v.begin(), v.end(), 50);
    std::cout << "50 exists? " << (exists ? "Yes" : "No") << std::endl;

    // Sum all elements
    int sum = std::accumulate(v.begin(), v.end(), 0);
    std::cout << "Sum: " << sum << std::endl;
    return 0;
}
```

### 3. Data Pointer Access
The `data()` method returns a direct pointer to the underlying C-style array. This is useful for interoperability with C libraries.

> **Note:** The pointer returned by `data()` is only valid until the next non-`const` operation that might cause a reallocation (e.g., `push_back`, `insert`, `resize`).

```cpp
#include <iostream>
#include <vector>

// A C-style function that expects a pointer and a length
void process_data(const int* arr, size_t len) {
    std::cout << "C-style processing: ";
    for (size_t i = 0; i < len; ++i) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};
    process_data(v.data(), v.size()); // Pass vector's data to C-style function
    return 0;
}
```

---

## Advanced

### 1. Memory Management
- `reserve()`: Increases capacity to a planned value, preventing intermediate reallocations.
- `resize()`: Changes the size of the vector. If the new size is larger, new elements are value-initialized.
- `shrink_to_fit()`: Requests that the capacity be reduced to match the size. This is a non-binding request.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v;
    std::cout << "Initial capacity: " << v.capacity() << std::endl;

    // Pre-allocate memory to avoid reallocations
    v.reserve(100);
    std::cout << "Capacity after reserve: " << v.capacity() << std::endl;

    for (int i = 0; i < 50; ++i) {
        v.push_back(i); // No reallocations will occur
    }

    v.resize(10); // Shrinks size to 10
    std::cout << "Size after resize: " << v.size() << std::endl;
    std::cout << "Capacity after resize: " << v.capacity() << std::endl; // Still 100

    // Release unused memory
    v.shrink_to_fit();
    std::cout << "Capacity after shrink_to_fit: " << v.capacity() << std::endl; // Likely 10
    return 0;
}
```

### 2. Modern C++ Features (C++11 and beyond)
- **Move Semantics**: `std::vector` is move-aware, allowing efficient transfer of resources.
- **Emplacement**: `emplace_back()` and `emplace()` construct objects directly in the vector's memory, avoiding temporary objects and unnecessary copies/moves.

```cpp
#include <iostream>
#include <vector>

struct Point {
    int x, y;
    Point(int a, int b) : x(a), y(b) { std::cout << "Constructed\n"; }
    Point(const Point& other) : x(other.x), y(other.y) { std::cout << "Copied\n"; }
    Point(Point&& other) noexcept : x(other.x), y(other.y) { std::cout << "Moved\n"; }
};

int main() {
    std::vector<Point> v;
    std::cout << "--- push_back ---" << std::endl;
    v.push_back(Point(1, 2)); // Constructs a temporary, then moves it

    std::cout << "\n--- emplace_back ---" << std::endl;
    v.emplace_back(3, 4); // Constructs directly in-place

    std::cout << "\n--- Moving a vector ---" << std::endl;
    std::vector<Point> v2 = std::move(v); // Transfers ownership, v is now empty
    std::cout << "v size: " << v.size() << ", v2 size: " << v2.size() << std::endl;

    return 0;
}
```

### 3. Multi-Dimensional Vectors
Vectors of vectors are commonly used to create 2D matrices or jagged arrays.

```cpp
#include <iostream>
#include <vector>

void print_matrix(const std::vector<std::vector<int>>& matrix) {
    for (const auto& row : matrix) {
        for (int cell : row) {
            std::cout << cell << " ";
        }
        std::cout << std::endl;
    }
}

int main() {
    // 3x4 matrix initialized to 0
    std::vector<std::vector<int>> matrix(3, std::vector<int>(4, 0));
    matrix[1][2] = 5;
    print_matrix(matrix);

    std::cout << "\n--- Jagged Array ---" << std::endl;
    std::vector<std::vector<int>> jagged = {{1, 2}, {3}, {4, 5, 6}};
    print_matrix(jagged);
    return 0;
}
```

### 4. Common Pitfalls and Special Cases

#### Iterator Invalidation
This is a critical source of bugs. Operations that change the vector's size or capacity can invalidate iterators.

| Operation | Invalidates Iterators, Pointers, and References? |
| :--- | :--- |
| `push_back`, `emplace_back` | All are invalidated **if a reallocation occurs**. If no reallocation, only the `end()` iterator is invalidated. |
| `insert`, `emplace` | All iterators, pointers, and references **at or after the insertion point** are invalidated. |
| `erase` | All iterators, pointers, and references **at or after the erase point** are invalidated. |
| `resize` | If new size > old size, same as `push_back`. If new size < old size, same as `erase`. |
| `clear` | All are invalidated (unless `capacity()` is 0). |

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};
    auto it = v.begin();
    while (it != v.end()) {
        if (*it % 2 == 0) {
            // erase() returns an iterator to the next valid element
            it = v.erase(it);
        } else {
            ++it;
        }
    }
    for (int i : v) std::cout << i << " "; // Output: 1 3 5
    return 0;
}
```

#### The `vector<bool>` Specialization
`std::vector<bool>` is a space-efficient specialization that packs boolean values into bits. However, it does not store actual `bool` elements.
- It does not satisfy all container requirements.
- `operator[]` returns a proxy object, not a `bool&`.
- Because of its unusual behavior, it's often better to use `std::vector<char>` or `std::deque<bool>` if you need a standard-behaving container of booleans.

### 5. Thread Safety
Like most STL containers, `std::vector` has specific thread-safety guarantees:
- **Safe**: Calling `const` member functions on the same vector from multiple threads is safe (e.g., `begin()`, `size()`, `[]`, `at()`).
- **Unsafe**: Calling any non-`const` member function from one thread while other threads are reading or writing to the same vector is unsafe and causes a data race. You must use a mutex or other synchronization mechanism to protect access.

---

## Summary

### Best Practices
- **Prefer `emplace_back`** over `push_back` for non-trivial objects to avoid copies.
- **Use `reserve`** when you know the approximate number of elements you will add, to prevent costly reallocations.
- **Use `at()`** during development/debugging for bounds checking; switch to `[]` in performance-critical code once correctness is assured.
- **Be mindful of iterator invalidation**. When erasing in a loop, use the idiomatic `it = v.erase(it)`.
- **Prefer range-based for loops** for simple traversal.
- **Avoid `std::vector<bool>`** unless you specifically need its space-saving properties and are aware of its quirky interface.

### Common Use Cases in DS Problems
- **Dynamic Arrays**: The default choice for a list of elements that can grow.
- **Stacks**: Can be used as a stack (`push_back`, `pop_back`, `back`).
- **Adjacency Lists**: For graph representations (`vector<vector<int>> adj`).
- **Frequency Counts**: Storing items to be sorted and counted.

```cpp
#include <iostream>
#include <vector>

// Adjacency list representation of a simple graph
void graph_example() {
    int nodes = 3;
    std::vector<std::vector<int>> adj(nodes);

    // Edge 0 -> 1
    adj[0].push_back(1);
    // Edge 1 -> 2
    adj[1].push_back(2);

    for (int i = 0; i < nodes; ++i) {
        std::cout << "Node " << i << " is connected to: ";
        for (int neighbor : adj[i]) {
            std::cout << neighbor << " ";
        }
        std::cout << std::endl;
    }
}

int main() {
    graph_example();
    return 0;
}
```

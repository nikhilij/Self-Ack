# Deep Dive into C++ STL `std::queue` and `std::deque`

This guide explores two essential C++ Standard Template Library (STL) containers: `std::queue` and `std::deque`. While both manage collections of elements, they are designed for different use cases. `std::queue` is a container adapter enforcing First-In-First-Out (FIFO) logic, whereas `std::deque` is a versatile sequence container allowing efficient insertions and deletions at both ends.

We will break down each component, progressing from basic functions to advanced concepts, with complete, runnable code examples.

---

### Table of Contents
1.  [**`std::queue` (Container Adapter)**](#1-stdqueue-container-adapter)
    - [Basics](#queue-basics)
    - [Intermediate Usage](#queue-intermediate-usage)
    - [Advanced Concepts](#queue-advanced-concepts)
    - [Summary & Best Practices](#queue-summary--best-practices)
2.  [**`std::deque` (Double-Ended Queue)**](#2-stddeque-double-ended-queue)
    - [Basics](#deque-basics)
    - [Intermediate Usage](#deque-intermediate-usage)
    - [Advanced Concepts](#deque-advanced-concepts)
    - [Summary & Best Practices](#deque-summary--best-practices)
3.  [**Container Comparison**](#3-container-comparison)
    - [Vector vs. Deque vs. List](#vector-vs-deque-vs-list)

---

## 1. `std::queue` (Container Adapter)

The `std::queue` is not a container itself, but a **container adapter**. It wraps an underlying container (like `std::deque` or `std::list`) and provides a strict First-In-First-Out (FIFO) interface. It is perfect for scenarios where elements must be processed in the order they were added, such as a breadth-first search (BFS) or task scheduling.

### Queue: Table of Contents
- [**Basics**](#queue-basics)
  - [Key Functions](#key-functions)
  - [Basic Operations](#basic-operations)
- [**Intermediate Usage**](#queue-intermediate-usage)
  - [Custom Underlying Container](#custom-underlying-container)
  - [Use Case: Breadth-First Search (BFS)](#use-case-breadth-first-search-bfs)
- [**Advanced Concepts**](#queue-advanced-concepts)
  - [Emplace, Swap, and Move](#emplace-swap-and-move)
  - [Clearing a Queue](#clearing-a-queue)
- [**Summary & Best Practices**](#queue-summary--best-practices)

### Queue: Basics

#### Key Functions
- `push(value)`: Adds an element to the back of the queue. (Amortized O(1))
- `pop()`: Removes the element from the front. (O(1))
- `front()`: Accesses the front element. (O(1))
- `back()`: Accesses the back element. (O(1))
- `size()`: Returns the number of elements.
- `empty()`: Checks if the queue is empty.

#### Basic Operations
```cpp
#include <iostream>
#include <queue>

int main() {
    std::queue<int> q;

    q.push(10); // q is {10}
    q.push(20); // q is {10, 20}

    std::cout << "Front: " << q.front() << ", Back: " << q.back() << std::endl; // Output: Front: 10, Back: 20

    q.pop(); // Removes 10, q is {20}
    std::cout << "Front after pop: " << q.front() << std::endl; // Output: 20

    std::cout << "Size: " << q.size() << ", Is empty? " << (q.empty() ? "Yes" : "No") << std::endl; // Output: Size: 1, Is empty? No
    return 0;
}
```

### Queue: Intermediate Usage

#### Custom Underlying Container
By default, `std::queue` uses `std::deque`. You can specify `std::list` if, for example, you need to avoid `deque`'s memory layout.

```cpp
#include <iostream>
#include <queue>
#include <list>

int main() {
    // A queue that uses a std::list internally
    std::queue<int, std::list<int>> q;
    q.push(10);
    q.push(20);
    std::cout << "Front: " << q.front() << std::endl; // Output: Front: 10
    q.pop();
    std::cout << "New front: " << q.front() << std::endl; // Output: New front: 20
    return 0;
}
```

#### Use Case: Breadth-First Search (BFS)
`std::queue` is the ideal data structure for implementing BFS, which explores a graph level by level.

```cpp
#include <iostream>
#include <queue>
#include <vector>

void bfs(const std::vector<std::vector<int>>& graph, int start_node) {
    std::queue<int> q;
    std::vector<bool> visited(graph.size(), false);

    q.push(start_node);
    visited[start_node] = true;

    while (!q.empty()) {
        int current = q.front();
        q.pop();
        std::cout << current << " "; // Process the node

        for (int neighbor : graph[current]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
    std::cout << std::endl;
}

int main() {
    // Adjacency list for a simple graph
    std::vector<std::vector<int>> graph = {{1, 2}, {0, 3}, {0}, {1}};
    std::cout << "BFS traversal starting from node 0: ";
    bfs(graph, 0); // Output: 0 1 2 3
    return 0;
}
```

### Queue: Advanced Concepts

#### Emplace, Swap, and Move
- `emplace(args...)`: (C++11) Constructs an element in-place at the back, avoiding temporary objects.
- `swap(other_queue)`: Swaps the contents of two queues.
- **Move Semantics**: `std::queue` supports efficient moving, transferring ownership of the underlying container.

```cpp
#include <iostream>
#include <queue>

int main() {
    std::queue<int> q1;
    q1.push(10);
    q1.emplace(20); // Constructs 20 in-place

    std::queue<int> q2;
    q2.push(99);

    q1.swap(q2);
    std::cout << "q1 front after swap: " << q1.front() << std::endl; // Output: 99

    std::queue<int> q3 = std::move(q2); // q2 is now empty
    std::cout << "q3 front after move: " << q3.front() << std::endl; // Output: 10
    std::cout << "Is q2 empty? " << (q2.empty() ? "Yes" : "No") << std::endl; // Output: Yes
    return 0;
}
```

#### Clearing a Queue
> **Tip:** `std::queue` has no `clear()` method. The standard way to clear a queue is to swap it with a new, empty one.

```cpp
#include <iostream>
#include <queue>

int main() {
    std::queue<int> q;
    q.push(10);
    q.push(20);
    std::cout << "Size before clear: " << q.size() << std::endl; // Output: 2

    std::queue<int>().swap(q); // Swap with a temporary empty queue

    std::cout << "Size after clear: " << q.size() << std::endl; // Output: 0
    return 0;
}
```

### Queue: Summary & Best Practices
- Use `std::queue` when you need a strict FIFO policy.
- Its simplicity is its strength—it prevents accidental out-of-order access.
- For performance-critical object storage, prefer `emplace` over `push`.
- Remember the `swap` trick to clear a queue.

---

## 2. `std::deque` (Double-Ended Queue)

The `std::deque` is a sequence container that allows for fast O(1) insertions and deletions at both its beginning and its end. It also provides O(1) random access via `operator[]`. Unlike `std::vector`, its elements are not stored contiguously, but in a series of memory blocks.

### Deque: Table of Contents
- [**Basics**](#deque-basics)
  - [Key Functions](#key-functions-1)
  - [Basic Operations](#basic-operations-1)
- [**Intermediate Usage**](#deque-intermediate-usage)
  - [Iterator Usage](#iterator-usage)
  - [Use Case: Sliding Window](#use-case-sliding-window-maximum)
- [**Advanced Concepts**](#deque-advanced-concepts)
  - [Memory and Performance](#memory-and-performance)
  - [Iterator Invalidation](#iterator-invalidation)
- [**Summary & Best Practices**](#deque-summary--best-practices)

### Deque: Basics

#### Key Functions
- `push_back(value)` / `push_front(value)`: Add to back/front.
- `pop_back()` / `pop_front()`: Remove from back/front.
- `operator[]` / `at()`: Random access.
- `insert(it, val)` / `erase(it)`: O(n) middle insertion/deletion.
- `begin()` / `end()`, `rbegin()` / `rend()`: Full iterator support.

#### Basic Operations
```cpp
#include <iostream>
#include <deque>

int main() {
    std::deque<int> d;
    d.push_back(20);  // d is {20}
    d.push_front(10); // d is {10, 20}
    d.push_back(30);  // d is {10, 20, 30}

    std::cout << "d[1]: " << d[1] << std::endl; // Output: 20
    std::cout << "Front: " << d.front() << ", Back: " << d.back() << std::endl; // Output: 10, 30

    d.pop_front(); // d is {20, 30}
    d.pop_back();  // d is {20}
    std::cout << "Final element: " << d.front() << std::endl; // Output: 20
    return 0;
}
```

### Deque: Intermediate Usage

#### Iterator Usage
`std::deque` provides random-access iterators, making it compatible with all STL algorithms like `std::sort`.

```cpp
#include <iostream>
#include <deque>
#include <algorithm>

int main() {
    std::deque<int> d = {30, 10, 20};
    
    // Sort the deque
    std::sort(d.begin(), d.end());

    std::cout << "Sorted deque: ";
    for (int val : d) {
        std::cout << val << " "; // Output: 10 20 30
    }
    std::cout << std::endl;
    return 0;
}
```

#### Use Case: Sliding Window Maximum
`std::deque` is perfect for solving sliding window problems, as it can efficiently track candidates by adding from one end and removing from the other.

```cpp
#include <iostream>
#include <vector>
#include <deque>

void print_sliding_window_max(const std::vector<int>& nums, int k) {
    std::deque<int> dq; // Stores indices of elements

    for (int i = 0; i < nums.size(); ++i) {
        // Remove indices that are out of the current window
        if (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }
        // Maintain a monotonically decreasing deque of values
        while (!dq.empty() && nums[dq.back()] < nums[i]) {
            dq.pop_back();
        }
        dq.push_back(i);

        // The front of the deque is the max of the current window
        if (i >= k - 1) {
            std::cout << nums[dq.front()] << " ";
        }
    }
    std::cout << std::endl;
}

int main() {
    std::vector<int> nums = {1, 3, -1, -3, 5, 3, 6, 7};
    int k = 3;
    std::cout << "Sliding window max: ";
    print_sliding_window_max(nums, k); // Output: 3 3 5 5 6 7
    return 0;
}
```

### Deque: Advanced Concepts

#### Memory and Performance
> **Note:** `std::deque` does not have `capacity()` or `reserve()` because its storage is not contiguous. It grows by allocating new fixed-size blocks, not by reallocating and copying a single large block like `std::vector`. This makes front insertions O(1) but can lead to lower cache performance.

- `emplace_front`/`emplace_back`: Construct elements in-place at either end.
- `shrink_to_fit()`: (C++11) A non-binding request to free unused memory blocks.

#### Iterator Invalidation
- **Insertions**:
  - `push_front`/`push_back`: Invalidates all iterators, but pointers and references to existing elements remain valid.
  - `insert` (middle): Invalidates all iterators, pointers, and references.
- **Deletions**:
  - `pop_front`/`pop_back`: Only invalidates iterators, pointers, and references to the removed element.
  - `erase` (middle): Invalidates all iterators, pointers, and references.

### Deque: Summary & Best Practices
- Choose `std::deque` when you need efficient insertions/deletions at **both ends**.
- It's an excellent choice for implementing queues, stacks, or solving sliding window problems.
- If you only need back insertions and contiguous memory (for cache performance), `std::vector` is usually better.

---

## 3. Container Comparison

### Vector vs. Deque vs. List

| Feature | `std::vector` | `std::deque` | `std::list` |
| :--- | :--- | :--- | :--- |
| **Memory** | Contiguous | Segmented blocks | Node-based (non-contiguous) |
| **Random Access** | O(1) | O(1) | O(n) |
| **Front Insert/Delete**| O(n) | **O(1)** | **O(1)** |
| **Back Insert/Delete** | Amortized O(1) | **O(1)** | **O(1)** |
| **Middle Insert/Delete**| O(n) | O(n) | **O(1)** (with iterator) |
| **Cache Performance**| **Excellent** | Good | Poor |
| **Iterator Invalidation**| Frequent on insertion | Less frequent on insertion | Never on insertion |

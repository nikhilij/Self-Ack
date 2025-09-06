### Deep Dive into C++ STL Vector: From Basics to Advanced

Vectors in C++ STL are dynamic arrays that can grow or shrink in size automatically. They are defined in the `<vector>` header and are templated, meaning they can hold any data type (e.g., `vector<int>`, `vector<string>`). Vectors provide random access (O(1) time for accessing elements by index) and are contiguous in memory, making them efficient for many operations.

I'll structure this as:
- **Basics**: Declaration, initialization, basic access, and modifications.
- **Intermediate**: Size vs capacity, iterators, common algorithms with vectors.
- **Advanced**: Emplacement, custom allocators, move semantics, multi-dimensional vectors, performance optimizations, and pitfalls.

For each concept or function, I'll explain briefly, then provide one or more complete, runnable code examples (using `#include <bits/stdc++.h>` for simplicity; compile with g++). Examples build on each other where possible to show progression.

---

### Basics

#### 1. Declaration and Initialization
- Vectors can be declared empty or with initial size/values.
- Common ways: empty, with size, with initializer list, copy from another vector.

Example: Empty vector and basic initialization.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v1;  // Empty vector
    vector<int> v2(5);  // Size 5, default values (0 for int)
    vector<int> v3(3, 10);  // Size 3, all elements 10
    vector<int> v4 = {1, 2, 3};  // Initializer list (C++11+)
    vector<int> v5(v4);  // Copy from v4

    cout << "v3: ";
    for(int i : v3) cout << i << " ";  // Output: v3: 10 10 10
    cout << "\nv5: ";
    for(int i : v5) cout << i << " ";  // Output: v5: 1 2 3
    return 0;
}
```

Example: Initializing from array or another container.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int arr[] = {4, 5, 6};
    vector<int> v(arr, arr + sizeof(arr)/sizeof(int));  // From array

    list<int> l = {7, 8};
    vector<int> v2(l.begin(), l.end());  // From list

    cout << "v: ";
    for(int i : v) cout << i << " ";  // Output: v: 4 5 6
    cout << "\nv2: ";
    for(int i : v2) cout << i << " ";  // Output: v2: 7 8
    return 0;
}
```

#### 2. Accessing Elements
- Use `[]` (no bounds check, faster but unsafe), `at()` (bounds check, throws exception if out of range), `front()`, `back()`.

Example: Basic access.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30};
    cout << v[1] << " " << v.at(2) << " " << v.front() << " " << v.back();  // Output: 20 30 10 30
    // v.at(3);  // Would throw std::out_of_range
    return 0;
}
```

#### 3. Adding Elements
- `push_back()`: Adds to end (amortized O(1)).
- `insert()`: Adds at specific position (O(n) due to shifting).

Example: push_back.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v;
    v.push_back(10);
    v.push_back(20);
    for(int i : v) cout << i << " ";  // Output: 10 20
    return 0;
}
```

Example: insert at position.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {10, 30};
    v.insert(v.begin() + 1, 20);  // Insert 20 at index 1
    v.insert(v.end(), 40);  // Insert at end (like push_back)
    for(int i : v) cout << i << " ";  // Output: 10 20 30 40
    return 0;
}
```

Example: Insert multiple elements.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 4};
    vector<int> to_insert = {2, 3};
    v.insert(v.begin() + 1, to_insert.begin(), to_insert.end());  // Insert range
    for(int i : v) cout << i << " ";  // Output: 1 2 3 4
    return 0;
}
```

#### 4. Removing Elements
- `pop_back()`: Removes last (O(1)).
- `erase()`: Removes at position or range (O(n)).
- `clear()`: Removes all.

Example: pop_back and erase.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30, 40};
    v.pop_back();  // Removes 40
    v.erase(v.begin() + 1);  // Removes 20
    v.erase(v.begin(), v.begin() + 1);  // Removes first element (10)
    for(int i : v) cout << i << " ";  // Output: 30
    return 0;
}
```

Example: clear.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};
    v.clear();
    cout << v.size() << " " << (v.empty() ? "Empty" : "Not");  // Output: 0 Empty
    return 0;
}
```

#### 5. Size and Empty Check
- `size()`: Current number of elements.
- `empty()`: True if size == 0.

Example:
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v;
    cout << v.size() << " " << v.empty();  // Output: 0 1 (true)
    v.push_back(1);
    cout << " " << v.size() << " " << v.empty();  // Output: 1 0 (false)
    return 0;
}
```

---

### Intermediate

#### 1. Size vs Capacity
- `size()`: Number of elements.
- `capacity()`: Allocated storage (can be > size; grows exponentially, usually doubles).
- `resize()`: Changes size (adds default or removes).
- `reserve()`: Increases capacity without changing size (prevents reallocations).

Example: Observing capacity growth.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v;
    cout << "Size: " << v.size() << ", Capacity: " << v.capacity() << endl;  // Size: 0, Capacity: 0
    v.push_back(1);
    cout << "Size: " << v.size() << ", Capacity: " << v.capacity() << endl;  // Size: 1, Capacity: 1 (or more, implementation-dependent)
    for(int i = 2; i <= 10; ++i) v.push_back(i);  // Capacity doubles as needed (e.g., 1->2->4->8->16)
    cout << "Size: " << v.size() << ", Capacity: " << v.capacity();  // Size: 10, Capacity: >=10 (e.g., 16)
    return 0;
}
```

Example: resize.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 2};
    v.resize(5);  // Adds 3 default (0) elements
    for(int i : v) cout << i << " ";  // Output: 1 2 0 0 0
    v.resize(3);  // Removes last 2
    cout << endl;
    for(int i : v) cout << i << " ";  // Output: 1 2 0
    return 0;
}
```

Example: reserve to optimize.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v;
    v.reserve(100);  // Pre-allocate for 100 elements
    cout << "Capacity: " << v.capacity();  // Output: Capacity: >=100
    for(int i = 0; i < 50; ++i) v.push_back(i);  // No reallocations
    cout << " Size: " << v.size();  // Size: 50
    return 0;
}
```

#### 2. Iterators
- `begin()`/`end()`: Forward iterators.
- `rbegin()`/`rend()`: Reverse.
- Iterators can be invalidated by modifications (e.g., insert/erase).

Example: Using iterators for traversal.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30};
    for(auto it = v.begin(); it != v.end(); ++it) {
        cout << *it << " ";  // Output: 10 20 30
    }
    cout << endl;
    for(auto it = v.rbegin(); it != v.rend(); ++it) {
        cout << *it << " ";  // Output: 30 20 10
    }
    return 0;
}
```

Example: Modifying via iterators.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};
    for(auto it = v.begin(); it != v.end(); ++it) {
        *it *= 2;  // Modify elements
    }
    for(int i : v) cout << i << " ";  // Output: 2 4 6
    return 0;
}
```

Example: Erase with iterator (returns next iterator).
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3, 4};
    auto it = v.begin() + 1;
    it = v.erase(it);  // Erase 2, it now points to 3
    cout << *it << endl;  // Output: 3
    for(int i : v) cout << i << " ";  // Output: 1 3 4
    return 0;
}
```

#### 3. Using Algorithms with Vectors
- Vectors work seamlessly with STL algorithms since they provide random access iterators.

Example: Sorting and searching.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {4, 2, 3, 1};
    sort(v.begin(), v.end());  // Sort ascending
    for(int i : v) cout << i << " ";  // Output: 1 2 3 4
    auto it = find(v.begin(), v.end(), 3);
    cout << "\nFound: " << *it;  // Found: 3
    bool exists = binary_search(v.begin(), v.end(), 5);
    cout << " " << (exists ? "Yes" : "No");  // No
    return 0;
}
```

Example: Custom sort (descending).
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 3, 2};
    sort(v.begin(), v.end(), greater<int>());
    for(int i : v) cout << i << " ";  // Output: 3 2 1
    return 0;
}
```

Example: Accumulate sum.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};
    int sum = accumulate(v.begin(), v.end(), 0);
    cout << sum;  // Output: 6
    return 0;
}
```

---

### Advanced

#### 1. Emplacement (C++11+)
- `emplace_back()`: Constructs element in-place (avoids copy/move for efficiency).
- `emplace()`: Like insert, but in-place.

Example: push_back vs emplace_back (with custom type).
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Point {
    int x, y;
    Point(int a, int b) : x(a), y(b) { cout << "Constructed\n"; }
};

int main() {
    vector<Point> v;
    v.push_back(Point(1, 2));  // Temporary constructed, then moved
    // Output: Constructed (for temp), possibly another for move
    v.emplace_back(3, 4);  // Constructed directly in vector
    // Output: Constructed (only once)
    cout << v[0].x << " " << v[1].y;  // Output: 1 4 (after constructions)
    return 0;
}
```

Example: emplace at position.
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Point {
    int x, y;
    Point(int a, int b) : x(a), y(b) {}
};

int main() {
    vector<Point> v = {Point(1,1)};
    v.emplace(v.begin(), 0, 0);  // Emplace at beginning
    cout << v[0].x << " " << v[1].y;  // Output: 0 1
    return 0;
}
```

#### 2. Shrink to Fit and Memory Management
- `shrink_to_fit()`: Reduces capacity to match size (C++11+).
- Useful to release excess memory.

Example:
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v(1000, 1);  // Capacity >=1000
    v.resize(10);  // Size=10, capacity still large
    cout << "Capacity before: " << v.capacity() << endl;  // e.g., 1000
    v.shrink_to_fit();  // Reduce capacity
    cout << "Capacity after: " << v.capacity();  // e.g., 10
    return 0;
}
```

#### 3. Move Semantics (C++11+)
- Vectors support move constructors/assignments to transfer ownership efficiently.

Example: Moving a vector.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v1 = {1, 2, 3};
    vector<int> v2 = move(v1);  // Move: v1 becomes empty
    cout << "v2 size: " << v2.size() << " v1 size: " << v1.size();  // v2 size: 3 v1 size: 0
    return 0;
}
```

#### 4. Multi-Dimensional Vectors
- Vectors of vectors for 2D arrays (jagged or rectangular).

Example: 2D vector (matrix).
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<vector<int>> matrix(3, vector<int>(4, 0));  // 3x4 matrix of 0s
    matrix[1][2] = 5;
    for(auto& row : matrix) {
        for(int cell : row) cout << cell << " ";
        cout << endl;
    }
    // Output:
    // 0 0 0 0
    // 0 0 5 0
    // 0 0 0 0
    return 0;
}
```

Example: Jagged array.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<vector<int>> jagged = {{1,2}, {3}, {4,5,6}};
    for(auto& row : jagged) {
        for(int cell : row) cout << cell << " ";
        cout << endl;
    }
    // Output:
    // 1 2
    // 3
    // 4 5 6
    return 0;
}
```

#### 5. Custom Allocators (Advanced Memory Control)
- Rarely used, but allows custom memory allocation (e.g., for pooled memory).

Example: Basic custom allocator stub (for illustration; full implementation is complex).
```cpp
#include <bits/stdc++.h>
using namespace std;

template <class T>
struct CustomAlloc : allocator<T> {};  // Inherit and override if needed

int main() {
    vector<int, CustomAlloc<int>> v;  // Uses custom allocator
    v.push_back(10);
    cout << v[0];  // Output: 10
    return 0;
}
```

#### 6. Performance Considerations and Pitfalls
- **Performance**: Push_back is fast, but frequent inserts in middle are slow (O(n)). Use reserve() for large growth. Vectors are cache-friendly due to contiguity.
- **Pitfalls**: 
  - Iterators invalidate after insert/erase (except end() sometimes).
  - Accessing invalid index with [] causes undefined behavior.
  - For large objects, use emplace to avoid copies.
  - Vectors of bool are specialized (bit-packed, no direct reference).

Example: Iterator invalidation pitfall.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {1, 2, 3};
    auto it = v.begin() + 1;  // Points to 2
    v.insert(v.begin(), 0);  // Invalidates it (reallocation possible)
    // cout << *it;  // Undefined behavior! Don't do this.
    // Fix: Re-get iterator after modification.
    it = v.begin() + 2;  // Now points to 2 again
    cout << *it;  // Output: 2
    return 0;
}
```

Example: Vector<bool> specialty (acts like bitset).
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<bool> vb(3, true);
    // bool& ref = vb[0];  // Won't compile; returns proxy
    vb.flip();  // Not standard vector function, but bit-like
    // Use regular vector<char> or bitset for true bool arrays if needed.
    cout << vb[0] << " " << vb[1];  // Output: 0 0 (after flip, if supported; behavior varies)
    return 0;
}
```
(Note: `vector<bool>` is controversial; avoid for production if possible.)

#### 7. Common Use Cases in DS Problems
- Stacks/queues (but prefer stack/queue adaptors).
- Dynamic arrays for graphs (adjacency lists: `vector<vector<int>>`).
- Frequency counts with sorting.

Example: Adjacency list for graph.
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int nodes = 3;
    vector<vector<int>> adj(nodes);
    adj[0].push_back(1);  // Edge 0->1
    adj[1].push_back(2);  // Edge 1->2
    for(int i = 0; i < nodes; ++i) {
        cout << i << ": ";
        for(int neighbor : adj[i]) cout << neighbor << " ";
        cout << endl;
    }
    // Output:
    // 0: 1
    // 1: 2
    // 2:
    return 0;
}
```

This covers vectors comprehensively. Practice by modifying these examples for DS problems like implementing a stack or sorting algorithms. Next, we can move to lists or another component—let me know!

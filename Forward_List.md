# std::forward_list

## Table of Contents
1. [Introduction](#introduction)
2. [Basic Concepts](#basic-concepts)
   - [What is std::forward_list?](#what-is-stdforward_list)
   - [Key Characteristics](#key-characteristics)
3. [Intermediate Topics](#intermediate-topics)
   - [Common Operations](#common-operations)
   - [Iterators](#iterators)
   - [Memory Management](#memory-management)
4. [Advanced Topics](#advanced-topics)
   - [Performance Considerations](#performance-considerations)
   - [Use Cases](#use-cases)
5. [Code Examples](#code-examples)
   - [Basic Usage](#basic-usage)
   - [Advanced Usage](#advanced-usage)
6. [Conclusion](#conclusion)
7. [References](#references)

## Introduction
`std::forward_list` is a standard library container in C++ that provides a singly-linked list implementation. It is part of the STL (Standard Template Library) and is particularly useful in memory-constrained environments and scenarios where insertion and deletion of elements at the front and back are frequent.

## Basic Concepts

### What is std::forward_list?
`std::forward_list` is a container that allows for dynamic size and efficient insertion and deletion of elements. Unlike `std::list`, which is a doubly-linked list, `std::forward_list` only allows traversal in one direction.

### Key Characteristics
- **Singly-Linked Structure**: Each node contains a value and a pointer to the next node.
- **Memory Management**: Allocates memory for each element independently.
- **No Back Pointers**: Unlike `std::list`, there are no pointers to the previous elements, which saves memory.

## Intermediate Topics

### Common Operations
- **Insertion**: Elements can be added to the front using `push_front()`. 
- **Deletion**: Elements can be removed from the front using `pop_front()`. 
- **Size**: The number of elements can be obtained using the `size()` method.

### Iterators
`std::forward_list` provides iterators for traversing the elements. The iterator supports operations like `++` for moving to the next element.

### Memory Management
With `std::forward_list`, memory is allocated for each element individually. This can lead to fragmentation but allows for dynamic resizing.

## Advanced Topics

### Performance Considerations
- **Time Complexity**: Insertion and deletion at the front are O(1) operations, while searching is O(n).
- **Space Complexity**: More efficient than `std::list` due to the absence of back pointers.

### Use Cases
- Ideal for stacks and queues where only front operations are needed.
- Suitable for scenarios with frequent insertions and deletions.

## Code Examples

### Basic Usage
```cpp
#include <forward_list>
#include <iostream>

int main() {
    std::forward_list<int> flist = {1, 2, 3};
    flist.push_front(0); // Adds 0 at the front

    for (const auto& elem : flist) {
        std::cout << elem << " "; // Output: 0 1 2 3
    }
    return 0;
}
```

### Advanced Usage
```cpp
#include <forward_list>
#include <iostream>

int main() {
    std::forward_list<int> flist = {1, 2, 3, 4, 5};
    
    // Remove elements after the first
    flist.remove_if([](int n) { return n % 2 == 0; });

    for (const auto& elem : flist) {
        std::cout << elem << " "; // Output: 1 3 5
    }
    return 0;
}
```

## Conclusion
`std::forward_list` is a powerful container for managing collections of data in a singly-linked manner. It provides efficient operations for insertion and deletion, making it suitable for various applications.

## References
- C++ Standard Library Documentation
- Effective STL by Scott Meyers
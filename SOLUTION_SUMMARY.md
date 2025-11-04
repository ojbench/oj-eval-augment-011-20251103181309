# Solution Summary for Problem 011 - Priority Queue

## Problem Overview
- **ACMOJ Problem ID**: 2623
- **Problem**: Implement a priority_queue container similar to std::priority_queue
- **Key Requirements**:
  - Implement a max-heap data structure
  - Merge operation with O(log n) complexity
  - Exception safety when Compare function throws
  - No memory leaks
  - Support all standard priority queue operations

## Solution Approach

### Data Structure: Leftist Heap
I implemented a **leftist heap** (also known as leftist tree), which is a mergeable heap data structure that satisfies:
1. **Heap property**: Parent node has higher priority than children (max-heap)
2. **Leftist property**: For every node, the null path length (npl) of the left child ≥ npl of the right child

### Key Features

#### 1. Node Structure
```cpp
struct Node {
    T data;
    Node *left;
    Node *right;
    int npl; // null path length
};
```

#### 2. Merge Operation - O(log n)
The merge operation is the core of the leftist heap:
- Compare roots and make the larger one the new root
- Recursively merge the smaller heap with the right subtree
- Swap left and right children if needed to maintain leftist property
- Update null path length

#### 3. Exception Safety
All operations (push, pop, merge) implement strong exception guarantee:
- Save original state before operation
- Perform operation with try-catch
- If Compare throws exception, restore original state and throw runtime_error
- This ensures the heap remains in a valid state even when exceptions occur

#### 4. Memory Management
- Deep copy in copy constructor and assignment operator
- Proper cleanup in destructor
- Exception-safe memory allocation (cleanup on failure)

## Implementation Details

### Operations Complexity
- **push**: O(log n) - creates new node and merges with existing heap
- **pop**: O(log n) - removes root and merges left and right subtrees
- **top**: O(1) - returns root element
- **merge**: O(log n) - merges two heaps
- **size**: O(1) - maintains count
- **empty**: O(1) - checks if root is null

### Exception Handling Strategy
For operations that may trigger Compare exceptions:
1. **push**: Save old root, create new node, merge. On exception: restore old root, delete new node
2. **pop**: Save old root and children, merge children. On exception: restore old root
3. **merge**: Save both roots and counts. On exception: restore both heaps

## Test Results

### Local Testing
All provided test cases passed:
- ✅ one: Basic operations, constructors, exceptions
- ✅ two: Comprehensive operations test
- ✅ three: Custom types and copy operations
- ✅ five: Large-scale merge test (800,000 elements)
- ✅ six: Exception safety tests

### ACMOJ Submission Results
- **Submission ID**: 706921
- **Status**: Accepted
- **Score**: 100/100
- **All Test Groups Passed**:
  1. one (10/10) - 3ms, 4.8MB
  2. two (10/10) - 56ms, 6.9MB
  3. three (10/10) - 49ms, 6.2MB
  4. four (10/10) - 2ms, 4.2MB
  5. five (10/10) - 688ms, 45.4MB
  6. one.memcheck (10/10) - 731ms, 52.6MB
  7. two.memcheck (10/10) - 1483ms, 92.1MB
  8. three.memcheck (10/10) - 2881ms, 241.2MB
  9. four.memcheck (10/10) - 731ms, 50.1MB
  10. five.memcheck (10/10) - 5124ms, 200.2MB

### Performance Analysis
- All tests completed well within time limits (max 5.1s vs 15s limit)
- Memory usage reasonable for the data sizes
- No memory leaks detected in memcheck tests

## Key Design Decisions

1. **Leftist Heap over Binary Heap**: Binary heap doesn't support efficient O(log n) merge
2. **Leftist Heap over Binomial/Fibonacci Heap**: Simpler implementation, sufficient performance
3. **Strong Exception Guarantee**: Ensures data structure integrity even with faulty comparators
4. **Recursive Merge**: Clean and elegant implementation, stack depth is O(log n)

## Conclusion
The implementation successfully meets all requirements:
- ✅ O(log n) merge complexity
- ✅ Exception safety with state restoration
- ✅ No memory leaks
- ✅ All standard operations working correctly
- ✅ Perfect score on ACMOJ (100/100)

The leftist heap provides an elegant solution that balances simplicity with performance requirements.


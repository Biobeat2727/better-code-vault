
Understanding fundamental algorithms and data structures is crucial for solving complex problems efficiently and writing performant code.

---

## 📊 Common Data Structures

### Arrays & Lists

- **Use**: Ordered collections, random access
- **Time**: O(1) access, O(n) search
- **Best for**: Known size, frequent lookups

### Hash Tables / Objects

- **Use**: Key-value pairs, fast lookups
- **Time**: O(1) average access
- **Best for**: Caching, counting, deduplication

### Stacks & Queues

- **Stack**: LIFO (Last In, First Out)
- **Queue**: FIFO (First In, First Out)
- **Best for**: Function calls, breadth-first search

### Trees & Graphs

- **Binary Trees**: Hierarchical data
- **Graphs**: Connected nodes, networks
- **Best for**: File systems, social networks

---

## 🔍 Essential Algorithms

### Sorting

```js
// Quick Sort (divide & conquer)
function quickSort(arr) {
  if (arr.length <= 1) return arr;
  const pivot = arr[0];
  const left = arr.filter(x => x < pivot);
  const right = arr.filter(x => x > pivot);
  return [...quickSort(left), pivot, ...quickSort(right)];
}
```

### Searching

```js
// Binary Search (sorted arrays)
function binarySearch(arr, target) {
  let left = 0, right = arr.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}
```

### Graph Traversal

```js
// Breadth-First Search
function bfs(graph, start) {
  const visited = new Set();
  const queue = [start];
  
  while (queue.length > 0) {
    const node = queue.shift();
    if (!visited.has(node)) {
      visited.add(node);
      queue.push(...graph[node]);
    }
  }
}
```

---

## 💭 Algorithm Paradigms

|Approach|Description|Examples|
|---|---|---|
|**Brute Force**|Try all possibilities|Password cracking|
|**Divide & Conquer**|Break into subproblems|Merge sort, binary search|
|**Dynamic Programming**|Store solutions to subproblems|Fibonacci, shortest path|
|**Greedy**|Make locally optimal choices|Dijkstra's algorithm|
|**Backtracking**|Try solutions, undo if wrong|Sudoku solver|

---

## 🧠 Problem-Solving Strategy

1. **Understand** the problem clearly
2. **Plan** your approach (which data structure/algorithm?)
3. **Code** a working solution first
4. **Optimize** for time/space complexity
5. **Test** with edge cases

---

## 🚀 Common Interview Problems

- Two Sum / Three Sum
- Reverse a linked list
- Find cycles in graphs
- Validate binary search trees
- Implement LRU cache
- Merge intervals

---

## 📚 Resources

- [LeetCode](https://leetcode.com/) – Practice problems
- [Visualgo](https://visualgo.net/) – Algorithm visualizations
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- Cracking the Coding Interview (book)

---

## ✅ Learning Checklist

- [ ] Master array manipulation and string processing
- [ ] Understand hash tables and their use cases
- [ ] Implement basic sorting algorithms
- [ ] Practice tree and graph traversals
- [ ] Learn dynamic programming patterns
- [ ] Solve 50+ coding problems on LeetCode
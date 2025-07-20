
Big-O notation helps you analyze algorithm efficiency and choose the best solution for your constraints.

---

## 📐 Big-O Notation

**Time Complexity**: How runtime scales with input size  
**Space Complexity**: How memory usage scales with input size

### Common Complexities (Best to Worst)

|Notation|Name|Example|
|---|---|---|
|O(1)|Constant|Hash table lookup|
|O(log n)|Logarithmic|Binary search|
|O(n)|Linear|Array scan|
|O(n log n)|Linearithmic|Merge sort|
|O(n²)|Quadratic|Nested loops|
|O(2ⁿ)|Exponential|Fibonacci (naive)|

---

## ⚡ Time Complexity Examples

### O(1) - Constant Time

```js
function getFirst(arr) {
  return arr[0]; // Always one operation
}
```

### O(n) - Linear Time

```js
function findMax(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) max = arr[i];
  }
  return max; // Must check every element
}
```

### O(n²) - Quadratic Time

```js
function bubbleSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
}
```

### O(log n) - Logarithmic Time

```js
function binarySearch(arr, target) {
  let left = 0, right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1; // Eliminates half each iteration
}
```

---

## 💾 Space Complexity

### O(1) - Constant Space

```js
function sum(a, b) {
  return a + b; // No extra space needed
}
```

### O(n) - Linear Space

```js
function createCopy(arr) {
  return [...arr]; // New array of same size
}
```

### O(n) - Recursive Space

```js
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1); // Call stack grows with n
}
```

---

## 🧠 Analysis Tips

### Identify the Dominant Term

```js
// O(n² + n + 1) → O(n²)
function example(arr) {
  // O(n²) - nested loops dominate
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      console.log(arr[i], arr[j]);
    }
  }
  
  // O(n) - single loop
  for (let k = 0; k < arr.length; k++) {
    console.log(arr[k]);
  }
  
  // O(1) - constant operation
  console.log("Done");
}
```

### Common Patterns

|Pattern|Complexity|Example|
|---|---|---|
|Single loop|O(n)|Searching unsorted array|
|Nested loops|O(n²)|Comparing all pairs|
|Halving input|O(log n)|Binary search|
|Recursive with memoization|O(n)|Dynamic programming|
|All subsets|O(2ⁿ)|Power set generation|

---

## ⚖️ Time vs Space Trade-offs

### Caching (Space for Time)

```js
// Slow: O(2ⁿ) time, O(n) space
function fibSlow(n) {
  if (n <= 1) return n;
  return fibSlow(n - 1) + fibSlow(n - 2);
}

// Fast: O(n) time, O(n) space
function fibFast(n, memo = {}) {
  if (n in memo) return memo[n];
  if (n <= 1) return n;
  memo[n] = fibFast(n - 1, memo) + fibFast(n - 2, memo);
  return memo[n];
}
```

---

## 🎯 Real-World Impact

### Frontend Performance

- **O(n²) rendering**: Avoid nested loops in React renders
- **O(1) lookups**: Use Maps/Objects instead of arrays for frequent searches
- **O(log n) updates**: Consider tree-like structures for large datasets

### API Performance

- **Database queries**: Ensure proper indexing for O(log n) lookups
- **Pagination**: Process large datasets in chunks
- **Caching**: Trade memory for faster response times

---

## 🚨 Common Mistakes

- **Premature optimization**: Profile first, optimize bottlenecks
- **Ignoring constants**: O(n) with huge constants can be slower than O(n²)
- **Only considering worst case**: Sometimes average case matters more

---

## 📊 Complexity Cheat Sheet

### Data Structure Operations

|Structure|Access|Search|Insert|Delete|
|---|---|---|---|---|
|Array|O(1)|O(n)|O(n)|O(n)|
|Hash Table|O(1)|O(1)|O(1)|O(1)|
|Binary Tree|O(log n)|O(log n)|O(log n)|O(log n)|
|Heap|O(1)|O(n)|O(log n)|O(log n)|

### Sorting Algorithms

|Algorithm|Best|Average|Worst|Space|
|---|---|---|---|---|
|Quick Sort|O(n log n)|O(n log n)|O(n²)|O(log n)|
|Merge Sort|O(n log n)|O(n log n)|O(n log n)|O(n)|
|Bubble Sort|O(n)|O(n²)|O(n²)|O(1)|

---

## ✅ Learning Goals

- [ ] Recognize common complexity patterns in code
- [ ] Calculate time/space complexity for algorithms
- [ ] Choose appropriate data structures for your use case
- [ ] Identify optimization opportunities in existing code
- [ ] Make informed trade-offs between time and space
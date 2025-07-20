
Understanding how JavaScript manages memory helps you write performant applications and avoid memory leaks that can slow down or crash your apps.

---

## 🧠 How JavaScript Memory Works

### Memory Lifecycle

1. **Allocation**: JavaScript automatically allocates memory when you create variables
2. **Usage**: Reading and writing to allocated memory
3. **Release**: Garbage collector automatically frees unused memory

### Memory Regions

|Region|Purpose|Examples|
|---|---|---|
|**Stack**|Function calls, primitives|Numbers, booleans, function calls|
|**Heap**|Objects, closures|Objects, arrays, functions|
|**Global**|Global variables|Window object, global variables|

---

## 🔍 Primitive vs Reference Types

### Primitives (Stored in Stack)

```js
let a = 5;
let b = a; // Copy of value
a = 10;
console.log(b); // Still 5

// Primitives: number, string, boolean, null, undefined, symbol, bigint
```

### Objects (Stored in Heap)

```js
let obj1 = { count: 5 };
let obj2 = obj1; // Copy of reference
obj1.count = 10;
console.log(obj2.count); // 10 (same object!)

// References: objects, arrays, functions
```

---

## 🗑️ Garbage Collection

JavaScript automatically removes objects that are no longer reachable from your code.

### Mark and Sweep Algorithm

```js
function createObjects() {
  let obj1 = { data: 'temp' };    // ✅ Created
  let obj2 = { ref: obj1 };       // ✅ Created, references obj1
  
  // Do some work...
  
  return obj2;                    // ✅ obj2 returned (stays alive)
}                                 // ❌ obj1 becomes unreachable (garbage collected)

const result = createObjects();   // obj2 still alive
// obj1 was automatically cleaned up
```

### Reference Counting (Legacy)

```js
// Problem: Circular references
function createCircular() {
  let obj1 = {};
  let obj2 = {};
  
  obj1.ref = obj2;  // obj1 -> obj2
  obj2.ref = obj1;  // obj2 -> obj1 (circular!)
  
  // In old browsers, this could cause memory leaks
  // Modern browsers handle this with mark-and-sweep
}
```

---

## 💥 Common Memory Leaks

### 1. Global Variables

```js
// ❌ Bad - creates global variables
function processData() {
  // Forgot 'let' - creates global!
  data = new Array(1000000).fill('leak');
  results = [];
}

// ✅ Good - properly scoped
function processData() {
  let data = new Array(1000000).fill('data');
  let results = [];
}
```

### 2. Event Listeners Not Removed

```js
// ❌ Bad - listener never removed
function setupButton() {
  const button = document.getElementById('myButton');
  const heavyData = new Array(1000000).fill('data');
  
  button.addEventListener('click', function() {
    console.log(heavyData.length); // Closure keeps heavyData alive
  });
  
  // heavyData never gets garbage collected!
}

// ✅ Good - remove listener when done
function setupButton() {
  const button = document.getElementById('myButton');
  const heavyData = new Array(1000000).fill('data');
  
  const clickHandler = function() {
    console.log(heavyData.length);
  };
  
  button.addEventListener('click', clickHandler);
  
  // Cleanup function
  return function cleanup() {
    button.removeEventListener('click', clickHandler);
  };
}
```

### 3. Timers Not Cleared

```js
// ❌ Bad - timer keeps running
function startPolling() {
  const heavyData = new Array(1000000).fill('data');
  
  setInterval(() => {
    console.log(heavyData.length);
  }, 1000);
  
  // heavyData never gets freed!
}

// ✅ Good - clear timer when done
function startPolling() {
  const heavyData = new Array(1000000).fill('data');
  
  const timerId = setInterval(() => {
    console.log(heavyData.length);
  }, 1000);
  
  // Return cleanup function
  return function stopPolling() {
    clearInterval(timerId);
  };
}
```

### 4. Detached DOM Nodes

```js
// ❌ Bad - DOM removed but reference kept
let detachedElements = [];

function removeElements() {
  const parent = document.getElementById('container');
  const children = parent.children;
  
  // Store references before removing
  for (let child of children) {
    detachedElements.push(child); // Keeps DOM nodes alive!
  }
  
  parent.innerHTML = ''; // Removes from DOM but not memory
}

// ✅ Good - don't keep references to removed elements
function removeElements() {
  const parent = document.getElementById('container');
  parent.innerHTML = ''; // Let garbage collector handle it
}
```

---

## 🔍 Memory Profiling & Debugging

### Chrome DevTools Memory Tab

```js
// Add this to help identify objects in memory profiler
class UserData {
  constructor(name, data) {
    this.name = name;
    this.data = data;
    
    // Add identifier for debugging
    this._debugId = `UserData_${Date.now()}_${Math.random()}`;
  }
}

// Take heap snapshots before and after operations
// Look for objects that shouldn't be there
```

### Memory Usage Monitoring

```js
function monitorMemory() {
  if ('memory' in performance) {
    const memory = performance.memory;
    console.log({
      used: Math.round(memory.usedJSHeapSize / 1048576) + ' MB',
      total: Math.round(memory.totalJSHeapSize / 1048576) + ' MB',
      limit: Math.round(memory.jsHeapSizeLimit / 1048576) + ' MB'
    });
  }
}

// Monitor every 5 seconds
setInterval(monitorMemory, 5000);
```

---

## 🎛️ Memory Optimization Techniques

### 1. Object Pooling

```js
class ObjectPool {
  constructor(createFn, resetFn, initialSize = 10) {
    this.createFn = createFn;
    this.resetFn = resetFn;
    this.pool = [];
    
    // Pre-populate pool
    for (let i = 0; i < initialSize; i++) {
      this.pool.push(this.createFn());
    }
  }
  
  acquire() {
    if (this.pool.length > 0) {
      return this.pool.pop();
    }
    return this.createFn();
  }
  
  release(obj) {
    this.resetFn(obj);
    this.pool.push(obj);
  }
}

// Usage
const vectorPool = new ObjectPool(
  () => ({ x: 0, y: 0 }),
  (obj) => { obj.x = 0; obj.y = 0; }
);

function processVectors() {
  const vector = vectorPool.acquire();
  vector.x = 10;
  vector.y = 20;
  
  // Use vector...
  
  vectorPool.release(vector); // Reuse instead of garbage collection
}
```

### 2. Lazy Loading

```js
class DataManager {
  constructor() {
    this._cache = new Map();
  }
  
  getData(id) {
    // Only load when needed
    if (!this._cache.has(id)) {
      this._cache.set(id, this.loadData(id));
    }
    return this._cache.get(id);
  }
  
  clearUnused() {
    // Periodically clear old data
    const cutoff = Date.now() - 300000; // 5 minutes
    for (let [id, data] of this._cache) {
      if (data.lastAccessed < cutoff) {
        this._cache.delete(id);
      }
    }
  }
}
```

### 3. WeakMap and WeakSet

```js
// WeakMap - keys can be garbage collected
const elementData = new WeakMap();

function attachData(element, data) {
  elementData.set(element, data);
}

function removeElement(element) {
  element.remove(); // Element + data automatically cleaned up
  // No need to manually clean WeakMap
}

// Regular Map would prevent garbage collection:
// const regularMap = new Map(); // ❌ Keeps references alive
```

### 4. Memory-Efficient Data Structures

```js
// ❌ Memory inefficient
const users = [
  { id: 1, name: 'John', age: 30, email: 'john@example.com' },
  { id: 2, name: 'Jane', age: 25, email: 'jane@example.com' }
  // ... thousands more
];

// ✅ More memory efficient
class UserStore {
  constructor() {
    this.ids = new Uint32Array(1000);
    this.names = [];
    this.ages = new Uint8Array(1000);
    this.emails = [];
    this.length = 0;
  }
  
  addUser(id, name, age, email) {
    const index = this.length++;
    this.ids[index] = id;
    this.names[index] = name;
    this.ages[index] = age;
    this.emails[index] = email;
  }
  
  getUser(index) {
    return {
      id: this.ids[index],
      name: this.names[index],
      age: this.ages[index],
      email: this.emails[index]
    };
  }
}
```

---

## ⚛️ React Memory Management

### Cleanup in useEffect

```js
function MyComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    let isMounted = true;
    const controller = new AbortController();
    
    async function fetchData() {
      try {
        const response = await fetch('/api/data', {
          signal: controller.signal
        });
        const result = await response.json();
        
        // Only update state if component still mounted
        if (isMounted) {
          setData(result);
        }
      } catch (error) {
        if (!controller.signal.aborted) {
          console.error('Fetch failed:', error);
        }
      }
    }
    
    fetchData();
    
    // Cleanup function
    return () => {
      isMounted = false;
      controller.abort();
    };
  }, []);
  
  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

### Preventing Memory Leaks in Event Handlers

```js
function ImageGallery() {
  const [images, setImages] = useState([]);
  
  useEffect(() => {
    function handleResize() {
      // Handle window resize
    }
    
    function handleScroll() {
      // Handle scroll events
    }
    
    window.addEventListener('resize', handleResize);
    window.addEventListener('scroll', handleScroll);
    
    // ✅ Always cleanup event listeners
    return () => {
      window.removeEventListener('resize', handleResize);
      window.removeEventListener('scroll', handleScroll);
    };
  }, []);
  
  return <div>{/* Gallery content */}</div>;
}
```

---

## 🚀 Performance Best Practices

### 1. Avoid Creating Objects in Render

```js
// ❌ Bad - creates new object every render
function MyComponent({ items }) {
  return (
    <div>
      {items.map(item => (
        <Item 
          key={item.id}
          style={{ margin: '10px' }} // New object every time!
          data={item}
        />
      ))}
    </div>
  );
}

// ✅ Good - reuse objects
const itemStyle = { margin: '10px' };

function MyComponent({ items }) {
  return (
    <div>
      {items.map(item => (
        <Item 
          key={item.id}
          style={itemStyle} // Reused object
          data={item}
        />
      ))}
    </div>
  );
}
```

### 2. Memoization

```js
import { useMemo, useCallback } from 'react';

function ExpensiveComponent({ data, filter }) {
  // ✅ Memoize expensive calculations
  const processedData = useMemo(() => {
    return data
      .filter(item => item.category === filter)
      .map(item => ({
        ...item,
        computed: expensiveCalculation(item)
      }));
  }, [data, filter]);
  
  // ✅ Memoize callbacks to prevent child re-renders
  const handleClick = useCallback((id) => {
    // Handle click
  }, []);
  
  return (
    <div>
      {processedData.map(item => (
        <Item key={item.id} data={item} onClick={handleClick} />
      ))}
    </div>
  );
}
```

---

## 📊 Memory Monitoring Tools

### Browser Tools

- **Chrome DevTools Memory Tab**: Heap snapshots, allocation timelines
- **Firefox Memory Tool**: Similar memory profiling capabilities
- **Performance.measureUserAgentSpecificMemory()**: API for memory measurement

### Node.js Tools

```js
// Monitor memory in Node.js
function logMemoryUsage() {
  const usage = process.memoryUsage();
  console.log({
    rss: Math.round(usage.rss / 1024 / 1024) + ' MB',
    heapTotal: Math.round(usage.heapTotal / 1024 / 1024) + ' MB',
    heapUsed: Math.round(usage.heapUsed / 1024 / 1024) + ' MB',
    external: Math.round(usage.external / 1024 / 1024) + ' MB'
  });
}

setInterval(logMemoryUsage, 5000);
```

---

## ✅ Memory Management Checklist

### Development

- [ ] Remove event listeners when components unmount
- [ ] Clear timers and intervals
- [ ] Abort fetch requests on cleanup
- [ ] Avoid global variables
- [ ] Use WeakMap/WeakSet for temporary associations

### Production Monitoring

- [ ] Monitor memory usage metrics
- [ ] Set up alerts for memory leaks
- [ ] Profile memory in staging environment
- [ ] Test with realistic data volumes
- [ ] Use memory-efficient data structures for large datasets

### Code Review

- [ ] Check for proper cleanup in useEffect
- [ ] Verify event listeners are removed
- [ ] Look for circular references
- [ ] Ensure timers are cleared
- [ ] Review large object allocations

---

## 📚 Key Takeaways

**Automatic but Not Magic**: JavaScript handles memory automatically, but you need to avoid creating leaks  
**Clean Up After Yourself**: Always remove event listeners, clear timers, and abort requests  
**Profile in Production**: Use browser tools to find real memory issues  
**Optimize When Needed**: Don't prematurely optimize, but know techniques when you need them

---

## ✅ Learning Goals

- [ ] Understand JavaScript's memory model and garbage collection
- [ ] Identify and fix common memory leak patterns
- [ ] Use browser DevTools to profile memory usage
- [ ] Implement proper cleanup in React components
- [ ] Apply memory optimization techniques when needed
- [ ] Monitor memory usage in production applications
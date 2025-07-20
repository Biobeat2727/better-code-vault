
Asynchronous programming lets you handle time-consuming operations without blocking the main thread, crucial for responsive UIs and scalable backends.

---

## 🧠 Core Concepts

**Synchronous**: Code runs line by line, blocks until complete  
**Asynchronous**: Code can run "in the background" while other code continues

### The Problem Async Solves

```js
// ❌ Blocking - freezes the UI
function fetchDataSync() {
  const data = makeSlowNetworkRequest(); // Takes 2 seconds
  return data; // Nothing else can happen during this time
}

// ✅ Non-blocking - UI stays responsive
async function fetchDataAsync() {
  const data = await makeSlowNetworkRequest(); // Other code can run
  return data;
}
```

---

## 📞 Callbacks (Legacy Pattern)

```js
function fetchUser(id, callback) {
  setTimeout(() => {
    const user = { id, name: 'John' };
    callback(null, user); // Error first, data second
  }, 1000);
}

fetchUser(1, (error, user) => {
  if (error) {
    console.error('Failed:', error);
  } else {
    console.log('User:', user);
  }
});
```

**Problems**: Callback hell, hard to handle errors

---

## 🤝 Promises

```js
function fetchUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({ id, name: 'John' });
      } else {
        reject(new Error('Invalid ID'));
      }
    }, 1000);
  });
}

// Using Promises
fetchUser(1)
  .then(user => console.log('User:', user))
  .catch(error => console.error('Error:', error));
```

### Promise States

- **Pending**: Initial state
- **Fulfilled**: Operation completed successfully
- **Rejected**: Operation failed

---

## ⚡ Async/Await (Modern Approach)

```js
async function getUser(id) {
  try {
    const user = await fetchUser(id);
    console.log('User:', user);
    return user;
  } catch (error) {
    console.error('Error:', error);
    throw error; // Re-throw if needed
  }
}

// Call async function
getUser(1);
```

**Benefits**: Reads like synchronous code, easier error handling

---

## 🔄 Common Async Patterns

### Sequential Execution

```js
async function processUsers() {
  const user1 = await fetchUser(1); // Wait for this
  const user2 = await fetchUser(2); // Then this
  return [user1, user2];
}
```

### Parallel Execution

```js
async function processUsersParallel() {
  const [user1, user2] = await Promise.all([
    fetchUser(1), // Both start at the same time
    fetchUser(2)
  ]);
  return [user1, user2];
}
```

### Promise.all vs Promise.allSettled

```js
// Promise.all - fails if ANY promise rejects
const results = await Promise.all([fetchUser(1), fetchUser(2)]);

// Promise.allSettled - waits for ALL promises (success or failure)
const results = await Promise.allSettled([fetchUser(1), fetchUser(2)]);
```

---

## 🌐 Real-World Examples

### API Calls

```js
async function getPostWithComments(postId) {
  try {
    // Get post and comments in parallel
    const [post, comments] = await Promise.all([
      fetch(`/api/posts/${postId}`).then(r => r.json()),
      fetch(`/api/posts/${postId}/comments`).then(r => r.json())
    ]);
    
    return { ...post, comments };
  } catch (error) {
    console.error('Failed to load post:', error);
    throw error;
  }
}
```

### Database Operations

```js
async function createUser(userData) {
  const transaction = await db.beginTransaction();
  
  try {
    const user = await db.users.create(userData);
    await db.userProfiles.create({ userId: user.id });
    await transaction.commit();
    return user;
  } catch (error) {
    await transaction.rollback();
    throw error;
  }
}
```

### File Operations

```js
import { readFile } from 'fs/promises';

async function processFiles(filenames) {
  const fileContents = await Promise.all(
    filenames.map(name => readFile(name, 'utf8'))
  );
  
  return fileContents.map(content => content.toUpperCase());
}
```

---

## 🎛️ Advanced Patterns

### Retry Logic

```js
async function fetchWithRetry(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url);
      if (response.ok) return response;
      throw new Error(`HTTP ${response.status}`);
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await new Promise(resolve => setTimeout(resolve, 1000 * i));
    }
  }
}
```

### Timeout Pattern

```js
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Timeout')), ms)
  );
  
  return Promise.race([promise, timeout]);
}

// Usage
const data = await withTimeout(fetch('/api/data'), 5000);
```

### Queue Processing

```js
class AsyncQueue {
  constructor(concurrency = 1) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }
  
  async add(task) {
    return new Promise((resolve, reject) => {
      this.queue.push({
        task,
        resolve,
        reject
      });
      this.process();
    });
  }
  
  async process() {
    if (this.running >= this.concurrency || this.queue.length === 0) {
      return;
    }
    
    this.running++;
    const { task, resolve, reject } = this.queue.shift();
    
    try {
      const result = await task();
      resolve(result);
    } catch (error) {
      reject(error);
    } finally {
      this.running--;
      this.process();
    }
  }
}
```

---

## ⚠️ Common Pitfalls

### Forgetting await

```js
// ❌ Wrong - returns a Promise, not the data
function getUser() {
  return fetchUser(1); // Missing await
}

// ✅ Correct
async function getUser() {
  return await fetchUser(1);
}
```

### Unhandled Promise Rejections

```js
// ❌ Wrong - error silently swallowed
fetchUser(1); // No .catch() or try/catch

// ✅ Correct
fetchUser(1).catch(error => console.error(error));
```

### Sequential vs Parallel Confusion

```js
// ❌ Slow - runs sequentially (6 seconds total)
const user1 = await fetchUser(1); // 3 seconds
const user2 = await fetchUser(2); // 3 seconds

// ✅ Fast - runs in parallel (3 seconds total)
const [user1, user2] = await Promise.all([
  fetchUser(1), // Both start together
  fetchUser(2)
]);
```

---

## 🏗️ Framework-Specific Patterns

### React

```js
function UserComponent({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    async function loadUser() {
      try {
        const userData = await fetchUser(userId);
        setUser(userData);
      } catch (error) {
        console.error('Failed to load user:', error);
      } finally {
        setLoading(false);
      }
    }
    
    loadUser();
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  return <div>{user?.name}</div>;
}
```

### Node.js Express

```js
app.get('/users/:id', async (req, res) => {
  try {
    const user = await fetchUser(req.params.id);
    res.json(user);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

---

## 📚 Tools & Libraries

- **Axios**: Promise-based HTTP client
- **node-fetch**: Fetch API for Node.js
- **p-limit**: Control promise concurrency
- **bottleneck**: Rate limiting for APIs

---

## ✅ Learning Checklist

- [ ] Understand the difference between sync and async
- [ ] Write functions using async/await syntax
- [ ] Handle errors properly with try/catch
- [ ] Use Promise.all for parallel execution
- [ ] Implement retry and timeout patterns
- [ ] Debug async code effectively
- [ ] Avoid common async/await pitfalls
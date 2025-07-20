
Quick reference guides for common programming tasks, syntax, and commands. These are designed for fast lookup during development when you need syntax reminders or quick solutions.

---

## 🟨 JavaScript Essentials

### Array Methods

```js
// 🔍 Finding & Filtering
const numbers = [1, 2, 3, 4, 5, 6];

// Find single item
const found = numbers.find(n => n > 3);        // 4
const foundIndex = numbers.findIndex(n => n > 3); // 3

// Filter multiple items
const evens = numbers.filter(n => n % 2 === 0); // [2, 4, 6]

// Check conditions
const hasEven = numbers.some(n => n % 2 === 0);  // true
const allPositive = numbers.every(n => n > 0);  // true

// 🔄 Transforming
// Map - transform each element
const doubled = numbers.map(n => n * 2);        // [2, 4, 6, 8, 10, 12]

// Reduce - combine to single value
const sum = numbers.reduce((acc, n) => acc + n, 0); // 21

// FlatMap - map + flatten
const nested = [[1, 2], [3, 4]];
const flattened = nested.flatMap(arr => arr);   // [1, 2, 3, 4]

// 🏗️ Modifying (these mutate original array)
const fruits = ['apple', 'banana'];
fruits.push('orange');          // ['apple', 'banana', 'orange']
fruits.pop();                   // ['apple', 'banana']
fruits.unshift('grape');        // ['grape', 'apple', 'banana']
fruits.shift();                 // ['apple', 'banana']

// Non-mutating alternatives
const added = [...fruits, 'orange'];           // Add to end
const removed = fruits.slice(0, -1);           // Remove from end
const prepended = ['grape', ...fruits];        // Add to beginning
const withoutFirst = fruits.slice(1);          // Remove from beginning
```

### String Methods

```js
const text = "  Hello World  ";

// 🧹 Cleaning
text.trim();                    // "Hello World"
text.toLowerCase();             // "  hello world  "
text.toUpperCase();             // "  HELLO WORLD  "

// 🔍 Searching
text.includes('World');         // true
text.indexOf('World');          // 8
text.startsWith('  Hello');     // true
text.endsWith('World  ');       // true

// ✂️ Extracting
text.slice(2, 7);              // "Hello"
text.substring(2, 7);          // "Hello"
text.split(' ');               // ["", "", "Hello", "World", "", ""]

// 🔄 Replacing
text.replace('World', 'JavaScript'); // "  Hello JavaScript  "
text.replaceAll(' ', '_');            // "__Hello_World__"

// 📏 Length & Padding
text.length;                    // 15
'5'.padStart(3, '0');          // "005"
'5'.padEnd(3, '0');            // "500"
```

### Object Operations

```js
const user = { name: 'John', age: 30, city: 'NYC' };

// 🔑 Keys, Values, Entries
Object.keys(user);             // ['name', 'age', 'city']
Object.values(user);           // ['John', 30, 'NYC']
Object.entries(user);          // [['name', 'John'], ['age', 30], ['city', 'NYC']]

// 🔄 Transforming
// From entries back to object
const entries = [['a', 1], ['b', 2]];
Object.fromEntries(entries);   // { a: 1, b: 2 }

// 📋 Copying & Merging
const copy = { ...user };                    // Shallow copy
const updated = { ...user, age: 31 };        // Update property
const merged = { ...user, country: 'USA' };  // Add property

// Using Object.assign
const copy2 = Object.assign({}, user);
const updated2 = Object.assign({}, user, { age: 31 });

// 🗑️ Destructuring
const { name, age, ...rest } = user;  // name='John', age=30, rest={city:'NYC'}
const { city: location } = user;      // location='NYC' (rename)
const { country = 'Unknown' } = user; // country='Unknown' (default)
```

### Async/Await Patterns

```js
// 🔄 Basic async/await
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) throw new Error('User not found');
    const user = await response.json();
    return user;
  } catch (error) {
    console.error('Error fetching user:', error);
    throw error;
  }
}

// 🚀 Parallel execution
async function fetchMultipleUsers(ids) {
  try {
    // All requests start simultaneously
    const promises = ids.map(id => fetchUser(id));
    const users = await Promise.all(promises);
    return users;
  } catch (error) {
    // If any request fails, this catches it
    throw error;
  }
}

// 🛡️ Handle some failures
async function fetchUsersWithFallback(ids) {
  const promises = ids.map(async (id) => {
    try {
      return await fetchUser(id);
    } catch (error) {
      return null; // Return null for failed requests
    }
  });
  
  const results = await Promise.all(promises);
  return results.filter(user => user !== null);
}

// ⏱️ With timeout
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Timeout')), ms)
  );
  return Promise.race([promise, timeout]);
}

// Usage
const user = await withTimeout(fetchUser(1), 5000);
```

---

## ⚛️ React Hooks

### useState Patterns

```jsx
// 🏗️ Basic state
const [count, setCount] = useState(0);
const [user, setUser] = useState(null);
const [items, setItems] = useState([]);

// 🔄 Functional updates (when new state depends on previous)
setCount(prev => prev + 1);
setItems(prev => [...prev, newItem]);
setUser(prev => ({ ...prev, name: 'Updated' }));

// 🏃‍♂️ Lazy initial state (expensive calculation)
const [expensiveValue, setExpensiveValue] = useState(() => {
  return computeExpensiveValue(); // Only runs once
});

// 📋 Object state pattern
const [form, setForm] = useState({ name: '', email: '' });

// Update single field
const updateField = (field, value) => {
  setForm(prev => ({ ...prev, [field]: value }));
};

// Usage in input
<input 
  value={form.name}
  onChange={e => updateField('name', e.target.value)}
/>
```

### useEffect Patterns

```jsx
// 🔄 Component mount/unmount
useEffect(() => {
  console.log('Component mounted');
  
  return () => {
    console.log('Component unmounting');
  };
}, []); // Empty dependency array = run once

// 📡 Data fetching
useEffect(() => {
  let cancelled = false;
  
  async function fetchData() {
    try {
      const data = await api.getData();
      if (!cancelled) {
        setData(data);
      }
    } catch (error) {
      if (!cancelled) {
        setError(error);
      }
    }
  }
  
  fetchData
```
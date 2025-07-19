# What Are Design Patterns?

  

Design patterns are **general, reusable solutions to common problems** that arise in software design. They’re like **templates or blueprints** — not finished code, but structured strategies you can adapt and apply to various situations.

  

---

  

## 🧠 Why Use Design Patterns?

  

- ✅ Make your code **easier to understand**

- ✅ Improve **communication** with other developers (shared vocabulary)

- ✅ Enhance **reusability** and **scalability**

- ✅ Prevent **reinventing the wheel**

  

They are especially helpful in large projects, collaborative teams, or when structuring complex logic or systems.

  

---

  

## 📚 The Three Main Categories

  

### 1. **Creational Patterns**

> *Focus on how objects are created.*

  

| Pattern     | Purpose                             |

|-------------|-------------------------------------|

| Singleton   | Ensure a class has only one instance|

| Factory     | Create objects without specifying class |

| Builder     | Construct complex objects step-by-step |

  

---

  

### 2. **Structural Patterns**

> *Deal with object composition — how classes and objects are combined.*

  

| Pattern     | Purpose                                   |

|-------------|-------------------------------------------|

| Adapter     | Make incompatible interfaces work together|

| Decorator   | Add responsibilities without modifying code|

| Composite   | Treat individual objects and groups uniformly|

  

---

  

### 3. **Behavioral Patterns**

> *Handle communication and responsibility between objects.*

  

| Pattern     | Purpose                                 |

|-------------|-----------------------------------------|

| Observer    | Notify many objects of state changes    |

| Strategy    | Encapsulate interchangeable algorithms  |

| Command     | Encapsulate a request as an object      |

  

---

  

## 🛠️ How Are They Applied?

  

Design patterns are not libraries or frameworks. You apply them in your own code structure.

  

For example:

- Instead of manually triggering component changes, use **Observer** to notify subscribers.

- Instead of hardcoding sort methods, use **Strategy** to dynamically swap algorithms.

- Use **Factory** to generate UI components based on input data.

  

---

  

## 🎯 Real-World Example: Strategy in a React App

  

```js

const sortAscending = (a, b) => a - b;

const sortDescending = (a, b) => b - a;

  

function sortList(data, strategy) {

  return data.sort(strategy);

}

```

  

With this setup, you can easily switch between sorting strategies.

  

---

  

## 🧭 TL;DR

  

- **Design patterns = reusable design solutions**

- They're abstract, not exact code

- Use them to **structure**, **scale**, and **simplify** your codebase

  

Explore the patterns in this folder to deepen your understanding 🔍
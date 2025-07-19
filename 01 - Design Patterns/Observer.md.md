# Observer Pattern

  

**Category**: Behavioral  

**Purpose**: Allow multiple objects to listen to and react to state changes in another object.

  

---

  

## 🧠 Concept

A "subject" maintains a list of observers and notifies them when a change occurs.

  

---

  

## 📦 JavaScript Example

  

```js

class Subject {

  constructor() {

    this.observers = [];

  }

  subscribe(fn) {

    this.observers.push(fn);

  }

  notify(data) {

    this.observers.forEach(fn => fn(data));

  }

}

```

  

---

  

## 🚀 Real-World Uses

- React state hooks

- Event-driven UIs

- Pub/Sub systems
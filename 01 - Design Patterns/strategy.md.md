# Strategy Pattern

  

**Category**: Behavioral  

**Purpose**: Define a family of algorithms, encapsulate each one, and make them interchangeable.

  

---

  

## 🧠 Concept

Lets you swap out different behaviors or algorithms without changing the object that uses them.

  

---

  

## 📦 JavaScript Example

  

```js

class Sorter {

  constructor(strategy) {

    this.strategy = strategy;

  }

  sort(data) {

    return this.strategy.sort(data);

  }

}

  

const ascending = {

  sort: arr => arr.sort((a, b) => a - b)

};

  

const descending = {

  sort: arr => arr.sort((a, b) => b - a)

};

  

const sorter = new Sorter(ascending);

console.log(sorter.sort([4, 2, 7, 1]));

```

  

---

  

## 🚀 Real-World Uses

- Sorting/filtering UIs

- Payment gateways with different providers
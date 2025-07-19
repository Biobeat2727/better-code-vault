# Singleton Pattern

  

**Category**: Creational  

**Purpose**: Ensure a class has only one instance and provide a global point of access to it.

  

---

  

## 🧠 Concept

A singleton pattern restricts the instantiation of a class to a single object and ensures controlled access to that instance.

  

---

  

## 📦 JavaScript Example

  

```js

class AppSettings {

  constructor() {

    if (AppSettings.instance) return AppSettings.instance;

    this.darkMode = false;

    AppSettings.instance = this;

  }

}

const settings1 = new AppSettings();

const settings2 = new AppSettings();

console.log(settings1 === settings2); // true

```

  

---

  

## 🚀 Real-World Uses

- Central configuration objects

- Redux store

- Logging service
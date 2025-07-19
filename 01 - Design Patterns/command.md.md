# Command Pattern

  

**Category**: Behavioral  

**Purpose**: Encapsulate a request as an object, thereby allowing you to parametrize clients, queue requests, and support undoable operations.

  

---

  

## 🧠 Concept

Used when you need to decouple the sender of a request from the receiver.

  

---

  

## 📦 JavaScript Example

  

```js

class Light {

  on() { console.log("Light on"); }

  off() { console.log("Light off"); }

}

  

class LightCommand {

  constructor(light) {

    this.light = light;

  }

  execute() {

    this.light.on();

  }

}

```

  

---

  

## 🚀 Real-World Uses

- Button/shortcut bindings

- Undo/redo functionality
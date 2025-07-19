# Factory Pattern

  

**Category**: Creational  

**Purpose**: Define an interface for creating an object, but let subclasses alter the type of objects that will be created.

  

---

  

## 🧠 Concept

Useful when the exact type of the object isn’t known until runtime.

  

---

  

## 📦 JavaScript Example

  

```js

class ButtonFactory {

  createButton(type) {

    if (type === 'fancy') return new FancyButton();

    else return new BasicButton();

  }

}

```

  

---

  

## 🚀 Real-World Uses

- Component generation (modals, toasts)

- Theme selectors
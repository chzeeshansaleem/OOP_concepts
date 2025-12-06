# OOP in JavaScript - Beginner Level Interview Preparation Guide

## 📚 Table of Contents
1. [Introduction to OOP](#introduction-to-oop)
2. [Data Types & Memory Model](#data-types--memory-model)
3. [Object Fundamentals](#object-fundamentals)
4. [Functions & Methods Basics](#functions--methods-basics)
5. [Common Interview Questions](#common-interview-questions)

---

## Introduction to OOP

### What is OOP?
**OOP (Object-Oriented Programming)** is a way of writing code that models real-world things as objects.

### Real-Life Example: Car
Think of a **Car**:
- **Properties**: color, model, speed
- **Actions**: start(), stop(), accelerate()

```javascript
// Create a car as an object
const myCar = {
  color: "red",
  model: "Honda",
  speed: 0,
  
  start: function() {
    console.log("Car started!");
  },
  
  accelerate: function() {
    this.speed += 10;
  }
};

myCar.start();      // Car started!
myCar.accelerate(); // Speed increased
```

### Why OOP in JavaScript?
- ✅ Organize complex code
- ✅ Reuse code easily
- ✅ Protect data
- ✅ Model real-world scenarios
- ✅ Scale applications
- ✅ Maintain code better
- ✅ Collaborate with teams

---

## Data Types & Memory Model

### Primitive Types
**Primitive types** are basic, immutable data types that store a single value directly. They are stored in the **Stack memory**.

#### 7 Primitive Types:
1. **String** - Text
2. **Number** - Numbers
3. **Boolean** - true/false
4. **Undefined** - No value assigned
5. **Null** - Intentionally empty
6. **Symbol** - Unique identifier
7. **BigInt** - Large numbers

```javascript
// 1. String
let name = "Ali";

// 2. Number
let age = 25;
let price = 99.99;

// 3. Boolean
let isStudent = true;
let hasJob = false;

// 4. Undefined
let address;  // undefined (not assigned)

// 5. Null
let salary = null;  // intentionally empty

// 6. Symbol (ES6)
let id = Symbol('unique');

// 7. BigInt (ES2020)
let bigNumber = 9007199254740991n;
```

### Reference (Non-Primitive) Types
**Non-primitive types** are complex data types that can store multiple values or collections. They are stored in the **Heap memory**, and variables hold a reference to them.

#### 3 Main Non-Primitive Types:
1. **Object** - Collection of key-value pairs
2. **Array** - Ordered list of values
3. **Function** - Executable code block

### Key Differences

| Feature | Primitive | Non-Primitive |
|---------|-----------|---------------|
| **Stores** | Single value | Multiple values |
| **Memory** | Stack | Heap |
| **Mutable** | ❌ Immutable | ✅ Mutable |
| **Comparison** | By value | By reference |
| **Copy** | Creates new copy | Copies reference |

---

### Pass-by-Value vs Pass-by-Reference

#### Pass-by-Value (Primitives)
When you pass a primitive value to a function, JavaScript creates a **copy** of that value. Changes inside the function don't affect the original variable.

```javascript
let x = 10;

function changeValue(num) {
  num = 20;  // Changes the copy, not original
  console.log("Inside function:", num);  // 20
}

changeValue(x);
console.log("Outside function:", x);  // 10 (unchanged!)
```

**Visual Explanation:**
```
Original: x = 10
          ↓
Pass to function → Creates COPY = 10
          ↓
Change copy to 20
          ↓
Original x is still 10 ✅
```

#### Pass-by-Reference (Non-Primitives)
When you pass a non-primitive value (object, array) to a function, JavaScript passes a **reference** (address) to the original. Changes inside the function **DO affect** the original.

```javascript
let person = { name: "Ali", age: 25 };

function changeObject(obj) {
  obj.age = 30;  // Changes the original!
  console.log("Inside function:", obj.age);  // 30
}

changeObject(person);
console.log("Outside function:", person.age);  // 30 (changed!)
```

**Visual Explanation:**
```
Original: person = { name: "Ali" }
                    ↓
Pass to function → Passes REFERENCE (address)
                    ↓
Both point to SAME object
                    ↓
Change affects original ⚠️
```

---

### Stack vs Heap Memory

#### Stack Memory (Primitive / Static Memory)
**Used for:**
- Primitive data types (string, number, boolean, null, undefined, symbol, bigint)
- Function calls (call stack frames)
- References / pointers to objects

**Characteristics:**
- ✅ Very fast
- ✅ Stores fixed-size values
- ✅ Memory is managed automatically (LIFO)
- ✅ Stored values are copied by value

**Example:**
```javascript
let a = 10;
let b = a; // copy value
a = 20;

console.log(a); // 20
console.log(b); // 10
```

**Stack Diagram:**
```
STACK
------
|  b = 10   |
|  a = 20   |
|  return   |
|  function |
------
```

#### Heap Memory
**Used for:**
- Objects
- Arrays
- Functions
- Non-primitive types

**Characteristics:**
- ⚠️ Slower than stack
- ✅ Stores dynamic / variable size data
- ✅ Accessed via references / pointers
- ✅ Non-primitives are copied by reference

**Example:**
```javascript
let obj1 = { name: "Ali" };
let obj2 = obj1;   // copy reference
obj1.name = "Zeeshan";

console.log(obj2.name); // "Zeeshan"
```

**Memory Diagram:**
```
STACK                     HEAP
--------                  ---------------
| obj1 | -----> --------> | {name: 'Zeeshan'} |
| obj2 | -----'           ---------------
--------
```

---

## Object Fundamentals

### Object Literals
An **object literal** is the curly-brace (`{}`) syntax used to create objects directly.

**Basic Usage:**
```javascript
const person = {
  name: "Ali",
  age: 25,
  greet: function() {
    console.log("Hello!");
  }
};

// Access properties
console.log(person.name);  // "Ali"
person.greet();            // "Hello!"
```

**ES6 Shorthand:**
```javascript
const name = "Ali";
const age = 25;

// Property shorthand
const person = { name, age };

// Method shorthand
const person = {
  name: "Ali",
  greet() {
    console.log("Hello!");
  }
};
```

---

### Dot Notation vs Bracket Notation

#### Dot Notation
Dot notation is simpler and more readable. Use it when:
- The property name is a valid identifier
- You know the property name ahead of time

```javascript
const person = { name: 'alice', age: 30 };
console.log(person.name); // 'alice'
```

#### Bracket Notation
Bracket notation is more flexible. Use it when:
- Property name is stored in a variable
- Property has special characters, spaces, or starts with a number
- Dynamically building property names

**Examples:**

1. **Using variables:**
```javascript
const person = { name: 'alice', age: 30 };
const prop = 'name';
console.log(person[prop]); // 'alice'
```

2. **Properties with special characters:**
```javascript
const person = { 'first name': 'alice', age: 30 };
console.log(person['first name']); // 'alice'
```

3. **Dynamic property names:**
```javascript
const property = 'name';
console.log(person[property]); // 'alice'
```

**When to Use Bracket Notation:**
- ✅ Property name is dynamic or stored in a variable
- ✅ Property name has spaces, special characters, or starts with a number

---

### Built-in Object Methods

#### 1. Object.keys(obj)
Returns an array of object's own enumerable property names.

```javascript
const user = { name: "Alice", age: 25 };
console.log(Object.keys(user)); // ["name", "age"]
```

#### 2. Object.values(obj)
Returns an array of object's own enumerable property values.

```javascript
const user = { name: "Alice", age: 25 };
console.log(Object.values(user)); // ["Alice", 25]
```

#### 3. Object.entries(obj)
Returns an array of [key, value] pairs.

```javascript
const user = { name: "Alice", age: 25 };
for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
// name: Alice
// age: 25
```

#### 4. Object.assign(target, ...sources)
Copies properties from source objects to target object.

```javascript
const user = { name: "Alice" };
const updates = { age: 25, city: "Seoul" };
const merged = Object.assign({}, user, updates);
console.log(merged); // { name: "Alice", age: 25, city: "Seoul" }
```

#### 5. Object.freeze(obj)
Makes an object immutable (cannot modify properties).

```javascript
const config = Object.freeze({ theme: "dark", version: 1 });
config.theme = "light"; // ignored
console.log(config.theme); // "dark"
```

#### 6. Object.hasOwn(obj, key)
Checks if an object has a specific property.

```javascript
const user = { name: "Alice" };
console.log(Object.hasOwn(user, "name")); // true
console.log(Object.hasOwn(user, "toString")); // false
```

**Quick Reference Table:**

| Method | Returns | Use Case |
|--------|---------|----------|
| `Object.keys()` | array | Loop over keys |
| `Object.values()` | array | Loop over values |
| `Object.entries()` | array of pairs | Converting object → array |
| `Object.assign()` | object | Shallow copy / merge |
| `Object.freeze()` | object | Make immutable |
| `Object.hasOwn()` | boolean | Check property existence |

---

## Functions & Methods Basics

### Functions as First-Class Citizens
In JavaScript, **functions are first-class citizens**, meaning they can be treated like any other value.

**This means you can:**
- ✅ Assign functions to variables
- ✅ Pass functions as arguments
- ✅ Return functions from other functions
- ✅ Store functions in arrays/objects
- ✅ Use functions as values anywhere

**Example:**
```javascript
// Assign to variable
const greet = function() {
  console.log("Hello!");
};

// Pass as argument
function callFunction(fn) {
  fn();
}
callFunction(greet); // "Hello!"

// Return from function
function createGreeter() {
  return function() {
    console.log("Hi!");
  };
}
```

---

### this Keyword (Basic Usage)
The `this` keyword refers to the object that is executing the current function.

**Basic Rules:**
1. **In a method**: `this` refers to the object
2. **In a function**: `this` refers to `window` (or `undefined` in strict mode)
3. **In an arrow function**: `this` is inherited from parent scope

**Examples:**

```javascript
// 1. In a method - this refers to the object
const user = {
  name: "Hamza",
  show() {
    console.log(this.name); // "Hamza"
  }
};
user.show();

// 2. In a regular function - this refers to window
function showName() {
  console.log(this); // window (or undefined in strict mode)
}

// 3. Arrow function - this is lexical (from parent)
const user = {
  name: "Zain",
  show: () => {
    console.log(this.name); // undefined (this is window)
  }
};
user.show();
```

**Common Mistake:**
```javascript
const person = {
  name: "Sara",
  hobbies: ["cricket", "coding"],
  showHobbies() {
    this.hobbies.forEach(function(hobby) {
      console.log(this.name, hobby); // undefined (this is window)
    });
  }
};

// Fix: Use arrow function
showHobbies() {
  this.hobbies.forEach((hobby) => {
    console.log(this.name, hobby); // Works! (this is person)
  });
}
```

---

### Method Definition in Objects
When a function is stored inside an object, it is called a **method**.

**Different Ways to Define Methods:**

1. **Function Expression:**
```javascript
const person = {
  name: "Ali",
  greet: function() {
    console.log("Hello!");
  }
};
person.greet(); // Hello!
```

2. **ES6 Method Shorthand:**
```javascript
const person = {
  name: "Ali",
  greet() {
    console.log("Hello from ES6!");
  }
};
person.greet(); // Hello from ES6!
```

3. **Using this in Methods:**
```javascript
const user = {
  name: "Zeeshan",
  showName() {
    console.log(this.name); // "Zeeshan"
  }
};
user.showName();
```

4. **Computed Method Names:**
```javascript
const methodName = "start";
const car = {
  [methodName]() {
    console.log("Car started");
  }
};
car.start(); // Car started
```

5. **Adding Methods Later:**
```javascript
const obj = {};
obj.sayHello = function() {
  console.log("Hello!");
};
obj.sayHello(); // Hello!
```

**⚠️ Arrow Functions as Methods:**
```javascript
const person = {
  name: "Ali",
  greet: () => {
    console.log(this.name); // undefined (arrow function has no own this)
  }
};
person.greet();
```

---

## Common Interview Questions

### 1. What is the difference between primitive and non-primitive types?

**Answer:**
- **Primitives**: Store single values directly in stack memory, are immutable, compared by value
- **Non-primitives**: Store references in stack, actual data in heap, are mutable, compared by reference

**Example:**
```javascript
// Primitive - copied by value
let a = 10;
let b = a;
a = 20;
console.log(b); // 10 (unchanged)

// Non-primitive - copied by reference
let obj1 = { x: 10 };
let obj2 = obj1;
obj1.x = 20;
console.log(obj2.x); // 20 (changed!)
```

---

### 2. Explain pass-by-value vs pass-by-reference.

**Answer:**
- **Pass-by-value**: Primitives create a copy, changes don't affect original
- **Pass-by-reference**: Non-primitives pass a reference, changes affect original

**Example:**
```javascript
// Pass-by-value
function changeNum(num) {
  num = 100;
}
let x = 10;
changeNum(x);
console.log(x); // 10 (unchanged)

// Pass-by-reference
function changeObj(obj) {
  obj.value = 100;
}
let myObj = { value: 10 };
changeObj(myObj);
console.log(myObj.value); // 100 (changed!)
```

---

### 3. What is the difference between dot notation and bracket notation?

**Answer:**
- **Dot notation**: Simple, readable, use when property name is known and valid identifier
- **Bracket notation**: Flexible, use for dynamic property names, special characters, or variables

**Example:**
```javascript
const person = { name: "Ali", "first name": "Ali" };

// Dot notation
console.log(person.name); // "Ali"

// Bracket notation - dynamic
const prop = "name";
console.log(person[prop]); // "Ali"

// Bracket notation - special characters
console.log(person["first name"]); // "Ali"
```

---

### 4. What are functions as first-class citizens?

**Answer:**
Functions in JavaScript can be:
- Assigned to variables
- Passed as arguments
- Returned from functions
- Stored in data structures

**Example:**
```javascript
// Assign to variable
const fn = function() { return "Hello"; };

// Pass as argument
function callMe(fn) { return fn(); }

// Return from function
function getFunction() {
  return function() { return "Hi"; };
}
```

---

### 5. Explain the `this` keyword in JavaScript.

**Answer:**
`this` refers to the object executing the current function:
- In methods: `this` = the object
- In functions: `this` = `window` (or `undefined` in strict mode)
- In arrow functions: `this` = lexical parent scope

**Example:**
```javascript
const obj = {
  name: "Ali",
  regular: function() {
    console.log(this.name); // "Ali"
  },
  arrow: () => {
    console.log(this.name); // undefined (this is window)
  }
};
```

---

### 6. What is the difference between Object.keys(), Object.values(), and Object.entries()?

**Answer:**
- `Object.keys()`: Returns array of property names
- `Object.values()`: Returns array of property values
- `Object.entries()`: Returns array of [key, value] pairs

**Example:**
```javascript
const user = { name: "Ali", age: 25 };

Object.keys(user);    // ["name", "age"]
Object.values(user);  // ["Ali", 25]
Object.entries(user); // [["name", "Ali"], ["age", 25]]
```

---

### 7. What is Object.freeze() and when would you use it?

**Answer:**
`Object.freeze()` makes an object immutable - properties cannot be added, removed, or modified.

**Use cases:**
- Configuration objects
- Constants
- Preventing accidental mutations

**Example:**
```javascript
const config = Object.freeze({ theme: "dark" });
config.theme = "light"; // Ignored
config.newProp = "value"; // Ignored
console.log(config.theme); // "dark"
```

---

### 8. Explain Stack vs Heap memory in JavaScript.

**Answer:**
- **Stack**: Fast, stores primitives and function calls, LIFO structure, fixed size
- **Heap**: Slower, stores objects/arrays/functions, dynamic size, accessed via references

**Example:**
```javascript
// Stack - primitives
let a = 10; // Stored in stack

// Heap - objects
let obj = { name: "Ali" }; // Reference in stack, object in heap
```

---

### 9. How do you check if an object has a property?

**Answer:**
Use `Object.hasOwn()` or `in` operator:

```javascript
const user = { name: "Ali" };

// Recommended
Object.hasOwn(user, "name"); // true

// Alternative
"name" in user; // true

// Note: in operator checks prototype chain too
"toString" in user; // true (inherited)
Object.hasOwn(user, "toString"); // false (own property only)
```

---

### 10. What is a method in JavaScript?

**Answer:**
A method is a function that is a property of an object.

**Example:**
```javascript
const person = {
  name: "Ali",
  // This is a method
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

person.greet(); // "Hello, I'm Ali"
```

---

## Practice Exercises

### Exercise 1: Create an Object
Create a `book` object with properties: `title`, `author`, `year`, and a method `getInfo()` that returns a string with all book information.

<details>
<summary>Solution</summary>

```javascript
const book = {
  title: "JavaScript Guide",
  author: "John Doe",
  year: 2023,
  getInfo() {
    return `${this.title} by ${this.author}, published in ${this.year}`;
  }
};

console.log(book.getInfo());
```
</details>

---

### Exercise 2: Object Manipulation
Given an object, write code to:
1. Add a new property
2. Update an existing property
3. Delete a property
4. Check if a property exists

<details>
<summary>Solution</summary>

```javascript
const person = { name: "Ali", age: 25 };

// Add property
person.city = "New York";

// Update property
person.age = 26;

// Delete property
delete person.city;

// Check if exists
console.log(Object.hasOwn(person, "name")); // true
```
</details>

---

### Exercise 3: Object Methods
Use `Object.keys()`, `Object.values()`, and `Object.entries()` to iterate over an object and log all key-value pairs.

<details>
<summary>Solution</summary>

```javascript
const user = { name: "Ali", age: 25, city: "NYC" };

// Using Object.entries()
for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}

// Using Object.keys()
Object.keys(user).forEach(key => {
  console.log(`${key}: ${user[key]}`);
});
```
</details>

---

## Key Takeaways

✅ **Primitives** are stored in stack, copied by value, immutable
✅ **Non-primitives** are stored in heap, copied by reference, mutable
✅ **Dot notation** for known properties, **bracket notation** for dynamic
✅ **Functions are first-class** - can be assigned, passed, returned
✅ **`this`** refers to the object calling the method
✅ **Object methods** help manipulate and iterate over objects
✅ **Stack** is fast for primitives, **Heap** is flexible for objects

---

## Study Tips

1. **Practice with examples** - Write code for each concept
2. **Understand memory model** - Visualize stack vs heap
3. **Master `this` keyword** - It's a common interview topic
4. **Know built-in methods** - Object.keys(), values(), entries() are frequently asked
5. **Practice exercises** - Try solving problems without looking at solutions

---

**Good luck with your interview preparation! 🚀**


# OOP in JavaScript - Beginner Level Interview Preparation Guide

## 📚 Table of Contents
1. [Introduction to OOP](#introduction-to-oop)
2. [Data Types & Memory Model](#data-types--memory-model)
3. [Object Fundamentals](#object-fundamentals)
4. [Functions & Methods Basics](#functions--methods-basics)
5. [Common Interview Questions](#common-interview-questions)
6. [Four Pillars of OOP](#four-pillars-of-oop)
7. [Constructor Functions (ES5 OOP)](#constructor-functions-es5-oop)
8. [Prototype Property](#prototype-property)
9. [Prototype Chain](#prototype-chain)
10. [__proto__](#__proto__)
11. [Working of new keyword](#working-of-new-keyword)
12. [Factory Functions](#factory-functions)
13. [Closures for Private Variables](#closures-for-private-variables)
14. [call(), apply(), bind()](#call-apply-bind)
15. [this keyword (Full Rules)](#this-keyword-full-rules)
16. [Arrow Functions & Lexical this](#arrow-functions--lexical-this)
17. [Object Cloning (Shallow)](#object-cloning-shallow)
18. [Object Cloning (Deep)](#object-cloning-deep)
19. [JSON-based clone vs structuredClone](#json-based-clone-vs-structuredclone)


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
# OOP in JavaScript - Advanced Deep Dive Guide


---

## Four Pillars of OOP

Object-Oriented Programming is built on four fundamental principles that help create maintainable, scalable, and robust code.

---

### 1. Encapsulation

**Definition:** Encapsulation is the bundling of data (properties) and methods (functions) that operate on that data into a single unit (object), while hiding internal implementation details from the outside world.

**Real-World Analogy (Non-Tech):**
Think of a **Coffee Machine**:
- **Public Interface**: You can press buttons (start, stop, select coffee type) - these are the public methods
- **Internal Details**: You don't need to know about the heating element, water pump, or internal wiring - these are private
- **Data Protection**: The machine protects its internal state (water temperature, pressure) from being directly modified

**Technical Example:**

```javascript
// Without Encapsulation (Bad)
const bankAccount = {
  balance: 1000
};

// Anyone can directly modify the balance
bankAccount.balance = -500; // ❌ No validation!
console.log(bankAccount.balance); // -500 (invalid state)

// With Encapsulation (Good)
function createBankAccount(initialBalance) {
  // Private variable (encapsulated)
  let balance = initialBalance;
  
  // Public methods (interface)
  return {
    // Getter method
    getBalance() {
      return balance;
    },
    
    // Setter method with validation
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
        return `Deposited $${amount}. New balance: $${balance}`;
      }
      return "Invalid deposit amount";
    },
    
    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        return `Withdrew $${amount}. New balance: $${balance}`;
      }
      return "Insufficient funds or invalid amount";
    }
  };
}

const account = createBankAccount(1000);
console.log(account.balance); // undefined (private!)
console.log(account.getBalance()); // 1000
account.deposit(500); // ✅ Valid
account.withdraw(2000); // ❌ "Insufficient funds"
```

**Benefits:**
- ✅ **Data Protection**: Prevents unauthorized access/modification
- ✅ **Validation**: Ensures data integrity through controlled access
- ✅ **Maintainability**: Internal changes don't affect external code
- ✅ **Abstraction**: Users only see what they need

**ES6 Class Example with Private Fields:**

```javascript
class BankAccount {
  // Private field (ES2022)
  #balance;
  
  constructor(initialBalance) {
    this.#balance = initialBalance;
  }
  
  getBalance() {
    return this.#balance;
  }
  
  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return `Deposited $${amount}`;
    }
    throw new Error("Invalid amount");
  }
  
  withdraw(amount) {
    if (amount > 0 && amount <= this.#balance) {
      this.#balance -= amount;
      return `Withdrew $${amount}`;
    }
    throw new Error("Insufficient funds");
  }
}

const account = new BankAccount(1000);
console.log(account.#balance); // ❌ SyntaxError: Private field
console.log(account.getBalance()); // ✅ 1000
```

---

### 2. Abstraction

**Definition:** Abstraction is the concept of hiding complex implementation details and showing only the essential features to the user. It focuses on "what" an object does rather than "how" it does it.

**Real-World Analogy (Non-Tech):**
Think of **Driving a Car**:
- **What you know**: Press accelerator → car goes faster, press brake → car stops, turn steering wheel → car turns
- **What you don't need to know**: How the engine combustion works, how the transmission shifts gears, how the hydraulic brake system applies pressure
- **Complexity Hidden**: The car abstracts away all the mechanical complexity and gives you a simple interface

**Technical Example:**

```javascript
// Without Abstraction (Complex)
class DatabaseConnection {
  constructor(host, port, username, password) {
    this.host = host;
    this.port = port;
    this.username = username;
    this.password = password;
    this.connection = null;
    this.pool = null;
  }
  
  // Complex implementation exposed
  connect() {
    // Complex connection logic
    this.connection = new Connection({
      host: this.host,
      port: this.port,
      auth: {
        username: this.username,
        password: this.password
      }
    });
    this.connection.establish();
    this.connection.authenticate();
    this.pool = this.connection.createPool();
    this.pool.initialize();
    // ... 50 more lines of complex code
  }
  
  query(sql) {
    // Complex query execution
    const statement = this.pool.getStatement();
    statement.prepare(sql);
    statement.execute();
    const result = statement.fetch();
    statement.close();
    return result;
  }
}

// With Abstraction (Simple Interface)
class Database {
  constructor(config) {
    this.connection = new DatabaseConnection(
      config.host,
      config.port,
      config.username,
      config.password
    );
  }
  
  // Simple, abstracted interface
  connect() {
    this.connection.connect();
  }
  
  query(sql) {
    return this.connection.query(sql);
  }
  
  // High-level methods that hide complexity
  async getUserById(id) {
    return this.query(`SELECT * FROM users WHERE id = ${id}`);
  }
  
  async createUser(userData) {
    return this.query(`INSERT INTO users VALUES ...`);
  }
}

// Usage - Simple and clean!
const db = new Database({
  host: 'localhost',
  port: 5432,
  username: 'admin',
  password: 'secret'
});

db.connect();
const user = await db.getUserById(123); // Simple!
```

**Another Example - Media Player:**

```javascript
// Abstracted Media Player
class MediaPlayer {
  constructor(file) {
    this.file = file;
    this.decoder = this.initializeDecoder(file);
    this.audioContext = this.setupAudioContext();
    this.buffer = null;
  }
  
  // Private methods (implementation details hidden)
  initializeDecoder(file) {
    // Complex decoder initialization
    // Handles MP3, WAV, FLAC, etc.
    return new AudioDecoder(file);
  }
  
  setupAudioContext() {
    // Complex Web Audio API setup
    return new AudioContext();
  }
  
  // Public interface (what users need)
  async play() {
    await this.load();
    this.startPlayback();
  }
  
  pause() {
    this.pausePlayback();
  }
  
  stop() {
    this.stopPlayback();
  }
  
  setVolume(level) {
    this.adjustVolume(level);
  }
}

// User only needs to know these simple methods
const player = new MediaPlayer('song.mp3');
player.play();    // ✅ Simple!
player.pause();   // ✅ Simple!
player.setVolume(0.8); // ✅ Simple!
// User doesn't need to know about decoders, audio contexts, buffers, etc.
```

**Benefits:**
- ✅ **Simplified Interface**: Users work with high-level concepts
- ✅ **Reduced Complexity**: Hide implementation details
- ✅ **Flexibility**: Can change internal implementation without affecting users
- ✅ **Focus on Essentials**: Users focus on "what" not "how"

---

### 3. Inheritance

**Definition:** Inheritance is a mechanism where a new class (child/subclass) can acquire properties and methods from an existing class (parent/superclass). It promotes code reusability and establishes an "is-a" relationship.

**Real-World Analogy (Non-Tech):**
Think of **Animals**:
- **Base Class**: `Animal` (has: name, age, eat(), sleep())
- **Derived Classes**: 
  - `Dog` is-an `Animal` (inherits: name, age, eat(), sleep() + adds: bark())
  - `Cat` is-an `Animal` (inherits: name, age, eat(), sleep() + adds: meow())
  - `Bird` is-an `Animal` (inherits: name, age, eat(), sleep() + adds: fly())

**Technical Example:**

```javascript
// Base Class (Parent)
class Animal {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  eat() {
    return `${this.name} is eating.`;
  }
  
  sleep() {
    return `${this.name} is sleeping.`;
  }
  
  makeSound() {
    return `${this.name} makes a sound.`;
  }
}

// Derived Class (Child) - Dog inherits from Animal
class Dog extends Animal {
  constructor(name, age, breed) {
    super(name, age); // Call parent constructor
    this.breed = breed;
  }
  
  // Override parent method
  makeSound() {
    return `${this.name} barks: Woof! Woof!`;
  }
  
  // New method specific to Dog
  fetch() {
    return `${this.name} is fetching the ball!`;
  }
}

// Derived Class (Child) - Cat inherits from Animal
class Cat extends Animal {
  constructor(name, age, color) {
    super(name, age);
    this.color = color;
  }
  
  makeSound() {
    return `${this.name} meows: Meow! Meow!`;
  }
  
  // New method specific to Cat
  climb() {
    return `${this.name} is climbing the tree!`;
  }
}

// Usage
const dog = new Dog("Buddy", 3, "Golden Retriever");
console.log(dog.eat());        // "Buddy is eating." (inherited)
console.log(dog.sleep());      // "Buddy is sleeping." (inherited)
console.log(dog.makeSound());  // "Buddy barks: Woof! Woof!" (overridden)
console.log(dog.fetch());      // "Buddy is fetching the ball!" (new method)

const cat = new Cat("Whiskers", 2, "Orange");
console.log(cat.eat());        // "Whiskers is eating." (inherited)
console.log(cat.makeSound());  // "Whiskers meows: Meow! Meow!" (overridden)
console.log(cat.climb());      // "Whiskers is climbing the tree!" (new method)
```

**ES5 Inheritance (Prototype-based):**

```javascript
// Parent Constructor
function Animal(name, age) {
  this.name = name;
  this.age = age;
}

Animal.prototype.eat = function() {
  return `${this.name} is eating.`;
};

Animal.prototype.sleep = function() {
  return `${this.name} is sleeping.`;
};

// Child Constructor
function Dog(name, age, breed) {
  Animal.call(this, name, age); // Call parent constructor
  this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Override method
Dog.prototype.makeSound = function() {
  return `${this.name} barks: Woof!`;
};

// Add new method
Dog.prototype.fetch = function() {
  return `${this.name} is fetching!`;
};

const dog = new Dog("Buddy", 3, "Golden Retriever");
console.log(dog.eat()); // Inherited from Animal
console.log(dog.fetch()); // Own method
```

**Real-World Tech Example - E-commerce:**

```javascript
// Base Product Class
class Product {
  constructor(name, price, sku) {
    this.name = name;
    this.price = price;
    this.sku = sku;
  }
  
  calculateTax() {
    return this.price * 0.1; // 10% tax
  }
  
  getTotalPrice() {
    return this.price + this.calculateTax();
  }
  
  displayInfo() {
    return `${this.name} - $${this.price}`;
  }
}

// Digital Product (inherits from Product)
class DigitalProduct extends Product {
  constructor(name, price, sku, downloadLink) {
    super(name, price, sku);
    this.downloadLink = downloadLink;
  }
  
  // Override - digital products have no tax
  calculateTax() {
    return 0;
  }
  
  // New method
  download() {
    return `Downloading ${this.name} from ${this.downloadLink}`;
  }
}

// Physical Product (inherits from Product)
class PhysicalProduct extends Product {
  constructor(name, price, sku, weight, dimensions) {
    super(name, price, sku);
    this.weight = weight;
    this.dimensions = dimensions;
  }
  
  // Override - physical products have shipping
  getTotalPrice() {
    return super.getTotalPrice() + this.calculateShipping();
  }
  
  // New method
  calculateShipping() {
    return this.weight * 0.5; // $0.5 per kg
  }
}

// Usage
const ebook = new DigitalProduct("JavaScript Guide", 29.99, "EBOOK-001", "https://...");
console.log(ebook.getTotalPrice()); // 29.99 (no tax, no shipping)
console.log(ebook.download()); // "Downloading JavaScript Guide..."

const book = new PhysicalProduct("JavaScript Book", 39.99, "BOOK-001", 0.5, "20x15x2");
console.log(book.getTotalPrice()); // 39.99 + tax + shipping
```

**Benefits:**
- ✅ **Code Reusability**: Share common code between classes
- ✅ **DRY Principle**: Don't Repeat Yourself
- ✅ **Polymorphism**: Child classes can override parent methods
- ✅ **Hierarchical Organization**: Natural "is-a" relationships

---

### 4. Polymorphism

**Definition:** Polymorphism means "many forms". It allows objects of different types to be treated as objects of a common base type. The same method can behave differently based on the object that calls it.

**Real-World Analogy (Non-Tech):**
Think of **Shapes Drawing**:
- **Base Concept**: "Draw a shape"
- **Different Forms**:
  - Draw a Circle → draws a circle
  - Draw a Square → draws a square
  - Draw a Triangle → draws a triangle
- **Same Action, Different Behavior**: All shapes can be "drawn", but each draws differently

**Technical Example:**

```javascript
// Base Class
class Shape {
  constructor(name) {
    this.name = name;
  }
  
  // Method to be overridden (polymorphic)
  calculateArea() {
    return "Area calculation not defined";
  }
  
  // Method to be overridden
  draw() {
    return "Drawing shape...";
  }
}

// Different implementations of the same interface
class Circle extends Shape {
  constructor(radius) {
    super("Circle");
    this.radius = radius;
  }
  
  // Polymorphic method - different implementation
  calculateArea() {
    return Math.PI * this.radius * this.radius;
  }
  
  draw() {
    return `Drawing a circle with radius ${this.radius}`;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super("Rectangle");
    this.width = width;
    this.height = height;
  }
  
  // Polymorphic method - different implementation
  calculateArea() {
    return this.width * this.height;
  }
  
  draw() {
    return `Drawing a rectangle ${this.width}x${this.height}`;
  }
}

class Triangle extends Shape {
  constructor(base, height) {
    super("Triangle");
    this.base = base;
    this.height = height;
  }
  
  // Polymorphic method - different implementation
  calculateArea() {
    return (this.base * this.height) / 2;
  }
  
  draw() {
    return `Drawing a triangle with base ${this.base} and height ${this.height}`;
  }
}

// Polymorphism in action - same interface, different behaviors
const shapes = [
  new Circle(5),
  new Rectangle(4, 6),
  new Triangle(3, 4)
];

// Treat all shapes the same way
shapes.forEach(shape => {
  console.log(shape.name);              // Different names
  console.log(shape.calculateArea());   // Different calculations!
  console.log(shape.draw());            // Different drawings!
  console.log("---");
});

// Output:
// Circle
// 78.53981633974483
// Drawing a circle with radius 5
// ---
// Rectangle
// 24
// Drawing a rectangle 4x6
// ---
// Triangle
// 6
// Drawing a triangle with base 3 and height 4
```

**Real-World Tech Example - Payment Processing:**

```javascript
// Base Payment Class
class PaymentMethod {
  constructor(amount) {
    this.amount = amount;
  }
  
  processPayment() {
    throw new Error("processPayment must be implemented");
  }
  
  validatePayment() {
    throw new Error("validatePayment must be implemented");
  }
}

// Different payment implementations
class CreditCard extends PaymentMethod {
  constructor(amount, cardNumber, cvv) {
    super(amount);
    this.cardNumber = cardNumber;
    this.cvv = cvv;
  }
  
  processPayment() {
    this.validatePayment();
    return `Processing credit card payment of $${this.amount} for card ending in ${this.cardNumber.slice(-4)}`;
  }
  
  validatePayment() {
    if (this.cardNumber.length !== 16) {
      throw new Error("Invalid card number");
    }
    if (this.cvv.length !== 3) {
      throw new Error("Invalid CVV");
    }
  }
}

class PayPal extends PaymentMethod {
  constructor(amount, email) {
    super(amount);
    this.email = email;
  }
  
  processPayment() {
    this.validatePayment();
    return `Processing PayPal payment of $${this.amount} for ${this.email}`;
  }
  
  validatePayment() {
    if (!this.email.includes("@")) {
      throw new Error("Invalid email");
    }
  }
}

class Cryptocurrency extends PaymentMethod {
  constructor(amount, walletAddress) {
    super(amount);
    this.walletAddress = walletAddress;
  }
  
  processPayment() {
    this.validatePayment();
    return `Processing crypto payment of $${this.amount} to wallet ${this.walletAddress.slice(0, 8)}...`;
  }
  
  validatePayment() {
    if (this.walletAddress.length < 26) {
      throw new Error("Invalid wallet address");
    }
  }
}

// Polymorphic usage - same interface, different implementations
function checkout(paymentMethod) {
  // Doesn't care which payment method, just processes it
  return paymentMethod.processPayment();
}

// All work the same way!
const creditCard = new CreditCard(100, "1234567890123456", "123");
const paypal = new PayPal(100, "user@example.com");
const crypto = new Cryptocurrency(100, "1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa");

console.log(checkout(creditCard)); // "Processing credit card payment..."
console.log(checkout(paypal));     // "Processing PayPal payment..."
console.log(checkout(crypto));     // "Processing crypto payment..."
```

**Types of Polymorphism:**

1. **Compile-time Polymorphism (Method Overloading)** - Not directly supported in JavaScript
2. **Runtime Polymorphism (Method Overriding)** - Supported via inheritance

**Benefits:**
- ✅ **Flexibility**: Same interface, different implementations
- ✅ **Extensibility**: Easy to add new types without changing existing code
- ✅ **Maintainability**: Changes to one type don't affect others
- ✅ **Code Reusability**: Common interface for different behaviors

---

## Constructor Functions (ES5 OOP)

**Definition:** Constructor functions are special functions used to create and initialize objects. They are called with the `new` keyword and follow a naming convention (PascalCase).

**Real-World Analogy (Non-Tech):**
Think of a **Cookie Cutter**:
- The cookie cutter (constructor) defines the shape
- Each time you use it (call with `new`), you get a new cookie (object) with the same shape
- You can customize each cookie (object) with different ingredients (properties)

**Basic Syntax:**

```javascript
// Constructor Function (PascalCase convention)
function Person(name, age, city) {
  // Properties
  this.name = name;
  this.age = age;
  this.city = city;
  
  // Methods (added to each instance - inefficient!)
  this.greet = function() {
    return `Hello, I'm ${this.name} from ${this.city}`;
  };
}

// Create instances using 'new'
const person1 = new Person("Alice", 25, "New York");
const person2 = new Person("Bob", 30, "London");

console.log(person1.greet()); // "Hello, I'm Alice from New York"
console.log(person2.greet()); // "Hello, I'm Bob from London"
```

**Problem with Methods in Constructor:**

```javascript
function Person(name) {
  this.name = name;
  // ❌ BAD: Each instance gets its own copy of the function
  this.greet = function() {
    return `Hello, ${this.name}`;
  };
}

const p1 = new Person("Alice");
const p2 = new Person("Bob");

// Different function objects (wasteful!)
console.log(p1.greet === p2.greet); // false
```

**Solution: Use Prototype (Better Approach):**

```javascript
function Person(name, age) {
  // Properties in constructor
  this.name = name;
  this.age = age;
}

// Methods on prototype (shared across all instances)
Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

Person.prototype.getAge = function() {
  return `${this.name} is ${this.age} years old`;
};

const p1 = new Person("Alice", 25);
const p2 = new Person("Bob", 30);

// Same function object (efficient!)
console.log(p1.greet === p2.greet); // true ✅

console.log(p1.greet()); // "Hello, I'm Alice"
console.log(p2.greet()); // "Hello, I'm Bob"
```

**Real-World Tech Example - User Management:**

```javascript
// User Constructor
function User(username, email, role) {
  this.username = username;
  this.email = email;
  this.role = role;
  this.createdAt = new Date();
  this.isActive = true;
}

// Shared methods on prototype
User.prototype.login = function() {
  if (this.isActive) {
    return `${this.username} logged in successfully`;
  }
  return "Account is inactive";
};

User.prototype.logout = function() {
  return `${this.username} logged out`;
};

User.prototype.updateEmail = function(newEmail) {
  if (this.validateEmail(newEmail)) {
    this.email = newEmail;
    return "Email updated successfully";
  }
  return "Invalid email format";
};

User.prototype.validateEmail = function(email) {
  return email.includes("@") && email.includes(".");
};

User.prototype.getInfo = function() {
  return {
    username: this.username,
    email: this.email,
    role: this.role,
    createdAt: this.createdAt,
    isActive: this.isActive
  };
};

// Create users
const admin = new User("admin", "admin@example.com", "administrator");
const user1 = new User("john_doe", "john@example.com", "user");
const user2 = new User("jane_smith", "jane@example.com", "user");

console.log(admin.login()); // "admin logged in successfully"
console.log(user1.getInfo());
console.log(user2.updateEmail("jane.new@example.com"));
```

**Constructor vs Regular Function:**

```javascript
// Regular Function
function createPerson(name) {
  return { name: name };
}

// Constructor Function
function Person(name) {
  this.name = name;
}

// Usage difference
const p1 = createPerson("Alice"); // No 'new' needed
const p2 = new Person("Bob");      // 'new' required

// What happens without 'new'?
const p3 = Person("Charlie"); // ❌ BAD!
console.log(p3); // undefined
console.log(window.name); // "Charlie" (pollutes global scope!)
```

**Checking if called with 'new':**

```javascript
function Person(name) {
  // Safety check
  if (!(this instanceof Person)) {
    return new Person(name); // Auto-fix
  }
  
  this.name = name;
}

// Both work now
const p1 = new Person("Alice");
const p2 = Person("Bob"); // Auto-converts to 'new Person("Bob")'
```

**Key Points:**
- ✅ Use PascalCase for constructor names
- ✅ Always use `new` keyword
- ✅ Put methods on `prototype`, not in constructor
- ✅ `this` refers to the new instance
- ✅ Constructor returns the new object automatically

---

## Prototype Property

**Definition:** Every function in JavaScript has a `prototype` property. When you create a function, JavaScript automatically creates a `prototype` object for it. This is used when the function is used as a constructor.

**Important Concepts:**
1. **Function.prototype**: Property on the constructor function
2. **Object.__proto__**: Internal link from instance to prototype
3. **Prototype Chain**: How JavaScript looks up properties/methods

**Basic Example:**

```javascript
function Person(name) {
  this.name = name;
}

// Add to prototype
Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

Person.prototype.species = "Homo sapiens";

const person = new Person("Alice");

// Access prototype properties
console.log(person.greet()); // "Hello, Alice"
console.log(person.species); // "Homo sapiens"

// Check prototype
console.log(Person.prototype); // { greet: [Function], species: 'Homo sapiens' }
console.log(person.__proto__ === Person.prototype); // true
```

**Visual Representation:**

```
Person (Constructor Function)
    |
    | has property
    |
Person.prototype (Prototype Object)
    {
      greet: [Function],
      species: "Homo sapiens"
    }
    ^
    | __proto__ points to
    |
person (Instance)
    {
      name: "Alice"
    }
```

**Real-World Example - Product System:**

```javascript
function Product(name, price) {
  this.name = name;
  this.price = price;
  this.id = Math.random().toString(36).substr(2, 9);
}

// Shared methods on prototype
Product.prototype.calculateTax = function() {
  return this.price * 0.1; // 10% tax
};

Product.prototype.getTotalPrice = function() {
  return this.price + this.calculateTax();
};

Product.prototype.applyDiscount = function(percentage) {
  const discount = this.price * (percentage / 100);
  this.price -= discount;
  return `Applied ${percentage}% discount. New price: $${this.price.toFixed(2)}`;
};

Product.prototype.displayInfo = function() {
  return `${this.name}: $${this.price} (Tax: $${this.calculateTax().toFixed(2)})`;
};

// Create products
const laptop = new Product("Laptop", 999.99);
const phone = new Product("Phone", 699.99);

console.log(laptop.displayInfo()); // "Laptop: $999.99 (Tax: $100.00)"
console.log(phone.getTotalPrice()); // 769.989

// All instances share the same prototype methods
console.log(laptop.calculateTax === phone.calculateTax); // true ✅
```

**Adding Multiple Methods:**

```javascript
function Car(brand, model) {
  this.brand = brand;
  this.model = model;
  this.speed = 0;
}

// Method 1: Add individually
Car.prototype.start = function() {
  return `${this.brand} ${this.model} started`;
};

Car.prototype.accelerate = function() {
  this.speed += 10;
  return `Speed: ${this.speed} km/h`;
};

// Method 2: Replace entire prototype (careful!)
Car.prototype = {
  start() {
    return `${this.brand} ${this.model} started`;
  },
  accelerate() {
    this.speed += 10;
    return `Speed: ${this.speed} km/h`;
  },
  brake() {
    this.speed = Math.max(0, this.speed - 10);
    return `Speed: ${this.speed} km/h`;
  }
};

// Method 3: Extend existing prototype
Object.assign(Car.prototype, {
  honk() {
    return "Beep beep!";
  },
  getInfo() {
    return `${this.brand} ${this.model} at ${this.speed} km/h`;
  }
});
```

**Prototype Property vs Instance Properties:**

```javascript
function Person(name) {
  // Instance property (unique per object)
  this.name = name;
}

// Prototype property (shared)
Person.prototype.species = "Homo sapiens";

const p1 = new Person("Alice");
const p2 = new Person("Bob");

// Instance properties are different
console.log(p1.name); // "Alice"
console.log(p2.name); // "Bob"

// Prototype properties are shared
console.log(p1.species); // "Homo sapiens"
console.log(p2.species); // "Homo sapiens"
console.log(p1.species === p2.species); // true (same reference)

// But you can override per instance
p1.species = "Alien";
console.log(p1.species); // "Alien" (instance property)
console.log(p2.species); // "Homo sapiens" (prototype property)
console.log(Person.prototype.species); // "Homo sapiens" (unchanged)
```

**Key Points:**
- ✅ `prototype` is a property of the constructor function
- ✅ Used to share methods across all instances
- ✅ More memory efficient than instance methods
- ✅ Can be modified at runtime
- ✅ All instances share the same prototype object

---

## Prototype Chain

**Definition:** The prototype chain is the mechanism by which JavaScript looks up properties and methods. When a property is accessed, JavaScript first checks the object itself, then its prototype, then the prototype's prototype, and so on, until it finds the property or reaches `null`.

**How It Works:**

```
Object Instance
    |
    | Property lookup
    |
    ↓ (if not found)
    |
__proto__ → Constructor.prototype
    |
    | Property lookup
    |
    ↓ (if not found)
    |
__proto__ → Object.prototype
    |
    | Property lookup
    |
    ↓ (if not found)
    |
__proto__ → null (end of chain)
```

**Example:**

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.eat = function() {
  return `${this.name} is eating`;
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  return `${this.name} is barking`;
};

// Object.prototype methods are available
Object.prototype.toString = function() {
  return `[Object ${this.constructor.name}]`;
};

const dog = new Dog("Buddy", "Golden Retriever");

// Property lookup chain:
console.log(dog.breed);        // 1. Found on dog instance ✅
console.log(dog.name);         // 2. Found on dog instance ✅
console.log(dog.bark());       // 3. Found on Dog.prototype ✅
console.log(dog.eat());        // 4. Found on Animal.prototype ✅
console.log(dog.toString());   // 5. Found on Object.prototype ✅
console.log(dog.nonExistent);  // 6. Not found anywhere → undefined
```

**Visual Chain:**

```
dog (instance)
  {
    name: "Buddy",
    breed: "Golden Retriever"
  }
    |
    | __proto__
    ↓
Dog.prototype
  {
    bark: [Function],
    constructor: Dog
  }
    |
    | __proto__
    ↓
Animal.prototype
  {
    eat: [Function]
  }
    |
    | __proto__
    ↓
Object.prototype
  {
    toString: [Function],
    valueOf: [Function],
    hasOwnProperty: [Function],
    ...
  }
    |
    | __proto__
    ↓
null (end of chain)
```

**Real-World Example - Employee Hierarchy:**

```javascript
// Base class
function Employee(name, id) {
  this.name = name;
  this.id = id;
}

Employee.prototype.getInfo = function() {
  return `Employee: ${this.name} (ID: ${this.id})`;
};

Employee.prototype.work = function() {
  return `${this.name} is working`;
};

// Middle level
function Manager(name, id, department) {
  Employee.call(this, name, id);
  this.department = department;
}

Manager.prototype = Object.create(Employee.prototype);
Manager.prototype.constructor = Manager;

Manager.prototype.manage = function() {
  return `${this.name} is managing ${this.department}`;
};

// Top level
function CEO(name, id, company) {
  Manager.call(this, name, id, "All Departments");
  this.company = company;
}

CEO.prototype = Object.create(Manager.prototype);
CEO.prototype.constructor = CEO;

CEO.prototype.makeDecision = function() {
  return `${this.name} is making strategic decisions for ${this.company}`;
};

const ceo = new CEO("John Smith", "CEO001", "Tech Corp");

// Prototype chain lookup:
console.log(ceo.company);           // CEO instance
console.log(ceo.department);         // Manager instance
console.log(ceo.name);               // Employee instance
console.log(ceo.makeDecision());     // CEO.prototype
console.log(ceo.manage());           // Manager.prototype
console.log(ceo.work());             // Employee.prototype
console.log(ceo.getInfo());          // Employee.prototype
console.log(ceo.toString());         // Object.prototype
```

**hasOwnProperty() - Checking Own vs Inherited:**

```javascript
function Person(name) {
  this.name = name; // Own property
}

Person.prototype.species = "Homo sapiens"; // Inherited property

const person = new Person("Alice");

// Check own properties
console.log(person.hasOwnProperty("name"));     // true ✅
console.log(person.hasOwnProperty("species"));   // false (inherited)
console.log(person.hasOwnProperty("toString"));  // false (inherited)

// Check if property exists (own or inherited)
console.log("name" in person);     // true
console.log("species" in person);   // true
console.log("toString" in person); // true
```

**Modifying Prototype Chain:**

```javascript
function Animal() {}
Animal.prototype.eat = function() { return "Eating"; };

function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.bark = function() { return "Barking"; };

const dog = new Dog();

// Chain: dog → Dog.prototype → Animal.prototype → Object.prototype → null

// Add to Animal.prototype (affects all descendants)
Animal.prototype.sleep = function() { return "Sleeping"; };
console.log(dog.sleep()); // "Sleeping" ✅

// Add to Object.prototype (affects ALL objects - be careful!)
Object.prototype.walk = function() { return "Walking"; };
console.log(dog.walk()); // "Walking" ✅
console.log({}.walk());  // "Walking" ✅ (even plain objects!)
```

**Key Points:**
- ✅ JavaScript searches up the chain until property is found or `null` is reached
- ✅ Own properties take precedence over prototype properties
- ✅ Chain ends at `Object.prototype` which has `__proto__: null`
- ✅ Modifying prototypes affects all instances
- ✅ Use `hasOwnProperty()` to distinguish own vs inherited properties

---

## __proto__

**Definition:** `__proto__` is a property that points to the prototype object of the constructor function that was used to create the instance. It's the internal link that connects an object to its prototype in the prototype chain.

**Important Notes:**
- `__proto__` is a **getter/setter** for the internal `[[Prototype]]` property
- It's **deprecated** but still widely used and supported
- Modern alternative: `Object.getPrototypeOf()` and `Object.setPrototypeOf()`

**Basic Usage:**

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

const person = new Person("Alice");

// __proto__ points to Person.prototype
console.log(person.__proto__ === Person.prototype); // true ✅
console.log(person.__proto__); // { greet: [Function] }

// Access prototype through __proto__
console.log(person.__proto__.greet); // [Function: greet]
```

**Visual Representation:**

```javascript
function Animal() {}
Animal.prototype.eat = function() { return "Eating"; };

function Dog() {}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.bark = function() { return "Barking"; };

const dog = new Dog();

// Chain visualization
console.log(dog.__proto__ === Dog.prototype);           // true
console.log(dog.__proto__.__proto__ === Animal.prototype); // true
console.log(dog.__proto__.__proto__.__proto__ === Object.prototype); // true
console.log(dog.__proto__.__proto__.__proto__.__proto__ === null);   // true
```

**Setting __proto__ (Not Recommended):**

```javascript
const obj = { name: "Alice" };

// Setting __proto__ (deprecated, but works)
obj.__proto__ = {
  greet() {
    return `Hello, ${this.name}`;
  }
};

console.log(obj.greet()); // "Hello, Alice"

// Modern way (preferred)
const newProto = {
  greet() {
    return `Hello, ${this.name}`;
  }
};
Object.setPrototypeOf(obj, newProto);
```

**Modern Alternatives:**

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

const person = new Person("Alice");

// Instead of __proto__
console.log(person.__proto__);                    // Deprecated
console.log(Object.getPrototypeOf(person));       // Modern ✅
console.log(Object.getPrototypeOf(person) === Person.prototype); // true

// Setting prototype
Object.setPrototypeOf(person, { newMethod() { return "New"; } }); // Modern ✅
```

**Real-World Example:**

```javascript
function Vehicle(brand, model) {
  this.brand = brand;
  this.model = model;
}

Vehicle.prototype.start = function() {
  return `${this.brand} ${this.model} started`;
};

function Car(brand, model, doors) {
  Vehicle.call(this, brand, model);
  this.doors = doors;
}

Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;
Car.prototype.honk = function() {
  return "Beep beep!";
};

const car = new Car("Toyota", "Camry", 4);

// Using __proto__ to inspect chain
console.log("Level 1 (Car.prototype):", car.__proto__);
console.log("Level 2 (Vehicle.prototype):", car.__proto__.__proto__);
console.log("Level 3 (Object.prototype):", car.__proto__.__proto__.__proto__);
console.log("Level 4 (null):", car.__proto__.__proto__.__proto__.__proto__);

// Traverse the chain
let current = car;
let level = 0;
while (current !== null) {
  console.log(`Level ${level}:`, Object.getPrototypeOf(current) || null);
  current = Object.getPrototypeOf(current);
  level++;
}
```

**Checking Prototype Chain:**

```javascript
function A() {}
function B() {}
function C() {}

B.prototype = Object.create(A.prototype);
C.prototype = Object.create(B.prototype);

const c = new C();

// Check if A.prototype is in the chain
console.log(c instanceof A); // true
console.log(c instanceof B); // true
console.log(c instanceof C); // true

// Check prototype directly
console.log(A.prototype.isPrototypeOf(c)); // true
console.log(B.prototype.isPrototypeOf(c)); // true
console.log(C.prototype.isPrototypeOf(c)); // true
```

**Key Points:**
- ✅ `__proto__` is the link to the prototype object
- ✅ `obj.__proto__ === Constructor.prototype` (for constructor instances)
- ✅ Deprecated but still widely used
- ✅ Prefer `Object.getPrototypeOf()` and `Object.setPrototypeOf()`
- ✅ Chain ends at `null` (Object.prototype.__proto__ === null)

---

## Working of new keyword

**Definition:** The `new` keyword is used to create an instance of an object from a constructor function. It performs several important steps automatically.

**What `new` Does (Step by Step):**

1. **Creates a new empty object**
2. **Sets the object's `__proto__` to the constructor's `prototype`**
3. **Binds `this` to the new object**
4. **Executes the constructor function**
5. **Returns the new object** (unless constructor returns an object)

**Manual Implementation:**

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

// What 'new Person("Alice", 25)' does internally:

function myNew(constructor, ...args) {
  // Step 1: Create empty object
  const obj = {};
  
  // Step 2: Set __proto__ to constructor's prototype
  Object.setPrototypeOf(obj, constructor.prototype);
  // OR: obj.__proto__ = constructor.prototype;
  
  // Step 3 & 4: Bind 'this' and execute constructor
  const result = constructor.apply(obj, args);
  
  // Step 5: Return object (or result if it's an object)
  return result instanceof Object ? result : obj;
}

// Usage
const person1 = new Person("Alice", 25);
const person2 = myNew(Person, "Bob", 30);

console.log(person1.greet()); // "Hello, Alice"
console.log(person2.greet()); // "Hello, Bob"
```

**Step-by-Step Breakdown:**

```javascript
function Car(brand, model) {
  console.log("Step 4: Constructor executing");
  console.log("Step 3: 'this' is:", this);
  this.brand = brand;
  this.model = model;
  // If we return an object, that will be used instead
  // return { custom: "object" }; // Would override the new object
}

Car.prototype.start = function() {
  return `${this.brand} ${this.model} started`;
};

// When we do: new Car("Toyota", "Camry")

// Step 1: JavaScript creates {}
const newCar = {};

// Step 2: Sets prototype
newCar.__proto__ = Car.prototype;

// Step 3: Binds 'this' to newCar
// Step 4: Executes Car.call(newCar, "Toyota", "Camry")
Car.call(newCar, "Toyota", "Camry");

// Step 5: Returns newCar
const car = newCar;

console.log(car.start()); // "Toyota Camry started"
```

**What Happens Without `new`:**

```javascript
function Person(name) {
  this.name = name;
}

// Without 'new' - BAD!
const p1 = Person("Alice");
console.log(p1); // undefined ❌
console.log(window.name); // "Alice" (polluted global scope!) ❌

// With 'new' - GOOD!
const p2 = new Person("Bob");
console.log(p2); // Person { name: "Bob" } ✅
```

**Constructor Returning Values:**

```javascript
function Person(name) {
  this.name = name;
  
  // If you return a primitive, it's ignored
  return "ignored";
  
  // If you return an object, that object is used instead
  // return { custom: "object" }; // This would replace the new instance
}

const person = new Person("Alice");
console.log(person); // Person { name: "Alice" } (primitive return ignored)

function CustomPerson(name) {
  this.name = name;
  return { custom: "I'm custom!" }; // Object return overrides
}

const custom = new CustomPerson("Bob");
console.log(custom); // { custom: "I'm custom!" } (not a Person instance!)
```

**Real-World Example - Building a Library System:**

```javascript
function Book(title, author, isbn) {
  // 'this' refers to the new object being created
  this.title = title;
  this.author = author;
  this.isbn = isbn;
  this.isBorrowed = false;
  this.borrower = null;
}

Book.prototype.borrow = function(borrowerName) {
  if (this.isBorrowed) {
    return `Sorry, ${this.title} is already borrowed by ${this.borrower}`;
  }
  this.isBorrowed = true;
  this.borrower = borrowerName;
  return `${borrowerName} borrowed ${this.title}`;
};

Book.prototype.return = function() {
  if (!this.isBorrowed) {
    return `${this.title} is not currently borrowed`;
  }
  const borrower = this.borrower;
  this.isBorrowed = false;
  this.borrower = null;
  return `${borrower} returned ${this.title}`;
};

Book.prototype.getInfo = function() {
  return `${this.title} by ${this.author} (ISBN: ${this.isbn}) - ${
    this.isBorrowed ? `Borrowed by ${this.borrower}` : "Available"
  }`;
};

// 'new' creates instances
const book1 = new Book("1984", "George Orwell", "978-0-452-28423-4");
const book2 = new Book("To Kill a Mockingbird", "Harper Lee", "978-0-06-112008-4");

console.log(book1.getInfo()); // "1984 by George Orwell... Available"
book1.borrow("Alice");
console.log(book1.getInfo()); // "1984 by George Orwell... Borrowed by Alice"
```

**Visual Representation of `new`:**

```
new Person("Alice", 25)

Step 1: {} (empty object created)
        |
Step 2: { __proto__: Person.prototype }
        |
Step 3: Person.call({ __proto__: Person.prototype }, "Alice", 25)
        |
        this.name = "Alice"  →  { name: "Alice", __proto__: Person.prototype }
        this.age = 25        →  { name: "Alice", age: 25, __proto__: Person.prototype }
        |
Step 4: Return the object
        |
Result: Person { name: "Alice", age: 25 }
```

**Key Points:**
- ✅ `new` creates a new object
- ✅ Sets prototype link automatically
- ✅ Binds `this` to the new object
- ✅ Returns the new object (unless constructor returns an object)
- ✅ Always use `new` with constructor functions
- ✅ Without `new`, `this` refers to global object (or `undefined` in strict mode)

---

## Factory Functions

**Definition:** Factory functions are functions that return objects. They don't use the `new` keyword and don't rely on `this` or prototypes. They're an alternative to constructor functions.

**Real-World Analogy (Non-Tech):**
Think of a **Pizza Factory**:
- You call the factory with your order (parameters)
- The factory creates a pizza (object) according to your specifications
- Each pizza is independent and self-contained
- No need for a "Pizza constructor" - just a function that makes pizzas

**Basic Example:**

```javascript
// Factory Function
function createPerson(name, age, city) {
  return {
    name: name,
    age: age,
    city: city,
    greet() {
      return `Hello, I'm ${this.name} from ${city}`;
    },
    getInfo() {
      return `${this.name}, ${this.age} years old, from ${city}`;
    }
  };
}

// Usage (no 'new' needed!)
const person1 = createPerson("Alice", 25, "New York");
const person2 = createPerson("Bob", 30, "London");

console.log(person1.greet()); // "Hello, I'm Alice from New York"
console.log(person2.getInfo()); // "Bob, 30 years old, from London"
```

**ES6 Shorthand:**

```javascript
function createPerson(name, age, city) {
  return {
    name,  // Shorthand for name: name
    age,   // Shorthand for age: age
    city,  // Shorthand for city: city
    greet() {
      return `Hello, I'm ${this.name} from ${this.city}`;
    }
  };
}
```

**Factory Functions vs Constructor Functions:**

```javascript
// Constructor Function
function PersonConstructor(name) {
  this.name = name;
}
PersonConstructor.prototype.greet = function() {
  return `Hello, ${this.name}`;
};
const p1 = new PersonConstructor("Alice");

// Factory Function
function createPerson(name) {
  return {
    name: name,
    greet() {
      return `Hello, ${this.name}`;
    }
  };
}
const p2 = createPerson("Bob");

// Differences:
console.log(p1 instanceof PersonConstructor); // true
console.log(p2 instanceof createPerson);      // false (not a constructor)

console.log(p1.constructor); // PersonConstructor
console.log(p2.constructor); // Object (default)
```

**Real-World Tech Example - User Management:**

```javascript
function createUser(username, email, role = "user") {
  // Private variables (closure)
  let isActive = true;
  let loginCount = 0;
  
  return {
    // Public properties
    username,
    email,
    role,
    
    // Public methods
    login() {
      if (isActive) {
        loginCount++;
        return `${username} logged in (${loginCount} times)`;
      }
      return "Account is inactive";
    },
    
    logout() {
      return `${username} logged out`;
    },
    
    deactivate() {
      isActive = false;
      return `${username} account deactivated`;
    },
    
    activate() {
      isActive = true;
      return `${username} account activated`;
    },
    
    getStats() {
      return {
        username,
        email,
        role,
        loginCount,
        isActive
      };
    }
  };
}

const admin = createUser("admin", "admin@example.com", "administrator");
const user = createUser("john_doe", "john@example.com");

console.log(admin.login()); // "admin logged in (1 times)"
console.log(admin.login()); // "admin logged in (2 times)"
console.log(admin.getStats()); // { username: "admin", loginCount: 2, ... }
```

**Factory Functions with Inheritance:**

```javascript
// Base factory
function createAnimal(name, species) {
  return {
    name,
    species,
    eat() {
      return `${this.name} is eating`;
    },
    sleep() {
      return `${this.name} is sleeping`;
    
## Study Tips

1. **Practice with examples** - Write code for each concept
2. **Understand memory model** - Visualize stack vs heap
3. **Master `this` keyword** - It's a common interview topic
4. **Know built-in methods** - Object.keys(), values(), entries() are frequently asked
5. **Practice exercises** - Try solving problems without looking at solutions

---

**Good luck with your interview preparation! 🚀**


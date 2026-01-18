# **Lecture 2: OOPS (Abstraction & Encapsulation)**

### *(Real-World Systems: Payment & Ride-Hailing)*

---

## **1. Why Did We Move Beyond Procedural Programming?**

### **1.1 Early Languages**

#### **1. Machine Language (Binary)**

* Direct CPU instructions in `0s` and `1s`.

**Drawbacks**

* Extremely error-prone (one bit error = crash).
* Impossible to reason about business logic.
* No abstraction — every hardware detail is manual.

👉 **Unusable for systems like Uber or Stripe**

---

#### **2. Assembly Language**

* Introduced mnemonics (`MOV`, `ADD`, `JMP`)
* Still tied to CPU architecture.

**Limitations**

* Hardware-dependent
* No domain concepts (User, Ride, Payment)
* Impossible to scale for distributed systems

---

### **1.2 Procedural (Structured) Programming**

#### **Features Introduced**

* Functions
* Control structures (`if`, `for`, `while`)
* Modular files

#### **Advantages**

* Better readability
* Reusable logic blocks
* Suitable for small programs

#### **Limitations (Real-World Context)**

* ❌ Poor domain modeling

  * Hard to represent **Users, Drivers, Rides, Payments**
* ❌ No data protection

  * Any function can modify shared state
* ❌ Weak scalability

  * Large systems become tightly coupled
* ❌ No enforceable contracts

  * Payment logic can be misused accidentally

👉 **Procedural code breaks down in systems like Uber or online payments**

---

## **2. Entering Object-Oriented Programming (OOP)**

### **Core Idea**

Model software as **interacting objects** that reflect **real-world entities**.

### **Benefits**

* Natural domain modeling
  (`User`, `Driver`, `Ride`, `Payment`)
* Data protection (encapsulation)
* Loose coupling (abstraction)
* Extensibility (new payment providers, new ride types)

---

## **3. Modeling Real-World Entities in Code**

### **3.1 Objects, Classes & Instances**

* **Object**
  A real-world entity with state and behavior

  * Example: *A specific ride from Lahore to DHA*

* **Class**
  Blueprint defining structure and behavior

  * Example: `Ride`, `Payment`, `Driver`

* **Instance**
  A concrete object in memory

  * Example: `ride#9823`

---

## **4. Deep Dive: Pillar 1 – Abstraction**

### **Definition**

**Abstraction hides unnecessary implementation details and exposes only what is essential to use an object’s functionality.**

---

### **4.1 Real-World Analogies (Modern Systems)**

#### **Payment System (Stripe / PayPal)**

**What you do (Client / App)**

* Call `charge(amount, currency)`
* Call `refund(transactionId)`

**What you don’t need to know**

* Encryption & tokenization
* PCI compliance
* Bank network communication
* Retry logic & fraud checks

**Abstraction in Action**

* A **PaymentGateway API** hides all complexity
* App only interacts with a simple contract

---

#### **Ride-Hailing App (Uber-like)**

**What the app does**

* Request a ride
* Assign driver
* Start trip
* Complete trip

**What the app doesn’t know**

* Driver matching algorithm
* Pricing rules
* GPS triangulation
* Traffic optimization

**Abstraction in Action**

* RideService exposes clean methods
* Internals evolve without breaking clients

---

### **4.2 Language-Level Abstraction**

#### **Control Structures as Abstraction**

* `if`, `for`, `while` express intent
* Compiler handles:

  * CPU jumps
  * Stack frames
  * Registers

👉 Developers think in **business logic**, not machine instructions

---

## **5. Code-Based Abstraction: Abstract Classes & Interfaces**

### **5.1 Abstract Class Example (C++) – Payment System**

```cpp
// Abstract interface for any Payment Provider
class PaymentGateway {
public:
    virtual bool authenticate(const std::string& apiKey) = 0;
    virtual bool charge(double amount, const std::string& currency) = 0;
    virtual bool refund(const std::string& transactionId) = 0;
    virtual ~PaymentGateway() {}
};
```

#### **Key Points**

* Declares **WHAT operations exist**
* No implementation details
* Clients depend on `PaymentGateway`, not Stripe/PayPal
* Enables vendor independence

---

### **5.2 Concrete Subclass Example**

```cpp
class StripePayment : public PaymentGateway {
public:
    bool authenticate(const std::string&) override { return true; }
    bool charge(double, const std::string&) override { return true; }
    bool refund(const std::string&) override { return true; }
};
```

Another provider:

```cpp
class PayPalPayment : public PaymentGateway {
public:
    bool authenticate(const std::string&) override { return true; }
    bool charge(double, const std::string&) override { return true; }
    bool refund(const std::string&) override { return true; }
};
```

👉 **Business logic remains unchanged**

---

## **6. Benefits of Abstraction (Real-World)**

### **1. Simplified Interfaces**

* App calls `charge()` — nothing more

### **2. Ease of Maintenance**

* Stripe → PayPal switch requires **no business code changes**

### **3. Code Reuse**

* Same interface for:

  * Stripe
  * PayPal
  * JazzCash
  * Easypaisa

### **4. Reduced Complexity**

* Clear separation of concerns
* Scalable architecture

---
```cpp
#include <iostream>
#include <string>

using namespace std;

/*
Abstract class (Abstraction Layer)

1. Acts as a PUBLIC API for payment operations.
2. Defines WHAT operations are available, not HOW they are implemented.
3. Cannot be instantiated directly.
4. Concrete payment providers (Stripe, PayPal, etc.) must implement this contract.

Real-world meaning:
- Your application only knows that it can authenticate, charge, and refund.
- It does NOT know how encryption, HTTP calls, retries, or bank integrations work.
*/
class PaymentGateway {
public:
    virtual bool authenticate(const string& apiKey) = 0;
    virtual bool charge(double amount, const string& currency) = 0;
    virtual bool refund(const string& transactionId) = 0;
    virtual ~PaymentGateway() {}
};

/*
Concrete class (Implementation)

1. Provides Stripe-specific payment logic.
2. All Stripe API details are hidden inside this class.
3. Can be replaced with another provider without changing business logic.

Real-world meaning:
- Stripe SDK / API implementation lives here.
- Application code never depends on Stripe directly.
*/
class StripePayment : public PaymentGateway {
private:
    bool isAuthenticated;

public:
    StripePayment() {
        isAuthenticated = false;
    }

    bool authenticate(const string& apiKey) override {
        // Hidden Stripe authentication logic
        isAuthenticated = true;
        cout << "[Stripe] Authenticated using API Key." << endl;
        return true;
    }

    bool charge(double amount, const string& currency) override {
        if (!isAuthenticated) {
            cout << "[Stripe] Authentication required before charging." << endl;
            return false;
        }
        cout << "[Stripe] Charging " << amount << " " << currency << endl;
        return true;
    }

    bool refund(const string& transactionId) override {
        cout << "[Stripe] Refunding transaction ID: " << transactionId << endl;
        return true;
    }
};

/*
Another concrete class (Alternative Provider)

Demonstrates how easily we can switch vendors.
No changes required in business logic.
*/
class PayPalPayment : public PaymentGateway {
private:
    bool sessionActive;

public:
    PayPalPayment() {
        sessionActive = false;
    }

    bool authenticate(const string& apiKey) override {
        sessionActive = true;
        cout << "[PayPal] Session initialized." << endl;
        return true;
    }

    bool charge(double amount, const string& currency) override {
        if (!sessionActive) {
            cout << "[PayPal] Session not active." << endl;
            return false;
        }
        cout << "[PayPal] Processing payment of "
             << amount << " " << currency << endl;
        return true;
    }

    bool refund(const string& transactionId) override {
        cout << "[PayPal] Reversing transaction ID: " << transactionId << endl;
        return true;
    }
};

/*
Business Service Layer

Depends ONLY on the abstraction (PaymentGateway),
not on Stripe or PayPal.
*/
class OrderService {
private:
    PaymentGateway* paymentGateway;

public:
    OrderService(PaymentGateway* gateway) {
        paymentGateway = gateway;
    }

    void placeOrder(double amount) {
        paymentGateway->authenticate("SECRET_API_KEY");
        paymentGateway->charge(amount, "USD");
    }
};

/*
Main Method
*/
int main() {

    // Easily switch payment providers
    PaymentGateway* gateway = new StripePayment();
    // PaymentGateway* gateway = new PayPalPayment();

    OrderService orderService(gateway);
    orderService.placeOrder(499.99);

    delete gateway;
    return 0;
}
```__ 
## **7. Deep Dive: Pillar 2 – Encapsulation**

### **Definition**

**Encapsulation bundles data and behavior together and restricts direct access to internal state.**

---

### **7.1 Two Facets of Encapsulation**

#### **1. Logical Grouping**

* Related data + methods in one class

**Example**

* `Ride` class contains:

  * `status`
  * `startTrip()`
  * `endTrip()`

---

#### **2. Data Security**

* Prevent invalid state changes

**Example**

* Payment amount cannot be modified after charge
* Ride status cannot jump from `REQUESTED` → `COMPLETED`

---

### **7.2 Real-World Analogies**

#### **Payment Transaction**

* You can **view status**
* You cannot directly alter transaction amount

#### **Uber Ride**

* You can see trip progress
* You cannot manually set fare or distance

---

### **7.3 Access Modifiers in C++**

* **public** → API surface (safe operations)
* **private** → Internal state (protected)
* **protected** → Extension points for subclasses

---

### **7.4 Getters & Setters with Validation**

```cpp
class Payment {
private:
    double amount;

public:
    void setAmount(double value) {
        if (value <= 0) throw std::invalid_argument("Invalid amount");
        amount = value;
    }

    double getAmount() const {
        return amount;
    }
};
```

---

## **7.5 Encapsulation Benefits**

1. **Robustness** – prevents invalid state
2. **Maintainability** – internals can change safely
3. **Clear Contracts** – public APIs define usage
4. **Modularity** – easier testing & reuse

---

## **Final Professional Summary**

> Abstraction defines *what a system can do*.
> Encapsulation protects *how it does it*.
> Together, they form the foundation of scalable, maintainable, real-world software systems.


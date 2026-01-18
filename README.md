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
```
__ 
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
```cpp
#include <iostream>
#include <string>

using namespace std;

/*
Encapsulation says two important things:

1. An object's data (state) and behavior (methods) are bundled together
   inside a single unit (class).
2. Direct access to sensitive data is restricted to ensure data security.

In real-world systems (e.g., Payment Systems):
- You can view payment status.
- You cannot directly modify payment amount or transaction state.
- All changes must go through controlled methods.
*/
class Payment {
private:
    // Sensitive data (NOT directly accessible)
    string transactionId;
    double amount;
    bool isPaid;
    string paymentProvider;

public:
    // Constructor initializes payment safely
    Payment(string txnId, double amt, string provider) {
        transactionId = txnId;
        amount = amt;
        paymentProvider = provider;
        isPaid = false;
    }

    /*
    Getter methods
    - Allow read-only access
    - Prevent direct manipulation
    */
    double getAmount() const {
        return amount;
    }

    string getTransactionId() const {
        return transactionId;
    }

    bool getPaymentStatus() const {
        return isPaid;
    }

    string getPaymentProvider() const {
        return paymentProvider;
    }

    /*
    Setter-like behavior with validation
    - Controls how state can change
    - Prevents invalid operations
    */
    void processPayment() {
        if (isPaid) {
            cout << "[Payment] Transaction already completed." << endl;
            return;
        }
        isPaid = true;
        cout << "[Payment] Payment of " << amount
             << " processed successfully via "
             << paymentProvider << endl;
    }

    void refundPayment() {
        if (!isPaid) {
            cout << "[Payment] Cannot refund unpaid transaction." << endl;
            return;
        }
        isPaid = false;
        cout << "[Payment] Refund successful for transaction "
             << transactionId << endl;
    }

    /*
    Note:
    There is NO setter for amount or transactionId.
    This ensures data integrity.
    */
};

// Main Method
int main() {

    Payment* payment = new Payment("TXN-982341", 299.99, "Stripe");

    // Process payment
    payment->processPayment();

    // ❌ NOT ALLOWED (Encapsulation protects internal state)
    // payment->amount = 10000;
    // payment->isPaid = false;

    cout << "Transaction ID: " << payment->getTransactionId() << endl;
    cout << "Amount: " << payment->getAmount() << endl;
    cout << "Payment Status: "
         << (payment->getPaymentStatus() ? "PAID" : "NOT PAID") << endl;

    // Refund payment
    payment->refundPayment();

    delete payment;
    return 0;
}

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


# Lecture 3: Object-Oriented Programming (OOP)

## Inheritance & Polymorphism

### Real-World Systems: Payment Platforms & Ride-Hailing Apps

---

## 1. Inheritance

### 1.1 What is Inheritance?

**Inheritance models real-world “is-a” relationships.**

* A child entity **inherits common properties and behaviors** from a parent.
* The child can **extend or specialize** the parent.

### Real-World Meaning

* **StripePayment** *is a* **PaymentGateway**
* **CarRide** *is a* **Ride**
* **BikeRide** *is a* **Ride**

👉 Inheritance avoids duplication and enforces consistent behavior.

---

## 1.2 Real-World Example: Payment System Hierarchy

### Parent Class: `PaymentGateway` (Generic Payment Interface)

#### Common Responsibilities

* Authenticate provider
* Charge customer
* Refund transaction

```cpp
class PaymentGateway {
public:
    virtual bool authenticate(const std::string& apiKey) = 0;
    virtual bool charge(double amount, const std::string& currency) = 0;
    virtual bool refund(const std::string& transactionId) = 0;
    virtual ~PaymentGateway() {}
};
```

---

### Child Classes (Concrete Providers)

#### StripePayment

* Uses Stripe APIs
* Handles Stripe-specific authentication and retries

#### PayPalPayment

* Uses PayPal SDK
* Handles PayPal session and transactions

```cpp
class StripePayment : public PaymentGateway { };
class PayPalPayment : public PaymentGateway { };
```

### Real-World Impact

* Business logic never changes
* Payment vendors can be swapped easily

---

## 1.3 Ride-Hailing Example: Ride Hierarchy

### Parent Class: `Ride`

#### Common Attributes

* `rideId`
* `status`
* `fare`

#### Common Behaviors

* `startRide()`
* `completeRide()`
* `calculateFare()`

---

### Child Classes

#### CarRide

* Fare based on distance + time

#### BikeRide

* Cheaper fare
* No waiting charges

```cpp
class Ride {
public:
    virtual double calculateFare() = 0;
};
```

---

## 1.4 Access Specifiers in Inheritance

| Access Modifier | Meaning in Real Systems  |
| --------------- | ------------------------ |
| `public`        | API exposed to services  |
| `protected`     | Used by subclasses only  |
| `private`       | Internal, sensitive data |

❌ Payment secrets, fares, and IDs are **never public**

---

## 2. Polymorphism

### 2.1 What is Polymorphism?

**Same action → different behavior depending on object type.**

---

## 2.2 Real-World Scenarios

### Scenario 1: Charging a Payment

```cpp
PaymentGateway* gateway;
gateway->charge(500, "USD");
```

* Stripe → Card charge
* PayPal → Wallet transfer
* JazzCash → Mobile wallet debit

👉 Same method, different execution

---

### Scenario 2: Fare Calculation

```cpp
Ride* ride;
ride->calculateFare();
```

* CarRide → distance + time
* BikeRide → distance only

---

## 2.3 Types of Polymorphism

### 1️⃣ Static Polymorphism (Compile-Time)

* Method Overloading

### 2️⃣ Dynamic Polymorphism (Runtime)

* Method Overriding

---

## 3. Static Polymorphism (Method Overloading)

### Payment Example: Charge Variants

```cpp
class PaymentService {
public:
    void charge(double amount) {
        cout << "Charging default currency" << endl;
    }

    void charge(double amount, string currency) {
        cout << "Charging " << amount << " " << currency << endl;
    }
};
```

### Real-World Meaning

* Charge in default currency
* Charge in specified currency

---

## 4. Dynamic Polymorphism (Method Overriding)

### Abstract Ride Class

```cpp
class Ride {
public:
    virtual double calculateFare() = 0;
};
```

---

### Car Ride Implementation

```cpp
class CarRide : public Ride {
public:
    double calculateFare() override {
        return 300.0;
    }
};
```

---

### Bike Ride Implementation

```cpp
class BikeRide : public Ride {
public:
    double calculateFare() override {
        return 150.0;
    }
};
```

---

### Runtime Behavior

```cpp
void generateInvoice(Ride* ride) {
    cout << "Fare: " << ride->calculateFare() << endl;
}
```

👉 Fare logic decided **at runtime**, not compile time

---

## 5. Combining OOP Pillars (Real-World Architecture)

### Payment & Ride-Hailing Systems Use:

| Pillar        | Example                   |
| ------------- | ------------------------- |
| Abstraction   | PaymentGateway, Ride      |
| Encapsulation | Private transaction state |
| Inheritance   | StripePayment, CarRide    |
| Polymorphism  | charge(), calculateFare() |

---

## Additional Concepts

### Protected Members

* Shared logic for child classes
* Not visible to external services

### Operator Overloading (Homework)

* Customize operators (`+`, `==`)
* Available in C++
* Restricted in Java/Python to prevent ambiguity

---

## Conclusion

* **Inheritance** models real-world hierarchies
* **Polymorphism** enables flexible, scalable systems
* Payment & ride-hailing platforms rely heavily on both
* Clean architecture depends on OOP pillars working together

---

## Homework

1. Define **Operator Overloading**
2. Why is operator overloading limited or restricted in Java & Python?
3. Extend Ride system with `LuxuryRide` or `SharedRide`


## Example Code 

## Inheritance
--
```cpp
#include <iostream>
#include <string>

using namespace std;

/*
We know that real world Objects show inheritance relationship where we
have parent object and child object. child object have all the characters
or behaviours that parent have plus some additional characters/behaviours.
Like all cars in real world have a brand, model etc and can start, stop, 
accelerate etc. But some specific cars like manual car have gear System
while other specific cars like Electric cars have battery system.

We represent this scenario of real world in programming by creating a parent class and
defining all the characters(variables) or behaviours(methods) that all cars 
have in parent class. Then we create different child classes that inherits 
from this parent class and define only those characters and behaviours
that are specific to them. Although objects of these child classes can 
access or call parent class characters(variables) and behaviours(methods).
Hence providing code reusability.
*/
class Car {
protected:
    string brand;
    string model;
    bool isEngineOn;
    int currentSpeed;

public:
    Car(string b, string m) {
        this->brand = b;
        this->model = m;
        isEngineOn = false;
        currentSpeed = 0;
    }

    //Common methods for All cars.
    void startEngine() {
        isEngineOn = true;
        cout << brand << " " << model << " : Engine started." << endl;
    }

    void stopEngine() {
        isEngineOn = false;
        currentSpeed = 0;
        cout << brand << " " << model << " : Engine turned off." << endl;
    }

    void accelerate() { 
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        currentSpeed += 20;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h" << endl;
    }

    void brake() { 
        currentSpeed -= 20;
        if (currentSpeed < 0) currentSpeed = 0;
        cout << brand << " " << model << " : Braking! Speed is now " << currentSpeed << " km/h" << endl;
    }

    virtual ~Car() {}
};

class ManualCar : public Car {  // Inherits from Car
private:
    int currentGear; //spcific to Manual Car.

public:
    ManualCar(string b, string m) : Car(b, m) {
        currentGear = 0;
    }

    //Specialized method for Manual Car
    void shiftGear(int gear) {  
        currentGear = gear;
        cout << brand << " " << model << " : Shifted to gear " << currentGear << endl;
    }
};

class ElectricCar : public Car {  // Inherits from Car
private:
    int batteryLevel; //spcific to Electric Car.

public:
    ElectricCar(string b, string m) : Car(b, m) {
        batteryLevel = 100;
    }

    //specialized method for Electric Car
    void chargeBattery() {  
        batteryLevel = 100;
        cout << brand << " " << model << " : Battery fully charged!" << endl;
    }
};
    
    

// Main Method
int main() {

    ManualCar* myManualCar = new ManualCar("Suzuki", "WagonR");
    myManualCar->startEngine();
    myManualCar->shiftGear(1); //specific to manual car
    myManualCar->accelerate();
    myManualCar->brake();
    myManualCar->stopEngine();
    delete myManualCar;

    cout << "----------------------" << endl;

    ElectricCar* myElectricCar = new ElectricCar("Tesla", "Model S");
    myElectricCar->chargeBattery(); //specific to electric car
    myElectricCar->startEngine(); 
    myElectricCar->accelerate();
    myElectricCar->brake();
    myElectricCar->stopEngine();
    delete myElectricCar; 

    return 0;
}
```
--
## Dynamic Polymorphism
--
```cpp
#include <iostream>
#include <string>

using namespace std;

/*
Dynamic Polymorphism in real life says that 2 Objects coming from same
family will respond to same stimulus differently. Like in real world Manual
car and Electric car will respond to accelerate() differently.

To represent this in programming, we create a parent class that defines all
characters and behaviours that are generic to all child classes and are also same in
all child classes but make those methods abstract(virtual) that are generic to all
child classes but all child class will behave differently. Then those child class
will provide implementation details of these abstract methods the way they want.
*/ 
class Car {
protected:
    string brand;
    string model;
    bool isEngineOn;
    int currentSpeed;

public:
    Car(string brand, string model) {
        this->brand = brand;
        this->model = model;
        this->isEngineOn = false;
        this->currentSpeed = 0;
    }

    //Common methods for All cars.
    void startEngine() {
        isEngineOn = true;
        cout << brand << " " << model << " : Engine started." << endl;
    }

    void stopEngine() {
        isEngineOn = false;
        currentSpeed = 0;
        cout << brand << " " << model << " : Engine turned off." << endl;
    }

    virtual void accelerate() = 0;  // Abstract method for Dynamic Polymorphism
    virtual void brake() = 0;       // Abstract method for Dynamic Polymorphism
    virtual ~Car() {}               // Virtual destructor
};

class ManualCar : public Car {
private:
    int currentGear;

public:
    ManualCar(string brand, string model) : Car(brand, model) {
        this->currentGear = 0;
    }

    //Specialized method for Manual Car
    void shiftGear(int gear) {
        currentGear = gear;
        cout << brand << " " << model << " : Shifted to gear " << currentGear << endl;
    }

    // Overriding accelerate - Dynamic Polymorphism
    void accelerate() {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        currentSpeed += 20;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h" << endl;
    }

    // Overriding brake - Dynamic Polymorphism
    void brake() {
        currentSpeed -= 20;
        if (currentSpeed < 0) currentSpeed = 0;
        cout << brand << " " << model << " : Braking! Speed is now " << currentSpeed << " km/h" << endl;
    }
};

class ElectricCar : public Car {
private:
    int batteryLevel;

public:
    ElectricCar(string brand, string model) : Car(brand, model) {
        this->batteryLevel = 100;
    }

    //specialized method for Electric Car
    void chargeBattery() {
        batteryLevel = 100;
        cout << brand << " " << model << " : Battery fully charged!" << endl;
    }

    // Overriding accelerate - Dynamic Polymorphism
    void accelerate() {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        if (batteryLevel <= 0) {
            cout << brand << " " << model << " : Battery dead! Cannot accelerate." << endl;
            return;
        }
        batteryLevel -= 10;
        currentSpeed += 15;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h. Battery at " << batteryLevel << "%." << endl;
    }

    // Overriding brake - Dynamic Polymorphism
    void brake() {
        currentSpeed -= 15;
        if (currentSpeed < 0) currentSpeed = 0;
        cout << brand << " " << model << " : Regenerative braking! Speed is now " << currentSpeed << " km/h. Battery at " << batteryLevel << "%." << endl;
    }
};


// Main function
int main() {
    Car* myManualCar = new ManualCar("Suzuki", "WagonR");
    myManualCar->startEngine();
    myManualCar->accelerate();
    myManualCar->accelerate();
    myManualCar->brake();
    myManualCar->stopEngine();

    cout << "----------------------" << endl;

    Car* myElectricCar = new ElectricCar("Tesla", "Model S");
    myElectricCar->startEngine();
    myElectricCar->accelerate();
    myElectricCar->accelerate();
    myElectricCar->brake();
    myElectricCar->stopEngine();

    // Cleanup
    delete myManualCar;
    delete myElectricCar;

    return 0;
}
```
--
# Static Polymorphism
--
```cpp
#include <iostream>
#include <string>

using namespace std;

/*
Static Polymorphism (Compile-time polymorphism) in real life says that
the same action can behave differently depending on the input parameters.
For example, a Manual car can accelerate by a fixed amount or by a
specific amount you request. In programming, we achieve this via method
overloading: multiple methods with the same name but different signatures.
*/

class ManualCar {

private:
    string brand;
    string model;
    bool isEngineOn;
    int currentSpeed;
    int currentGear;

public:
    ManualCar(string brand, string model) {
        this->brand = brand;
        this->model = model;
        this->isEngineOn = false;
        this->currentSpeed = 0;
        this->currentGear = 0;
    }

    void startEngine() {
        isEngineOn = true;
        cout << brand << " " << model << " : Engine started." << endl;
    }

    void stopEngine() {
        isEngineOn = false;
        currentSpeed = 0;
        cout << brand << " " << model << " : Engine turned off." << endl;
    }

    // Overloading accelerate - Static Polymorphism
    void accelerate() {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        currentSpeed += 20;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h" << endl;
    }

    void accelerate(int speed) {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        currentSpeed += speed;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h" << endl;
    }

    void brake() {
        currentSpeed -= 20;
        if (currentSpeed < 0) currentSpeed = 0;
        cout << brand << " " << model << " : Braking! Speed is now " << currentSpeed << " km/h" << endl;
    }

    void shiftGear(int gear) {
        currentGear = gear;
        cout << brand << " " << model << " : Shifted to gear " << currentGear << endl;
    }
};

// Main function
int main() {
    ManualCar* myManualCar = new ManualCar("Suzuki", "WagonR");
    myManualCar->startEngine();
    myManualCar->accelerate();
    myManualCar->accelerate(40);
    myManualCar->brake();
    myManualCar->stopEngine();

    // Cleanup
    delete myManualCar;

    return 0;
}
```
--
# Static And Dynamic Polymorphism
```cpp
#include <iostream>
#include <string>

using namespace std;

// Base Car class
class Car {
protected:
    string brand;
    string model;
    bool isEngineOn;
    int currentSpeed;

public:
    Car(string brand, string model) {
        this->brand = brand;
        this->model = model;
        this->isEngineOn = false;
        this->currentSpeed = 0;
    }

    //Common methods for All cars.
    void startEngine() {
        isEngineOn = true;
        cout << brand << " " << model << " : Engine started." << endl;
    }

    void stopEngine() {
        isEngineOn = false;
        currentSpeed = 0;
        cout << brand << " " << model << " : Engine turned off." << endl;
    }

    virtual void accelerate() = 0;  // Abstract method for Dynamic Polymorphism

    virtual void accelerate(int speed) = 0;  //Abstract method for Static Polymorphism

    virtual void brake() = 0;       // Abstract method for Dynamic Polymorphism
    
    virtual ~Car() {}               // Virtual destructor
};

class ManualCar : public Car {
private:
    int currentGear;

public:
    ManualCar(string brand, string model) : Car(brand, model) {
        this->currentGear = 0;
    }

    //Specialized method for Manual Car
    void shiftGear(int gear) {
        currentGear = gear;
        cout << brand << " " << model << " : Shifted to gear " << currentGear << endl;
    }

    // Overriding accelerate - Dynamic Polymorphism
    void accelerate() {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        currentSpeed += 20;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h" << endl;
    }

    //overriding and overloading accelerate at the same time.
    void accelerate(int speed) {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        currentSpeed += speed;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h" << endl;
    }

    // Overriding brake - Dynamic Polymorphism
    void brake() {
        currentSpeed -= 20;
        if (currentSpeed < 0) currentSpeed = 0;
        cout << brand << " " << model << " : Braking! Speed is now " << currentSpeed << " km/h" << endl;
    }
};

class ElectricCar : public Car {
private:
    int batteryLevel;

public:
    ElectricCar(string brand, string model) : Car(brand, model) {
        this->batteryLevel = 100;
    }

    //specialized method for Electric Car
    void chargeBattery() {
        batteryLevel = 100;
        cout << brand << " " << model << " : Battery fully charged!" << endl;
    }

    // Overriding accelerate - Dynamic Polymorphism
    void accelerate() {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        if (batteryLevel <= 0) {
            cout << brand << " " << model << " : Battery dead! Cannot accelerate." << endl;
            return;
        }
        batteryLevel -= 10;
        currentSpeed += 15;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h. Battery at " << batteryLevel << "%." << endl;
    }

    // Overriding accelerate - Dynamic Polymorphism
    void accelerate(int speed) {
        if (!isEngineOn) {
            cout << brand << " " << model << " : Cannot accelerate! Engine is off." << endl;
            return;
        }
        if (batteryLevel <= 0) {
            cout << brand << " " << model << " : Battery dead! Cannot accelerate." << endl;
            return;
        }
        batteryLevel -= 10 + speed;
        currentSpeed += speed;
        cout << brand << " " << model << " : Accelerating to " << currentSpeed << " km/h. Battery at " << batteryLevel << "%." << endl;
    }

    // Overriding brake - Dynamic Polymorphism
    void brake() {
        currentSpeed -= 15;
        if (currentSpeed < 0) currentSpeed = 0;
        cout << brand << " " << model << " : Regenerative braking! Speed is now " << currentSpeed << " km/h. Battery at " << batteryLevel << "%." << endl;
    }
};

// Main function
int main() {
    Car* myManualCar = new ManualCar("Ford", "Mustang");
    myManualCar->startEngine();
    myManualCar->accelerate();
    myManualCar->accelerate();
    myManualCar->brake();
    myManualCar->stopEngine();

    cout << "----------------------" << endl;

    Car* myElectricCar = new ElectricCar("Tesla", "Model S");
    myElectricCar->startEngine();
    myElectricCar->accelerate();
    myElectricCar->accelerate();
    myElectricCar->brake();
    myElectricCar->stopEngine();

    // Cleanup
    delete myManualCar;
    delete myElectricCar;

    return 0;
}
```


# 🧠 Object-Oriented Programming (OOP) — Complete Notes
### *Placement & Interview Ready | C++ & Java Parallel Coverage*

---

> 📌 **Quick Navigation**
> `Introduction` → `Classes & Objects` → `Access Specifiers` → `4 Pillars` → `Constructors` → `Inheritance` → `Polymorphism` → `Abstraction` → `Static Members` → `MCQ Answer Key`

---

## 📖 Table of Contents

| # | Topic | Key Concept |
|---|-------|-------------|
| 1 | Introduction | What & Why OOP |
| 2 | Classes & Objects | Blueprint vs Instance |
| 3 | Access Specifiers | private / protected / public |
| 4 | Friend Class | C++ Only |
| 5 | Core 4 Pillars | Encapsulation, Inheritance, Polymorphism, Abstraction |
| 6 | Encapsulation | Data Hiding |
| 7 | Constructor & Destructor | Object Lifecycle |
| 8 | Scope Resolution `::` | C++ Only |
| 9 | `this` Pointer | Current Object Reference |
| 10 | Shallow vs Deep Copy | Memory Copying |
| 11 | Inheritance | Types & Modes |
| 12 | Diamond Problem | Virtual Inheritance |
| 13 | Polymorphism | Overloading vs Overriding |
| 14 | Virtual Functions | C++ Dynamic Binding |
| 15 | Abstraction | Abstract Class & Interface |
| 16 | Static Members | Class-level Data |
| 17 | MCQ Answer Key | 45 Questions |

---

## 1. 🚀 Introduction to OOP

### What is OOP?

> **OOP** is a programming paradigm that models software around real-world **objects** — each having **data (attributes)** and **behavior (methods)**.

```
Real World Entity        OOP Equivalent
─────────────────────────────────────────
Car Blueprint      ──▶  Class
Actual Car         ──▶  Object
Color, Engine      ──▶  Attributes (Data)
Drive, Brake       ──▶  Methods (Behavior)
```

### ❓ Why OOP? — Procedural vs OOP

| Problem in Procedural Code | OOP Solution |
|---------------------------|--------------|
| No clear modular structure | Classes group data & logic |
| Poor code reuse | Inheritance enables reuse |
| Data not secure | Encapsulation hides sensitive info |
| Hard to manage large code | Abstraction & structure |
| Hard to model real-world entities | OOP models real-world objects |

### ✅ Advantages of OOP

- **Modularity** — Code divided into classes → organized & manageable
- **Reusability** — Inheritance allows reuse, reducing redundancy
- **Data Hiding** — Encapsulation exposes only what's necessary
- **Maintainability** — Update parts without affecting unrelated code
- **Real-world Modelling** — Mirrors entities like Student, Car, Account
- **Scalability** — Complex apps can be built & maintained efficiently

---

## 2. 📦 Classes & Objects

```
┌─────────────────────────────────────────────┐
│                  CLASS (Blueprint)          │
│  ┌───────────────┐    ┌───────────────────┐ │
│  │  Attributes   │    │     Methods       │ │
│  │  color        │    │  setColor()       │ │
│  │  tip : int    │    │  setTip()         │ │
│  └───────────────┘    └───────────────────┘ │
└─────────────────────────────────────────────┘
           │
           │  instantiate
           ▼
┌─────────────────────────────────────────────┐
│               OBJECT (Instance)             │
│   color = "Blue",  tip = 5                  │
└─────────────────────────────────────────────┘
```

> 📌 **Key Rule:** Class = blueprint (no memory). Object = real instance (memory allocated).

### Code — Parallel C++ & Java

```cpp
// C++ — Static & Dynamic Object Creation
class Car {
public:
    string color;
    int engineCC;

    void drive() {
        cout << "Driving a " << color << " car" << endl;
    }
};

int main() {
    Car myCar;              // Static allocation
    myCar.color = "Red";
    myCar.engineCC = 2000;
    myCar.drive();

    Car* carPtr = new Car(); // Dynamic allocation
    carPtr->color = "Blue";
    carPtr->drive();
    delete carPtr;           // ⚠️ Must free dynamic memory in C++
}
```

```java
// Java — Object Creation
class Car {
    String color;
    int engineCC;

    void drive() {
        System.out.println("Driving a " + color + " car");
    }

    public static void main(String[] args) {
        Car myCar = new Car();   // Always heap allocated
        myCar.color = "Red";
        myCar.engineCC = 2000;
        myCar.drive();
    }
}
```

---

## 3. 🔒 Access Specifiers / Modifiers

> Control the **visibility** of class members (variables & methods).

### Types

| Specifier | Accessible In |
|-----------|--------------|
| `private` | Within the **same class** only |
| `protected` | Same class + **derived/child classes** + same package (Java) |
| `public` | **Everywhere** |

### Java Access Table

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| `private` | ✅ | ❌ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |
| `default` (no keyword) | ✅ | ✅ | ❌ | ❌ |

> ⚡ **C++ Note:** Only `private`, `protected`, `public` exist. No `default` package concept.

---

## 4. 🤝 Friend Class *(C++ Only)*

> A **friend class** is granted special access to **private & protected** members of another class.

```
┌──────────────────┐          ┌──────────────────┐
│   class Car      │◀─friend──│  class Mechanic  │
│  - engineNumber  │          │  + checkEngine() │
└──────────────────┘          └──────────────────┘
  (private data)               (can access private)
```

```cpp
class Car {
private:
    string engineNumber;
public:
    Car() { engineNumber = "XYZ1234"; }
    friend class Mechanic;  // Grant access
};

class Mechanic {
public:
    void checkEngine(Car c) {
        cout << "Engine: " << c.engineNumber << endl;  // ✅ Allowed
    }
};
```

> 📌 **Interview Q:** Does Java have `friend` classes? **No.** Java uses package-level access (`default`) or inner classes instead.

---

## 5. 🏛️ The 4 Pillars of OOP

```
                    ┌─────────────────────────┐
                    │     4 PILLARS OF OOP    │
                    └─────────────────────────┘
          ┌──────────────┬──────────────┬──────────────┐
          ▼              ▼              ▼              ▼
   Encapsulation   Inheritance   Polymorphism   Abstraction
   (Data Hiding)  (Code Reuse)  (Many Forms)  (Hide Details)
```

| Pillar | One-liner | Real-life Analogy |
|--------|-----------|-------------------|
| **Encapsulation** | Wrap data + methods in a class; hide internals | A capsule hides medicine inside |
| **Inheritance** | Child class inherits from parent class | Child inherits eye color from parents |
| **Polymorphism** | Same method, different behaviors | Same person: student at college, player on field |
| **Abstraction** | Show only essential features; hide the rest | You tap app icons, not write code to run them |

---

## 6. 🛡️ Encapsulation

> **Wrapping** data (attributes) and methods into a single unit (class) and **restricting direct access** using access modifiers.

```
┌──────────────────────────────────────┐
│            CLASS (Capsule)           │
│  ┌───────────────┬─────────────────┐ │
│  │  Variables /  │   Methods /     │ │
│  │  Data Members │   Functions     │ │
│  └───────────────┴─────────────────┘ │
│   🔒 private data  ──▶ public getter/setter │
└──────────────────────────────────────┘
```

```cpp
// C++ Encapsulation
class Student {
private:
    int age;    // Hidden from outside

public:
    void setAge(int a) {
        if (a > 0) age = a;  // Validation before setting
    }
    int getAge() {
        return age;
    }
};

int main() {
    Student s;
    s.setAge(20);         // ✅ Valid
    cout << s.getAge();   // Output: 20
    // s.age = 25;        // ❌ Compile error — private!
}
```

```java
// Java Encapsulation
class Student {
    private int age;

    public void setAge(int a) {
        if (a > 0) age = a;
    }
    public int getAge() {
        return age;
    }

    public static void main(String[] args) {
        Student s = new Student();
        s.setAge(20);
        System.out.println(s.getAge()); // Output: 20
        // s.age = 25;                  // ❌ Not allowed
    }
}
```

> ⚡ **Interview Tip:** Encapsulation = Data Hiding. Always use `private` fields + `public` getters/setters. This enables **validation logic** inside setters.

---

## 7. 🔨 Constructor & Destructor

### Constructor

> **Special method** auto-called at object creation. Used for **initialization**. Same name as class. **No return type.**

```
Object Creation Timeline:
  new ──▶ Memory Allocated ──▶ Constructor Called ──▶ Object Ready
```

### 3 Types of Constructors

```cpp
// ─────────────── C++ ───────────────

// 1. Default / Non-Parameterized
class Student {
public:
    Student() {
        cout << "Default Constructor\n";
    }
};

// 2. Parameterized
class Student {
public:
    string name;
    Student(string n) { name = n; }
};

// 3. Copy Constructor
class Student {
public:
    string name;
    Student(string n) { name = n; }

    Student(const Student &s) {   // Copy Constructor
        name = s.name;
    }
};
```

```java
// ─────────────── Java ───────────────

// 1. Default
class Student {
    Student() {
        System.out.println("Default Constructor");
    }
}

// 2. Parameterized
class Student {
    String name;
    Student(String n) { name = n; }
}

// 3. Copy Constructor (manual in Java)
class Student {
    String name;
    Student(String n) { name = n; }

    Student(Student s) {          // Copy Constructor
        this.name = s.name;
    }
}
```

### Destructor *(C++ Only)*

> Auto-called when object **goes out of scope** or is **deleted**. Used to **free resources**. Prefixed with `~`. No return type or parameters.

```cpp
class Student {
public:
    Student()  { cout << "Constructor called\n"; }
    ~Student() { cout << "Destructor called\n"; }  // Auto-called
};

int main() {
    Student s1;  // Constructor called
}                // Destructor called automatically here
```

### Java — Garbage Collector

| Feature | C++ | Java |
|---------|-----|------|
| Memory cleanup | Manual (`delete`) + Destructor | Automatic — Garbage Collector |
| When triggered | Object goes out of scope | JVM decides (non-deterministic) |
| Method | `~ClassName()` | `finalize()` *(deprecated Java 9+)* |

> ⚡ **Interview Tip:** Java's GC detects unreachable objects, reclaims memory, prevents memory leaks — but you have **no control** over when it runs.

---

## 8. 🎯 Scope Resolution Operator `::` *(C++ Only)*

> Accesses members **outside current scope** or belonging to a specific class/namespace.

```cpp
// 1. Access global variable shadowed by local
int x = 10; // global
int main() {
    int x = 20;          // local shadows global
    cout << x;           // 20 (local)
    cout << ::x;         // 10 (global, using ::)
}

// 2. Define class function outside class body
class Student {
public:
    void show();         // Declared inside
};

void Student::show() {   // Defined outside with ::
    cout << "Function defined outside\n";
}

// 3. Access namespace members
namespace MyNS {
    int value = 5;
}
cout << MyNS::value;     // 5
```

---

## 9. 👉 `this` Pointer / Reference

> `this` refers to the **current object** — the object whose method is being executed.

```
┌─────────────────────────────────────────────────┐
│  C++           │  this ──▶  pointer (this->)    │
│  Java          │  this ──▶  reference (this.)   │
└─────────────────────────────────────────────────┘
```

**Key use case:** When parameter name **shadows** instance variable name.

```cpp
// C++
class Student {
public:
    string name;
    Student(string name) {
        this->name = name;  // this->name = instance variable
                            // name = parameter
    }
    void show() {
        cout << this->name << endl;
    }
};
```

```java
// Java
class Student {
    String name;
    Student(String name) {
        this.name = name;   // this.name = instance variable
    }
    void show() {
        System.out.println(this.name);
    }
}
```

> 📌 **Rules:** `this` is only available in **non-static** methods. Static methods have no object context — so no `this`.

---

## 10. 📋 Shallow Copy vs Deep Copy

```
SHALLOW COPY                    DEEP COPY
────────────────                ────────────────
  s1 ──▶ [marks] ◀── s2          s1 ──▶ [marks1]
          (SHARED)                s2 ──▶ [marks2]
  ⚠️ Changing s2 affects s1       ✅ Fully independent
```

| Feature | Shallow Copy | Deep Copy |
|---------|-------------|-----------|
| What's copied | Reference/address | Actual value (new memory) |
| Memory | Shared between objects | Independent memory |
| Side effects | Modifying one affects other | No side effects |
| Analogy | Photocopy of a key | Brand new key cut from scratch |

```cpp
// C++ — Shallow Copy (default copy constructor)
Student(const Student& s) {
    marks = s.marks;            // ❌ Copies address only — SHARED!
}

// C++ — Deep Copy (custom copy constructor)
Student(const Student& s) {
    marks = new int(*(s.marks)); // ✅ New memory + copies value
}
```

```java
// Java — Shallow Copy
Student(Student s) {
    this.marks = s.marks;        // ❌ Both point to same array
}

// Java — Deep Copy
Student(Student s) {
    this.marks = new int[1];
    this.marks[0] = s.marks[0]; // ✅ New array, independent
}
```

> ⚡ **Interview Tip:** C++'s default copy constructor does **shallow copy**. Always write a custom copy constructor when your class has **pointers/dynamic memory**.

---

## 11. 🌳 Inheritance

> A **child/derived class** inherits properties and behavior from a **parent/base class**.

```
Parent Class (Base)
      │
      │  inherits
      ▼
Child Class (Derived)
```

```cpp
// C++ — Keyword: colon (:)
class Vehicle {
public:
    void start() { cout << "Vehicle started\n"; }
};

class Car : public Vehicle {   // Car inherits Vehicle
public:
    void drive() { cout << "Car is driving\n"; }
};
```

```java
// Java — Keyword: extends
class Vehicle {
    void start() { System.out.println("Vehicle started"); }
}

class Car extends Vehicle {    // Car inherits Vehicle
    void drive() { System.out.println("Car is driving"); }
}
```

---

### 🔑 Modes of Inheritance *(C++ Only)*

| Base Member | Public Inheritance | Protected Inheritance | Private Inheritance |
|-------------|-------------------|----------------------|---------------------|
| `public` | `public` | `protected` | `private` |
| `protected` | `protected` | `protected` | `private` |
| `private` | Not inherited | Not inherited | Not inherited |

> 📌 **Default in C++:** `private` inheritance. Java's `extends` is always equivalent to `public` inheritance.

---

### 🌿 5 Types of Inheritance

#### 1. Single Inheritance — One parent → One child
```
A ──▶ B
```
```cpp
class A { };
class B : public A { };   // C++
```
```java
class B extends A { }     // Java
```

---

#### 2. Multilevel Inheritance — Chain
```
A ──▶ B ──▶ C
```
```cpp
class A { };
class B : public A { };
class C : public B { };
```
```java
class B extends A { }
class C extends B { }
```

---

#### 3. Hierarchical Inheritance — One parent → Many children
```
        A
      ┌─┴─┐
      B   C
```
```cpp
class B : public A { };
class C : public A { };
```
```java
class B extends A { }
class C extends A { }
```

---

#### 4. Multiple Inheritance — One child → Many parents

```
  A     B
   \   /
    \ /
     C
```

| Feature | C++ | Java |
|---------|-----|------|
| Multiple class inheritance | ✅ Supported | ❌ Not allowed |
| Multiple interface inheritance | — | ✅ Supported |
| Reason Java avoids it | — | Ambiguity / Diamond Problem |

```cpp
// C++ — Multiple Inheritance
class C : public A, public B { };
```

```java
// Java — Via Interfaces only
interface A { void showA(); }
interface B { void showB(); }

class C implements A, B {
    public void showA() { System.out.println("A"); }
    public void showB() { System.out.println("B"); }
}
```

---

#### 5. Hybrid Inheritance — Combination of types
```
        A
      ┌─┴─┐
      B   C
       \ /
        D
```
> C++ supports directly. Java uses interfaces to achieve the same.

---

## 12. 💎 The Diamond Problem

```
        ┌─────┐
        │  A  │
        └──┬──┘
        ╱     ╲
   ┌───┴──┐  ┌──┴───┐
   │  B   │  │  C   │
   └───┬──┘  └──┬───┘
        ╲     ╱
        ┌─┴────┴─┐
        │   D    │
        └────────┘
  ⚠️ D inherits A twice — AMBIGUOUS!
```

> When D calls a method from A — through B or through C? **Compiler can't decide.**

### Problem in C++

```cpp
class A {
public:
    void show() { cout << "Class A\n"; }
};

class B : public A { };
class C : public A { };
class D : public B, public C { };  // Diamond!

int main() {
    D obj;
    // obj.show();       // ❌ Ambiguous!
    obj.B::show();       // ✅ Manually resolve
    obj.C::show();
}
```

### Solution — Virtual Inheritance *(C++)*

```cpp
class B : virtual public A { };  // virtual keyword
class C : virtual public A { };
class D : public B, public C { };

int main() {
    D obj;
    obj.show();  // ✅ No ambiguity — only ONE copy of A
}
```

### Java's Approach

```java
// Java avoids diamond problem — no multiple class inheritance
interface A { void show(); }
interface B extends A { }
interface C extends A { }

class D implements B, C {
    public void show() {
        System.out.println("D's show — no ambiguity!");
    }
}
```

> ⚡ **Interview Tip:** Java avoids diamond problem because it only allows **single class inheritance**. When implementing multiple interfaces with same method, the class **must provide its own implementation** — resolving ambiguity.

---

## 13. 🎭 Polymorphism

> **"Many Forms"** — Same method name, different behavior depending on context.

```
┌─────────────────────────────────────┐
│           POLYMORPHISM              │
├──────────────────┬──────────────────┤
│  Compile-time    │   Run-time       │
│  (Static)        │   (Dynamic)      │
├──────────────────┼──────────────────┤
│• Method          │• Method          │
│  Overloading     │  Overriding      │
│• Operator        │• Virtual         │
│  Overloading     │  Functions       │
│  (C++ only)      │  (C++ only)      │
└──────────────────┴──────────────────┘
```

---

### ⚡ Compile-time Polymorphism (Static Binding / Early Binding)

> Method resolved at **compile time** based on method **signature** (name + parameters).

#### Method Overloading

```cpp
// C++ — Method Overloading
class Calculator {
public:
    int    add(int a, int b)             { return a + b; }
    double add(double a, double b)       { return a + b; }
    int    add(int a, int b, int c)      { return a + b + c; }
};
```

```java
// Java — Method Overloading
class Calculator {
    int    add(int a, int b)             { return a + b; }
    double add(double a, double b)       { return a + b; }
    int    add(int a, int b, int c)      { return a + b + c; }
}
```

**Rules for Overloading:**

| Rule | Detail |
|------|--------|
| Method name | Must be **same** |
| Parameters | Must differ in type, number, or order |
| Return type | **Cannot** be the only difference |

---

#### Operator Overloading *(C++ only)*

```cpp
class Complex {
public:
    int real, imag;
    Complex(int r, int i) : real(r), imag(i) {}

    Complex operator+(const Complex& obj) {   // Overload '+'
        return Complex(real + obj.real, imag + obj.imag);
    }
    void show() { cout << real << "+" << imag << "i\n"; }
};

int main() {
    Complex c1(2,3), c2(1,4);
    Complex c3 = c1 + c2;   // Uses overloaded '+'
    c3.show();               // Output: 3+7i
}
```

> 📌 **Java Note:** Java does **NOT** support operator overloading (except `+` for String concatenation — built-in, not customizable).

---

### 🔄 Run-time Polymorphism (Dynamic Binding / Late Binding)

> Method resolved at **runtime** based on **actual object type**, not the reference type.

#### Method Overriding

```
Base class pointer/reference ──▶ Points to Derived object
                                        │
                            At runtime, calls Derived's method
```

```cpp
// C++ — Method Overriding (needs virtual keyword)
class Animal {
public:
    virtual void speak() {           // virtual enables dynamic dispatch
        cout << "Animal speaks\n";
    }
};

class Dog : public Animal {
public:
    void speak() override {          // override keyword (C++11)
        cout << "Dog barks\n";
    }
};

int main() {
    Animal* a = new Dog();
    a->speak();    // Output: Dog barks  ✅ (dynamic dispatch)
    delete a;
    // Without virtual → "Animal speaks" ❌
}
```

```java
// Java — Method Overriding (dynamic dispatch by default)
class Animal {
    public void speak() {
        System.out.println("Animal speaks");
    }
}

class Dog extends Animal {
    @Override
    public void speak() {
        System.out.println("Dog barks");
    }
}

class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.speak();  // Output: Dog barks ✅
    }
}
```

**Rules for Overriding:**

| Rule | Detail |
|------|--------|
| Method name | Must be **same** |
| Parameters | Must be **exactly same** |
| Return type | Must be same (or covariant in Java) |
| Inheritance | **Required** — happens between base & derived |

---

### 📊 Overloading vs Overriding — Comparison Table

| Feature | Overloading | Overriding |
|---------|-------------|------------|
| Definition | Same name, different parameters **in same class** | Same signature **in parent & child class** |
| Polymorphism type | Compile-time (Static) | Runtime (Dynamic) |
| Inheritance required | ❌ No | ✅ Yes |
| Parameters | Must differ | Must be exactly same |
| Return type | Can differ | Must be same (or covariant) |
| `virtual` (C++) | Not needed | Required for dynamic dispatch |
| `@Override` (Java) | Not used | Recommended (catches errors) |

---

## 14. ⚙️ Virtual Functions *(C++ Only)*

> A **virtual function** tells the compiler to use **runtime (dynamic) binding** instead of compile-time binding.

```
WITHOUT virtual:                WITH virtual:
──────────────                  ────────────
Base* ptr = new Derived();      Base* ptr = new Derived();
ptr->method()                   ptr->method()
      │                               │
      ▼ (compile-time decision)       ▼ (runtime decision)
  Base::method() called          Derived::method() called ✅
```

```cpp
class Animal {
public:
    virtual void speak() {          // virtual function
        cout << "Animal speaks\n";
    }
    virtual ~Animal() {}            // Always make destructor virtual!
};

class Dog : public Animal {
public:
    void speak() override {
        cout << "Dog barks\n";
    }
};
```

> ⚡ **Critical Interview Point:** Always declare destructors as `virtual` in base classes when using polymorphism! Otherwise, deleting a derived object via base pointer causes **undefined behavior** (only base destructor called).

---

## 15. 🎨 Abstraction

> **Hiding** unnecessary implementation details and showing only **essential/relevant features**.

```
User taps   ──▶   App opens   ──▶   (Internal code hidden)
```

### Abstract Class — C++

> **Cannot be instantiated**. Has at least one **pure virtual function** (`= 0`).

```cpp
class Shape {                          // Abstract class
public:
    virtual void area() = 0;           // Pure virtual function
    void display() {
        cout << "This is a shape.\n";  // Concrete method ✅
    }
    virtual ~Shape() {}                // Virtual destructor
};

class Circle : public Shape {
public:
    void area() override {             // MUST implement pure virtual
        cout << "Area = π * r * r\n";
    }
};

class Rectangle : public Shape {
public:
    void area() override {
        cout << "Area = l * b\n";
    }
};

int main() {
    // Shape s;                        // ❌ Cannot instantiate abstract class
    Shape* s1 = new Circle();          // ✅ Pointer to abstract class OK
    s1->area();                        // Area = π * r * r
    s1->display();                     // This is a shape.
    delete s1;
}
```

---

### Abstract Class — Java

> Cannot be instantiated. Can have **abstract** (no body) AND **concrete** (with body) methods.

```java
abstract class Animal {
    String name;

    Animal(String name) {              // Constructors allowed ✅
        this.name = name;
    }

    public abstract void makeSound();  // Must be overridden
    
    public void sleep() {              // Concrete method ✅
        System.out.println(name + " is sleeping");
    }
}

class Dog extends Animal {
    Dog(String name) { super(name); }

    @Override
    public void makeSound() {
        System.out.println(name + " barks!");
    }
}
```

---

### Interface — Java

> A **contract** — defines WHAT a class must do, not HOW. Pure abstraction.

```java
interface Flyable {
    void fly();                        // Abstract by default

    default void land() {              // Default method (Java 8+)
        System.out.println("Landing...");
    }

    static void info() {               // Static method
        System.out.println("Flyable interface");
    }
}

class Bird implements Flyable {
    public void fly() {
        System.out.println("Bird is flying!");
    }
}
```

---

### 🆚 Abstract Class vs Interface — Java

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Keyword | `extends` | `implements` |
| Multiple inheritance | ❌ No (single class) | ✅ Yes (multiple interfaces) |
| Constructors | ✅ Yes | ❌ No |
| Fields | Instance + static | Only `public static final` (constants) |
| Abstract methods | ✅ Yes | ✅ Yes |
| Concrete methods | ✅ Yes | ✅ Via `default`/`static` |
| Access modifiers | Any | `public` by default |
| Instantiation | ❌ No | ❌ No |
| Use case | **Shared base logic** | **Capability/behavior contract** |

> ⚡ **When to use what?**
> - **Abstract class** → when classes share **common implementation**
> - **Interface** → when you want to define a **contract/capability** (e.g., `Flyable`, `Serializable`)

---

## 16. 📊 Static Data Member & Static Member Function

```
┌─────────────────────────────────────────────────────────┐
│                    CLASS MEMORY                         │
│  ┌──────────────────────────────────────────────────┐  │
│  │  static int count   ◀── SHARED by ALL objects   │  │
│  └──────────────────────────────────────────────────┘  │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐   │
│  │  Object 1   │  │  Object 2   │  │  Object 3   │   │
│  │  (own data) │  │  (own data) │  │  (own data) │   │
│  └─────────────┘  └─────────────┘  └─────────────┘   │
└─────────────────────────────────────────────────────────┘
```

| Feature | Static Data Member | Static Member Function |
|---------|-------------------|----------------------|
| Belongs to | The **class** (not any object) | The **class** (not any object) |
| Memory | Allocated **once**, shared by all | Called without object |
| Access | Any object change reflects everywhere | Can only access **static members** |
| C++ call | `ClassName::member` | `ClassName::function()` |
| Java call | `ClassName.member` | `ClassName.function()` |

```cpp
// C++ Static Example
class Counter {
public:
    static int count;  // Declared here

    Counter() { count++; }

    static void showCount() {
        cout << "Count: " << count << endl;
        // cout << name;  ❌ Can't access non-static member
    }
};

int Counter::count = 0;  // Defined outside class

int main() {
    Counter c1, c2, c3;
    Counter::showCount();  // Count: 3
}
```

```java
// Java Static Example
class Counter {
    static int count = 0;

    Counter() { count++; }

    static void showCount() {
        System.out.println("Count: " + count);
    }

    public static void main(String[] args) {
        new Counter(); new Counter(); new Counter();
        Counter.showCount();  // Count: 3
    }
}
```

---

## 17. 🧩 Quick Reference — Key Differences C++ vs Java

| Feature | C++ | Java |
|---------|-----|------|
| Multiple inheritance | ✅ Classes | ✅ Interfaces only |
| Operator overloading | ✅ Yes | ❌ No |
| Destructor | ✅ `~ClassName()` | ❌ GC handles it |
| `virtual` keyword | Required for overriding | Not needed (default) |
| Friend class | ✅ Yes | ❌ No |
| Scope resolution `::` | ✅ Yes | ❌ No |
| Pointers | ✅ Yes | ❌ References only |
| Memory management | Manual (`new`/`delete`) | Automatic (GC) |
| `abstract` keyword | Pure virtual `= 0` | `abstract` keyword |
| Interface | Not native (use pure abstract) | `interface` keyword |
| `this` | Pointer (`this->`) | Reference (`this.`) |
| Default inheritance mode | `private` | `public` (via `extends`) |

---

## 18. 📝 MCQ Answer Key (All 45 Questions)

| Q | Ans | Q | Ans | Q | Ans | Q | Ans | Q | Ans |
|---|-----|---|-----|---|-----|---|-----|---|-----|
| 1 | C | 2 | D | 3 | B | 4 | B | 5 | C |
| 6 | D | 7 | C | 8 | C | 9 | D | 10 | B |
| 11 | B | 12 | B | 13 | B | 14 | A | 15 | B |
| 16 | C | 17 | C | 18 | C | 19 | A | 20 | C |
| 21 | B | 22 | A | 23 | A | 24 | C | 25 | B |
| 26 | D | 27 | C | 28 | B | 29 | C | 30 | B |
| 31 | C | 32 | C | 33 | C | 34 | A | 35 | A |
| 36 | A | 37 | C | 38 | A | 39 | C | 40 | B |
| 41 | B | 42 | A | 43 | C | 44 | A | 45 | C |

### Key Explanations for Tricky Questions

| Q | Explanation |
|---|-------------|
| Q10 | C++ default class inheritance is **private** (struct defaults to public) |
| Q14 | **Static** methods can't be overridden — they're class-level, not object-level |
| Q22 | Without virtual destructor, only base destructor runs → **memory leak / UB** |
| Q24 | A class occupies **zero memory** until an object is created |
| Q28 | Empty C++ class = **1 byte** (to ensure unique addresses for objects) |
| Q29 | **Constructors** can be overloaded but NOT overridden or inherited |
| Q34 | **Constructors** cannot be `virtual` in C++ |
| Q42 | **Object slicing** — assigning derived to base object loses derived data |
| Q45 | `finalize()` is **deprecated since Java 9** — use try-with-resources instead |

---

## 🎯 Interview Cheat Sheet

```
┌────────────────────────────────────────────────────────────────┐
│                    TOP INTERVIEW TOPICS                        │
├────────────────────────────────────────────────────────────────┤
│ ✅ 4 Pillars — be able to define + give real-world examples    │
│ ✅ Overloading vs Overriding — differences table               │
│ ✅ Virtual functions — why needed, what happens without them   │
│ ✅ Diamond problem — explain + C++ solution + Java avoidance   │
│ ✅ Abstract class vs Interface — when to use which             │
│ ✅ Shallow vs Deep copy — with memory diagrams                 │
│ ✅ Static members — class vs object level                      │
│ ✅ Constructor types — default, parameterized, copy            │
│ ✅ C++ destructor vs Java GC                                   │
│ ✅ Access specifiers — private/protected/public table          │
└────────────────────────────────────────────────────────────────┘
```

---

*Notes compiled from Apna College OOP PDF — Structured for placement & interview preparation.*
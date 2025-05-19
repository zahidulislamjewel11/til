## Java OOP Questions

### **1. Can a Java class implement multiple interfaces with the same method signature but different return types?**

**Answer:**
No. This will cause a compilation error because Java uses method signatures (name + parameters) for overloading, and return type is not part of the signature.

---

### **2. What happens if a class implements two interfaces with default methods having the same signature?**

**Answer:**
Compilation error unless you **override the method** explicitly in your class to resolve the conflict.

```java
interface A {
    default void show() { System.out.println("A"); }
}

interface B {
    default void show() { System.out.println("B"); }
}

class C implements A, B {
    public void show() {
        A.super.show(); // or B.super.show();
    }
}
```

---

### **3. Can you override a private or static method in Java? Explain with reasoning.**

**Answer:**

* **Private methods** cannot be overridden because they are not visible to subclasses. If a subclass defines a method with the same signature, it's a **new method** (method hiding), not an override.
* **Static methods** are class-level and also cannot be overridden. If a subclass declares a static method with the same signature, it's method **hiding**, not overriding.

Thus, **polymorphic behavior does not apply** to private or static methods.

---

### **4. Is Java’s object model truly 100% object-oriented? If not, why?**

**Answer:**
No, Java is **not purely object-oriented**.

* **Primitive types** (e.g., `int`, `boolean`) are **not objects**.
* Operations on primitives are not done via method calls, breaking the "everything is an object" principle.
* Java allows **static methods and fields**, which are not tied to object instances.

Languages like Smalltalk are closer to pure object-oriented models. Java is a **multi-paradigm language** leaning heavily on OOP, but with pragmatic deviations for performance.


---

### **5. What is the difference between abstraction and encapsulation with real examples?**

**Answer:**

* **Abstraction**: Hiding *implementation details*, exposing only essential behavior.
   *Example*: `List list = new ArrayList();` — You don’t care how `add()` is implemented.

* **Encapsulation**: Binding data and methods together + restricting direct access via `private` fields and `getters/setters`.

---

### **6. What is object slicing? Can it happen in Java?**

**Answer:**
**Object slicing** occurs when a subclass object is assigned to a superclass variable and the subclass-specific fields/methods are "sliced off".

In **C++**, slicing physically removes derived class members.
In **Java**, slicing as such doesn't happen because Java uses **references**.

However, similar behavior can occur if you **explicitly copy only the base part** or **serialize superclass fields only**, leading to loss of subclass data.

---

### **7. What are covariant return types and how do they affect method overriding in Java?**

**Answer:**
Covariant return types allow a **subclass to override a method** and **change the return type** to a subclass of the original return type.

```java
class Animal {}
class Dog extends Animal {}

class Parent {
    Animal getPet() { ... }
}

class Child extends Parent {
    Dog getPet() { ... } // Valid in Java
}
```

This enables more specific behavior in subclasses and helps **avoid casting** in client code.

---

### **8. What is the real difference between composition and aggregation in Java? Why would you prefer one over the other?**

**Answer:**
Both composition and aggregation represent "has-a" relationships, but the **lifecycle dependency** is the key difference.

* **Composition** means a class *owns* the other class and is responsible for its lifecycle. When the container object is destroyed, the contained objects are also destroyed. E.g., `House` and `Room`. If the house is destroyed, rooms are gone too.
* **Aggregation** is a weaker relationship. The contained object can exist independently. E.g., `Department` and `Professor`. A professor can exist even if the department is removed.

**Preference:**
Use **composition** when the container must manage the lifecycle of components, ensuring tight control and encapsulation. Use **aggregation** when the objects can live independently, promoting flexibility and reusability.

---

### **9. Can constructor be overridden? If not, why?**

**Answer:**
No, constructors **cannot be overridden** because they are not inherited by subclasses. Overriding applies to instance methods that are inherited.

However, you can **overload** constructors within a class to provide multiple ways of object creation.

---

### **10. Is "new" always required to create an object in Java?**

**Answer:**
No. Objects can also be created via:

* **Deserialization**
* **Cloning**
* **Reflection**
* **Factory methods (e.g., `valueOf`)**

---

### **11. How does the Liskov Substitution Principle (LSP) relate to Java inheritance? Give an example where LSP can be violated.**

**Answer:**
LSP states that **subtypes must be substitutable** for their base types without affecting correctness.
LSP says: A subclass should be substitutable for its superclass **without breaking behavior**.

**Violation Example:**

```java
class Bird {
    void fly() { ... }
}

class Ostrich extends Bird {
    void fly() {
        throw new UnsupportedOperationException("Ostrich can't fly");
    }
}
```

Here, substituting `Bird` with `Ostrich` will break the code where `fly()` is expected to work. This violates LSP.

**Fix**: Don't put `fly()` in `Bird`. Use interfaces or redesign hierarchy.

**Better design:** Use interfaces like `Flyable` and decouple `Bird` from fly behavior. This avoids forcing behavior on all subtypes.

---

### **12. Can abstract classes have constructors? What’s their use?**

**Answer:**
Yes. Abstract classes can have constructors. They're called **when subclass constructors are invoked**, to **initialize inherited fields**.

---

### **13. What is the diamond problem in OOP? Why doesn’t Java face it with classes?**

**Answer:**
The **diamond problem** arises in **multiple inheritance** when a class inherits from two classes that have a common ancestor, leading to ambiguity.

Java **avoids this** by:

* **Disallowing multiple inheritance** with classes.
* Allowing it with interfaces only (default methods), and providing explicit conflict resolution.

```java
interface A { default void greet() { ... } }
interface B { default void greet() { ... } }

class C implements A, B {
    @Override
    public void greet() {
        A.super.greet(); // resolve explicitly
    }
}
```

Thus, **Java solves diamond problem via interface default method conflict resolution**.

---

### **14. What's the difference between instanceof and getClass()?**

**Answer:**

* `instanceof` returns `true` for instances of subclasses too.
* `getClass()` checks **exact type**.

```java
SubClass obj = new SubClass();
System.out.println(obj instanceof SuperClass); // true
System.out.println(obj.getClass() == SuperClass.class); // false
```

---

### **15. How does Java support runtime polymorphism under the hood?**

**Answer:**
Using **method overriding** and **dynamic method dispatch**.
Java uses **virtual method tables (v-tables)** to support runtime polymorphism.

* Every class with instance methods has a v-table.
* The JVM uses these tables to resolve method calls at runtime.
* When you call a method on a superclass reference pointing to a subclass object, the method is resolved using the v-table of the **actual class**, not the reference type.

This is the backbone of **dynamic method dispatch** in Java.

---

### **16. Explain method overloading vs method overriding. Which one is resolved at compile-time and which at runtime?**

**Answer:**

* **Overloading** means defining multiple methods in the same class with the same name but different parameters. It's resolved at **compile-time** (static binding).
* **Overriding** means redefining a superclass method in a subclass. It's resolved at **runtime** (dynamic binding) using **virtual method dispatch**.

Thus, polymorphism in Java truly happens during **method overriding**, not overloading.


---

### **17. Why is method overloading not considered polymorphism in strict OOP terms?**

**Answer:**
Because it's resolved at **compile-time**, not runtime — so technically it's not **true polymorphism** (dynamic binding).

---

### **18. When should you use an abstract class vs. an interface?**

**Use abstract class:**

* You want to share code (state or behavior).
* You want to evolve the base class without breaking children.
* Abstract class could have method implementations and state.

**Use interface:**

* You want multiple inheritance.
* You want to define only contract (capabilities).
* Interfaces can have **default methods**, **static methods**, and **private methods**.
* However, interfaces **cannot** have instance variables (state), constructors, or enforce access modifiers other than `public`.

---

### **19. What is a marker interface? Give an example.**

**Answer:**
An interface with **no methods**, used to mark classes for some behavior.

Example: `Serializable`, `Cloneable`

```java
public class MyClass implements Serializable { }
```

---

### **20. What is method hiding in Java?**

**Answer:**
If a subclass defines a **static method** with the same signature as a static method in the superclass, it hides the method, it does **not override**.

---

### **21. Can we override final, static, or private methods?**

**Answer:**

* **final**:  Cannot override.
* **static**:  Hidden, not overridden.
* **private**:  Not visible in subclass — no overriding.

---

### **22. What’s the difference between shallow copy and deep copy? How would you implement deep cloning in Java?**

**Answer:**

* **Shallow copy** copies object references. Nested objects still refer to the same memory.
* **Deep copy** creates entirely new instances, recursively duplicating nested objects.

**Shallow copy example:** `Object.clone()` by default is shallow.
**Deep copy implementation:** You need to manually clone nested objects or use serialization libraries.

```java
public class Person implements Cloneable {
    Address address;

    @Override
    protected Person clone() {
        Person clone = (Person) super.clone();
        clone.address = new Address(this.address); // deep clone
        return clone;
    }
}
```

Or use libraries like **Apache Commons Lang SerializationUtils.clone()**, if the object is serializable.

---

### **23. How would you prevent inheritance in your Java class design?**

**Answer:**
To prevent inheritance:

* Mark the class as `final`.
* Declare constructors as `private` or `protected` and use factory methods.
* Use `sealed` classes (Java 15+) to restrict which classes can inherit.

```java
public final class Utils {
    private Utils() {} // Prevents instantiation
}
```

---

### **24. What is upcasting? Why is it used in Java, and how does it relate to polymorphism?**

**Answer:**
**Upcasting** is casting a subclass object to a superclass reference type. It’s **implicit**, safe, and enables **runtime polymorphism**.

### Example:

```java
class Animal {
    void speak() { System.out.println("Animal speaks"); }
}

class Dog extends Animal {
    void speak() { System.out.println("Dog barks"); }
    void wagTail() { System.out.println("Dog wags tail"); }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog(); // Upcasting
        a.speak(); // Output: Dog barks — runtime polymorphism
        // a.wagTail(); //  Compilation error — not visible in Animal
    }
}
```

**Use Case:**
Upcasting is used when the exact type is unknown but common behavior is required — often in collections, factory methods, and polymorphic APIs.

---

### **25. What is downcasting? Is it safe? When should you use it?**

**Answer:**
**Downcasting** is casting a superclass reference **back to subclass type**. It’s **explicit** and potentially unsafe if not verified.

### Risk:

```java
Animal a = new Animal();
Dog d = (Dog) a; //  ClassCastException at runtime
```

### Safe downcasting with `instanceof`:

```java
Animal a = new Dog();

if (a instanceof Dog) {
    Dog d = (Dog) a; //  Safe
    d.wagTail();
}
```

---

### **26. How does upcasting enable runtime polymorphism in Java?**

**Answer:**
When you upcast a subclass object to a superclass reference, **Java uses the actual object’s implementation** (not the reference type) for overridden methods — this is **dynamic dispatch**.

---

### **27. What are the SOLID principles? How do they relate to Java OOP?**

**Answer:**

* **S** – Single Responsibility Principle: A class should have only one reason to change.
* **O** – Open/Closed Principle: Classes should be open for extension, closed for modification.
* **L** – Liskov Substitution Principle: Subclasses must behave like their parent classes.
* **I** – Interface Segregation Principle: No client should depend on methods it doesn't use.
* **D** – Dependency Inversion Principle: High-level modules should not depend on low-level modules. Use abstractions.

These principles are crucial for writing **maintainable, testable, and scalable** Java code. They are applied through **design patterns**, **interface-based design**, and **layered architecture**.

---

## 

### **28. Can you access subclass-specific methods after upcasting? Or, If we upcast a subclass object to a superclass reference, how do we access subclass methods?**

**Answer:**
You can’t — unless you **downcast** back to subclass.

```java
Animal a = new Dog();
a.wagTail(); // Compile error

((Dog) a).wagTail(); // Works, but ensure instanceof before casting
```

---

### **29. What happens if downcasting fails? Or, What happens if you downcast an object that isn't actually an instance of the subclass?**

**Answer:**
It compiles, but throws a **`ClassCastException`** at runtime.

```java
Animal a = new Animal(); // not a Dog

Dog d = (Dog) a; //  Runtime error
```

---

### **30. Given the following code, what is the output?**

```java
class Animal {
    void makeSound() { System.out.println("Animal"); }
}

class Cat extends Animal {
    void makeSound() { System.out.println("Cat"); }
    void purr() { System.out.println("Purr"); }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Cat();  // Upcasting
        a.makeSound();         // ???
        ((Cat) a).purr();      // ???
    }
}
```

**Answer:**

* `a.makeSound();` → Output: `Cat` (dynamic method dispatch)
* `((Cat) a).purr();` → Output: `Purr` (safe downcast)

---

### Real-world Analogy

**Upcasting:**

> You’re treating a **Dog** like an **Animal** — you can feed it or call `speak()`, but you don’t know it can `wagTail()` unless you explicitly treat it like a Dog again.

**Downcasting:**

> You say: “I know this Animal is a Dog, so let me cast it back and call `wagTail()`.” If you're wrong — boom!  `ClassCastException`.

> "Whenever I need to downcast, I ensure type safety using `instanceof` to avoid `ClassCastException`."
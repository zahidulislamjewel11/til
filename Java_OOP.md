## Java OOP Questions

**Question 1:**
*Can a Java class implement multiple interfaces with the same method signature but different return types?*

**Answer:**
No. This will cause a compilation error because Java uses method signatures (name + parameters) for overloading, and return type is not part of the signature.

---

**Question 2:** 
*What happens if a class implements two interfaces with default methods having the same signature?*

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

**Question 3:** 
*Can you override a private or static method in Java?*

**Answer:**

* **Private methods** are not visible to subclasses → **not overridden**, they are simply hidden.
* **Static methods** can be re-declared in subclass → **method hiding**, **not overriding**.

---

**Question 4:**
*Why is Java not 100% Object-Oriented?*

**Answer:**
Because it has **primitive types** like `int`, `char`, `boolean`, etc. which are not objects. But with autoboxing (`int` ↔ `Integer`), this is abstracted.

---

**Question 5:** 
*What is the difference between abstraction and encapsulation with real examples?*

**Answer:**

* **Abstraction**: Hiding *implementation details*, exposing only essential behavior.
   *Example*: `List list = new ArrayList();` — You don’t care how `add()` is implemented.

* **Encapsulation**: Binding data and methods together + restricting direct access via `private` fields and `getters/setters`.

---

**Question 6:** 
*What is object slicing in Java?*

**Answer:**
Java doesn't have object slicing like C++. But similar problems can occur when downcasting without type checks, or when a subclass-specific field is ignored if treated as a superclass.

---

**Question 7:** 
*What are covariant return types in Java?*

**Answer:**
Java allows overriding methods to return a more specific type (covariant).

```java
class Parent {
    Number show() { return 1; }
}

class Child extends Parent {
    Integer show() { return 1; } // Allowed
}
```

---

**Question 8:** 
*What is the difference between composition and aggregation?*

**Answer:**

* **Composition**: Strong association. Lifespan of contained object is tied.
*Example*: A `Car` **has-a** `Engine`. Engine doesn't exist without Car.

* **Aggregation**: Weak association. Contained object can exist independently.
    *Example*: A `Department` has `Students`.

---

**Question 9:** 
*Can constructor be overridden in Java?*

**Answer:**
No. Constructors are **not inherited**, hence **cannot be overridden**. But you can **overload** them within the same class.

---

**Question 10:** 
*Is "new" always required to create an object in Java?*

**Answer:**
No. Objects can also be created via:

* **Deserialization**
* **Cloning**
* **Reflection**
* **Factory methods (e.g., `valueOf`)**

---

**Question 11:** 
*Explain the Liskov Substitution Principle (LSP) with Java code.*

**Answer:**
LSP says: A subclass should be substitutable for its superclass **without breaking behavior**.

```java
class Bird {
    void fly() { }
}

class Ostrich extends Bird {
    void fly() {
        throw new UnsupportedOperationException(); // violates LSP
    }
}
```

**Fix**: Don't put `fly()` in `Bird`. Use interfaces or redesign hierarchy.

---

**Question 12:** 
*Can abstract classes have constructors? What’s their use?*

**Answer:**
Yes. Abstract classes can have constructors. They're called **when subclass constructors are invoked**, to **initialize inherited fields**.

---

**Question 13:** 
*What is the diamond problem? How does Java handle it?*

**Answer:**
Occurs in multiple inheritance (two classes have same method and a subclass inherits both). Java avoids this by **not allowing multiple class inheritance**. But with interfaces (default methods), Java requires **explicit resolution**.

---

**Question 14:** 
*What's the difference between instanceof and getClass()?*

**Answer:**

* `instanceof` returns `true` for instances of subclasses too.
* `getClass()` checks **exact type**.

```java
SubClass obj = new SubClass();
System.out.println(obj instanceof SuperClass); // true
System.out.println(obj.getClass() == SuperClass.class); // false
```

---

**Question 15:** 
*How does Java achieve runtime polymorphism?*

**Answer:**
Using **method overriding** and **dynamic method dispatch**.

```java
Animal a = new Dog();
a.makeSound(); // Dog’s version is called at runtime
```

---

**Question 16:** 
*Why is method overloading not considered polymorphism in strict OOP terms?*

**Answer:**
Because it's resolved at **compile-time**, not runtime — so technically it's not **true polymorphism** (dynamic binding).

---

**Question 17:** 
*When should you use an abstract class vs. an interface?*

**Use abstract class:**

* You want to share code (state or behavior).
* You want to evolve the base class without breaking children.

**Use interface:**

* You want multiple inheritance.
* You want to define only contract (capabilities).

---

**Question 18:** 
*What is a marker interface? Give an example.*

**Answer:**
An interface with **no methods**, used to mark classes for some behavior.

Example: `Serializable`, `Cloneable`

```java
public class MyClass implements Serializable { }
```

---

**Question 19:** 
*What is method hiding in Java?*

**Answer:**
If a subclass defines a **static method** with the same signature as a static method in the superclass, it hides the method, it does **not override**.

---

**Question 20:** 
*Can we override final, static, or private methods?*

**Answer:**

* **final**:  Cannot override.
* **static**:  Hidden, not overridden.
* **private**:  Not visible in subclass — no overriding.

---

**Question 21:**
*What is upcasting? Why is it used in Java, and how does it relate to polymorphism?*

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

**Question 22:**
*What is downcasting? Is it safe? When should you use it?*

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

**Question 23:**
*How does upcasting enable runtime polymorphism in Java?*

**Answer:**
When you upcast a subclass object to a superclass reference, **Java uses the actual object’s implementation** (not the reference type) for overridden methods — this is **dynamic dispatch**.

---

## 

**Question 24:**
*Can you access subclass-specific methods after upcasting?*
or
*If we upcast a subclass object to a superclass reference, how do we access subclass methods?*

**Answer:**
You can’t — unless you **downcast** back to subclass.

```java
Animal a = new Dog();
a.wagTail(); // Compile error

((Dog) a).wagTail(); // Works, but ensure instanceof before casting
```

---

**Question 25:**
*What happens if downcasting fails?*
or
*What happens if you downcast an object that isn't actually an instance of the subclass?*

**Answer:**
It compiles, but throws a **`ClassCastException`** at runtime.

```java
Animal a = new Animal(); // not a Dog

Dog d = (Dog) a; //  Runtime error
```

---

**Question 26:**
*Given the following code, what is the output?*

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

## Real-world Analogy

**Upcasting:**

> You’re treating a **Dog** like an **Animal** — you can feed it or call `speak()`, but you don’t know it can `wagTail()` unless you explicitly treat it like a Dog again.

**Downcasting:**

> You say: “I know this Animal is a Dog, so let me cast it back and call `wagTail()`.” If you're wrong — boom!  `ClassCastException`.

---

## Summary Table

| Aspect       | Upcasting                     | Downcasting                            |
| ------------ | ----------------------------- | -------------------------------------- |
| Direction    | Subclass → Superclass         | Superclass → Subclass                  |
| Safety       |  Always safe                 |  Risky, must check with `instanceof` |
| Syntax       | Implicit                      | Explicit                               |
| Purpose      | Enable polymorphism           | Access subclass-specific methods       |
| Polymorphism | Yes (method overriding works) | No — used after polymorphism           |

---

## Final Tip for Interviews

Always say:

> "Whenever I need to downcast, I ensure type safety using `instanceof` to avoid `ClassCastException`."
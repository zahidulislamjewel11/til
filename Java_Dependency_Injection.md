## Java Dependency Injection (DI) Interview Questions

### 1. **What is Dependency Injection? Why is it useful?**

**Question:**
*What is Dependency Injection, and how does it help in application design?*

**Answer:**
**Dependency Injection** is a design pattern used to remove hard-coded dependencies and make code loosely coupled, easier to test, and maintainable.

Instead of a class **creating its dependencies**, the dependencies are **injected** externally — typically by a framework like Spring.

**Before DI (Tight coupling):**

```java
class Service {
    private Repository repo = new Repository(); // tightly coupled
}
```

**With DI (Loose coupling):**

```java
class Service {
    private final Repository repo;

    public Service(Repository repo) { // injected via constructor
        this.repo = repo;
    }
}
```

**Relation to DIP:**
This follows the **Dependency Inversion Principle**:

> "High-level modules should not depend on low-level modules. Both should depend on abstractions."

In DI, we depend on interfaces, not concrete classes.

**Bonus Tip:**
Mention **IoC (Inversion of Control)** container and how Spring handles DI behind the scenes.

---

### 2. **How does DI follow the Dependency Inversion Principle (DIP)?**

**Question:**
*Can you explain the link between Dependency Injection and the Dependency Inversion Principle?*

**Answer:**
Absolutely.

* **DIP** says: high-level modules (e.g., `OrderService`) shouldn’t depend on low-level modules (e.g., `StripePaymentGateway`); both should depend on **abstractions** (`PaymentGateway` interface).
* **DI** enables this by injecting the concrete implementation **behind an interface**, ensuring the service doesn't create it manually.

```java
interface PaymentGateway { void pay(); }

@Component
class StripePaymentGateway implements PaymentGateway {
    public void pay() { /* logic */ }
}

@Component
class OrderService {
    private final PaymentGateway gateway;

    public OrderService(PaymentGateway gateway) { // DIP via DI
        this.gateway = gateway;
    }
}
```

So, **DI is the implementation technique**, and **DIP is the principle**.

---

### 3. **What are the types of Dependency Injection in Spring?**

**Question:**
*What different ways can dependencies be injected into beans in Spring Framework?*

**Answer:**
Spring supports 3 types of DI:

1. **Constructor Injection** – preferred for immutability and mandatory dependencies.
2. **Setter Injection** – good for optional dependencies.
3. **Field Injection** – less recommended (harder to test).

```java
@Component
class Car {
    private final Engine engine;

    @Autowired
    public Car(Engine engine) { // Constructor Injection
        this.engine = engine;
    }
}
```

**Bonus Tip:**
Prefer **constructor injection** in modern Spring (especially for testing and final fields).

---

### 4. **Constructor vs Setter Injection: Which should you use and when?**

**Question:**
*In Spring, when should you use constructor injection vs setter injection?*

**Answer:**

* **Constructor Injection** is **recommended** when the dependency is **mandatory**. It ensures immutability and guarantees all dependencies are initialized.

```java
@Component
class OrderService {
    private final PaymentGateway gateway;

    public OrderService(PaymentGateway gateway) {
        this.gateway = gateway;
    }
}
```

* **Setter Injection** is good for **optional dependencies**.

```java
@Component
class NotificationService {
    private EmailSender emailSender;

    @Autowired
    public void setEmailSender(EmailSender emailSender) {
        this.emailSender = emailSender;
    }
}
```

**Bonus Tip:** In Spring Boot (v4.3+), if a class has only one constructor, `@Autowired` is optional.

---

### 5. **Why is field injection discouraged in Spring?**

**Question:**
*Why do many experts discourage field injection in favor of constructor injection?*

**Answer:**
**Field injection is discouraged** because:

* You **can’t make fields `final`**
* It’s **hard to test** — can’t inject mocks easily
* **No visibility** of required dependencies
* **Constructor injection** makes dependencies **explicit**

**Field Injection (Not Recommended):**

```java
@Component
class MyService {
    @Autowired
    private Repository repo;
}
```

**Constructor Injection (Preferred):**

```java
@Component
class MyService {
    private final Repository repo;

    public MyService(Repository repo) {
        this.repo = repo;
    }
}
```

---

### 6. **How does Spring Framework handle Dependency Injection?**

**Question:**
*How does Spring handle Dependency Injection under the hood?*

**Answer:**
Spring uses the **Inversion of Control (IoC) Container** to manage beans.

* Spring scans for `@Component`, `@Service`, `@Repository`, etc.
* It creates instances and resolves dependencies via:

  * **Constructor Injection** (preferred)
  * **Setter Injection**
  * **Field Injection** (discouraged for testing)

```java
@Component
class CarService {
    private final Engine engine;

    @Autowired
    public CarService(Engine engine) {
        this.engine = engine;
    }
}
```

Spring injects the `Engine` bean automatically when creating `CarService`.

---

### 7. **How is DI achieved in Spring without using `new` keyword?**

**Question:**
*In your project, you never use `new` to create objects. How does Spring handle this?*

**Answer:**
Spring uses an **IoC container** to **scan**, **instantiate**, and **inject** dependencies automatically using annotations like `@Component`, `@Autowired`, and `@Configuration`.

```java
@Component
class Engine {}

@Component
class Car {
    @Autowired
    private Engine engine; // Injected by Spring
}
```

Spring **manages object lifecycles**, reducing boilerplate code.

---

### 8. **How does DI help with Unit Testing?**

**Question:**
*How does Dependency Injection make unit testing easier?*

**Answer:**
DI allows you to **inject mock implementations** instead of real ones.
With DI, you can inject **mock or fake dependencies** during testing.

```java
@Test
void testOrder() {
    PaymentGateway mockGateway = mock(PaymentGateway.class);
    OrderService service = new OrderService(mockGateway);

    service.placeOrder();

    verify(mockGateway).pay(); // easy to verify
}
```

Without DI, you’d be stuck with concrete implementations hardcoded inside the service, which makes testing and mocking much harder.

This avoids tight coupling and makes **unit testing easy and isolated**.

---

### 9. **Can you give a real-world analogy of Dependency Injection?**

**Question:**
*Give me a simple analogy to explain Dependency Injection to a junior developer.*

**Answer:**
Imagine you go to a restaurant.

* Instead of **cooking** your own meal (creating your own dependency), you’re **served** food (dependency is injected).
* The chef (Spring Container) knows how to prepare the dish and gives it to you when needed.

This way, you focus on **eating** (business logic), not **preparing** the food (object creation).
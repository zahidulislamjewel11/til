
## Java Design Pattern Questions

### 1. **Singleton Pattern** (Creational)

**Question:**
*You're designing a logging utility for an enterprise app that writes to a central log file. How would you ensure only one logger instance exists in the entire application?*

**Answer:**
Use the **Singleton pattern**. This ensures that only one instance of the logger exists across the JVM.

```java
public class Logger {
    private static final Logger instance = new Logger();
    private Logger() {} // private constructor

    public static Logger getInstance() {
        return instance;
    }

    public void log(String message) {
        // write to file
    }
}
```

**Bonus Tip:**
Mention **thread safety**, **eager vs lazy initialization**, and **enum singleton** for robustness.

---

### 2. **Factory Method Pattern** (Creational)

**Question:**
*You're building a notification service. Depending on user preference, you need to send Email, SMS, or Push notifications. How would you design this using a design pattern?*

**Answer:**
Use the **Factory Method Pattern** to encapsulate object creation logic.

```java
interface Notification {
    void send(String message);
}

class EmailNotification implements Notification {
    public void send(String message) { /* email logic */ }
}

class SmsNotification implements Notification {
    public void send(String message) { /* sms logic */ }
}

class NotificationFactory {
    public static Notification getNotifier(String type) {
        return switch (type.toLowerCase()) {
            case "email" -> new EmailNotification();
            case "sms" -> new SmsNotification();
            default -> throw new IllegalArgumentException("Unknown type");
        };
    }
}
```

---

### 3. **Strategy Pattern** (Behavioral)

**Question:**
*You're developing a payment system that supports Credit Card, PayPal, and UPI. How would you design it so that new payment methods can be added with minimal changes?*

**Answer:**
Use the **Strategy Pattern** to encapsulate each payment algorithm.

```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy { ... }
class PayPalPayment implements PaymentStrategy { ... }

class CheckoutContext {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }

    public void checkout(double amount) {
        paymentStrategy.pay(amount);
    }
}
```

---

### 4. **Observer Pattern** (Behavioral)

**Question:**
*You’re building a stock market app. When the stock price updates, multiple UI elements and alert systems should update too. How would you design this?*

**Answer:**
Use the **Observer Pattern** where observers are automatically notified of changes in the subject (stock).

```java
interface Observer {
    void update(float price);
}

class Stock {
    private List<Observer> observers = new ArrayList<>();
    private float price;

    public void attach(Observer obs) { observers.add(obs); }

    public void setPrice(float newPrice) {
        this.price = newPrice;
        for (Observer o : observers) {
            o.update(price);
        }
    }
}
```

---

### 5. **Decorator Pattern** (Structural)

**Question:**
*You’re creating a pizza ordering system where customers can choose toppings like cheese, pepperoni, etc. How would you design it to add toppings dynamically?*

**Answer:**
Use the **Decorator Pattern** to layer additional functionality (toppings) onto a base object (Pizza).

```java
interface Pizza {
    String getDescription();
    double getCost();
}

class BasicPizza implements Pizza {
    public String getDescription() { return "Basic Pizza"; }
    public double getCost() { return 5.0; }
}

class CheeseDecorator implements Pizza {
    private Pizza pizza;
    public CheeseDecorator(Pizza p) { this.pizza = p; }
    public String getDescription() { return pizza.getDescription() + ", Cheese"; }
    public double getCost() { return pizza.getCost() + 1.5; }
}
```

---

### 6. **Command Pattern** (Behavioral)

**Question:**
*You're implementing an undo/redo system in a text editor. How can you encapsulate actions (like insert, delete) and allow undoing them later?*

**Answer:**
Use the **Command Pattern** to encapsulate each action as an object with `execute()` and `undo()` methods.

```java
interface Command {
    void execute();
    void undo();
}

class InsertTextCommand implements Command {
    private String text;
    public void execute() { /* insert logic */ }
    public void undo() { /* remove inserted text */ }
}

class CommandManager {
    Stack<Command> history = new Stack<>();
    public void executeCommand(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }

    public void undoLast() {
        if (!history.isEmpty()) history.pop().undo();
    }
}
```

---

### 7. **Adapter Pattern** (Structural)

**Question:**
*You need to integrate a third-party payment gateway with a completely different interface from your application. How would you adapt it without changing existing code?*

**Answer:**
Use the **Adapter Pattern** to convert the interface of the third-party class into one your app expects.

```java
interface PaymentGateway {
    void pay(double amount);
}

class ThirdPartyPayAPI {
    void makePayment(double amt) { /* payment logic */ }
}

class PayAdapter implements PaymentGateway {
    private ThirdPartyPayAPI api = new ThirdPartyPayAPI();
    public void pay(double amount) {
        api.makePayment(amount);
    }
}
```

---

## Bonus Interview Tips:

* Always mention **SOLID principles** when explaining patterns.
* Understand **when not to use** a pattern — overengineering is a red flag.
* Use **real-world analogies**:

  * Strategy: “Different routes in Google Maps”
  * Observer: “YouTube notifications”
  * Decorator: “Extra toppings on pizza”

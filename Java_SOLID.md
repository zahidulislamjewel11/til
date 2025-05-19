## Java SOLID Interview Questions

---

### 1. **Single Responsibility Principle (SRP)**

**Question:**
*You have a class that handles user registration, sends confirmation emails, and logs activities. What principle is violated, and how would you refactor it?*

**Answer:**
This violates the **Single Responsibility Principle**. A class should have **only one reason to change**, meaning it should handle a **single concern**.

**Before (SRP Violation):**

```java
public class UserService {
    public void register(User user) {
        // save user to DB
        // send confirmation email
        // log registration
    }
}
```

**After (SRP Compliant):**

```java
public class UserService {
    private EmailService emailService;
    private AuditService auditService;

    public void register(User user) {
        saveToDb(user);
        emailService.sendConfirmation(user);
        auditService.log("User registered: " + user.getUsername());
    }
}
```

**Insight:**
By splitting the responsibilities, you improve **testability**, **reusability**, and **maintainability**. Changes in email logic won't risk breaking user registration.

---

### 2. **Open/Closed Principle (OCP)**

**Question:**
*You're developing a report generator. You initially support PDF reports, but now Excel and CSV formats are needed. How would you extend the functionality without modifying existing code?*

**Answer:**
Follow the **Open/Closed Principle** — classes should be **open for extension, but closed for modification**.

**Bad Approach (violates OCP):**

```java
public class ReportGenerator {
    public void generate(String type) {
        if (type.equals("PDF")) { /* generate PDF */ }
        else if (type.equals("Excel")) { /* generate Excel */ }
        // more conditions coming...
    }
}
```

**Better Approach (OCP compliant - use polymorphism):**

```java
interface Report {
    void generate();
}

class PdfReport implements Report {
    public void generate() { /* PDF logic */ }
}

class ExcelReport implements Report {
    public void generate() { /* Excel logic */ }
}

class ReportService {
    public void process(Report report) {
        report.generate();
    }
}
```

**Insight:**
You can now introduce `CsvReport`, `HtmlReport`, etc., **without modifying `ReportService`**, just **extending** behavior.

---

### 3. **Liskov Substitution Principle (LSP)**

**Question:**
*What happens if you substitute a subclass and it breaks the behavior of the parent class? How does it violate LSP?*

**Answer:**
**Liskov Substitution Principle** states: **Objects of a superclass should be replaceable with objects of its subclasses without altering the correctness of the program**.

**Violation Example:**

```java
class Bird {
    void fly() { /* flying logic */ }
}

class Ostrich extends Bird {
    void fly() { throw new UnsupportedOperationException(); }
}
```

Now, substituting `Bird` with `Ostrich` in any function expecting a flying bird breaks the program.

**Fix via composition:**

```java
interface Bird { }
interface FlyingBird extends Bird {
    void fly();
}

class Parrot implements FlyingBird {
    public void fly() { /* fly */ }
}

class Ostrich implements Bird {
    // doesn't implement fly
}
```

**Insight:**
Design your inheritance hierarchy based on **behavioral compatibility**, not just shared data or taxonomy. Violating LSP often results in **runtime errors and broken expectations**.

---

### 4. **Interface Segregation Principle (ISP)**

**Question:**
*You have an interface with 10 methods, but implementing classes only use 2 of them. How would you fix this?*

**Answer:**
That violates the **Interface Segregation Principle**. Clients shouldn't be forced to depend on methods they do not use.

**Bad Example:**

```java
interface Worker {
    void code();
    void test();
    void deploy();
    void writeDocumentation();
}
```

If a `Tester` class only uses `test()`, it’s burdened with irrelevant methods.

**Better Approach:**

```java
interface Developer {
    void code();
}

interface Tester {
    void test();
}

interface Deployer {
    void deploy();
}
```

**Insight:**
Breaking interfaces into smaller, **role-specific contracts** results in **cleaner**, more **cohesive**, and **flexible** code.

---

### 5. **Dependency Inversion Principle (DIP)**

**Question:**
*Your class directly instantiates low-level modules like `MySqlDatabase` or `SmtpMailer`. What principle is violated, and how would you decouple the dependencies?*

**Answer:**
This violates the **Dependency Inversion Principle**, which states that **high-level modules should not depend on low-level modules. Both should depend on abstractions**.

**Bad Approach:**

```java
public class NotificationService {
    private SmtpMailer mailer = new SmtpMailer(); // tightly coupled

    public void send(String msg) {
        mailer.sendEmail(msg);
    }
}
```

**Better (DIP via constructor injection):**

```java
interface Mailer {
    void sendEmail(String msg);
}

class SmtpMailer implements Mailer {
    public void sendEmail(String msg) { /* logic */ }
}

class NotificationService {
    private Mailer mailer;

    public NotificationService(Mailer mailer) {
        this.mailer = mailer;
    }

    public void send(String msg) {
        mailer.sendEmail(msg);
    }
}
```

**Insight:**
DIP promotes **inversion of control**, allowing easier **unit testing**, **loose coupling**, and **runtime flexibility** (like plugging in `AwsSesMailer`).

---

### Final Thoughts:

**Interview Tips When Explaining SOLID:**

* Don’t just explain the theory — **use practical code samples**.
* Emphasize **how each principle improves maintainability, scalability, and testability**.
* Relate SOLID to design patterns. For instance:

  * SRP and Strategy pattern go hand-in-hand.
  * OCP is implemented via Factory or Decorator patterns.
  * DIP is the foundation of Dependency Injection frameworks like **Spring**.
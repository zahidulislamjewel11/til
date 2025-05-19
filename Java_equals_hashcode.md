### **Interview Question:**

Explain the purpose and use of the `equals()` and `hashCode()` methods in Java. When and why should they be overridden? Why is it important to override both together? What are the consequences of omitting one or the other? Also, explain their significance in the context of collections like `Map`, `HashMap`, and `HashSet`.

---

### **Answer:**

In Java, `equals()` and `hashCode()` are methods inherited from the root class `Object`. They play a fundamental role in determining object equality and behavior in hash-based collections such as `HashSet` and `HashMap`.

---

### **1. Purpose of `equals()`**

The `equals()` method defines the **logical equality** between two objects. The default implementation in the `Object` class checks for reference equality — whether two references point to the exact same object in memory.

However, in most practical scenarios, we want to determine equality based on the actual content or state of objects, not just their memory addresses. For example, two `Employee` objects with the same ID and name should be considered equal logically, even if they are distinct instances.

By overriding `equals()`, a class defines its criteria for logical equality.

#### **Example:**

```java
class Employee {
    private int id;
    private String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;                  // same reference
        if (obj == null || getClass() != obj.getClass()) return false;
        Employee other = (Employee) obj;
        return id == other.id && name.equals(other.name);
    }
}
```

---

### **2. Purpose of `hashCode()`**

The `hashCode()` method returns an integer hash code representation of the object. It is primarily used by hash-based collections such as `HashMap`, `HashSet`, and `Hashtable` to **efficiently store and retrieve objects**.

The contract between `equals()` and `hashCode()` is critical:

* If two objects are equal according to `equals()`, they **must return the same hash code**.
* If two objects have the same hash code, they are **not necessarily equal** (hash collisions can occur).

Overriding `hashCode()` ensures objects that are logically equal produce the same hash code, which is vital for proper functioning of hash-based collections.

#### **Example:**

```java
@Override
public int hashCode() {
    return Objects.hash(id, name);
}
```

---

### **3. Why Override Both `equals()` and `hashCode()` Together?**

If you override only one of these methods and not the other, it breaks the general contract:

* **If only `equals()` is overridden but not `hashCode()`:**
  Two logically equal objects might produce different hash codes. In hash-based collections, this causes unexpected behavior such as failing to find the object in a `HashSet` or failing to retrieve a value from a `HashMap` even though an equal key exists.

* **If only `hashCode()` is overridden but not `equals()`:**
  The collection might treat unequal objects as equal if their hash codes collide, leading to logical errors or duplication.

Therefore, overriding both consistently is essential for correct equality comparison and proper storage/retrieval in hash-based collections.

---

### **4. Consequences of Omitting `equals()` or `hashCode()`**

* **Omitting `equals()` override:**
  Equality defaults to reference equality (`==`), so two objects with identical content but different references are treated as different objects.

* **Omitting `hashCode()` override:**
  Hash code falls back to the default implementation, which typically depends on the object's memory address. This results in logically equal objects producing different hash codes, breaking the contract with `equals()` and causing failures in hash-based collections.

---

### **5. Usage in Context of `Map`, `HashMap`, and `HashSet`**

* **Hash-based Collections** use `hashCode()` to determine the **bucket location** where the object or key-value pair should be stored.

* After locating the bucket, the collection uses `equals()` to compare keys or elements for equality within that bucket.

* If `hashCode()` and `equals()` are not properly overridden:

  * Objects that are logically equal may end up in different buckets, leading to duplicate entries or failed retrieval.
  * Searches, insertions, and deletions become unreliable and inconsistent.

#### **Example with `HashSet`:**

```java
Set<Employee> employees = new HashSet<>();

Employee e1 = new Employee(1, "Alice");
Employee e2 = new Employee(1, "Alice");

employees.add(e1);
employees.add(e2);

System.out.println(employees.size());  // Should print 1 if equals and hashCode are overridden properly
```

If `equals()` and `hashCode()` are not overridden consistently, the above code might incorrectly print `2`, meaning the set considers them different objects, even though logically they are the same.

---

### **Summary**

* Override `equals()` to define **logical equality** between objects.
* Override `hashCode()` to maintain **hash code consistency** for logically equal objects.
* Always override both methods together to fulfill the general contract and ensure correct behavior in hash-based collections.
* Proper overriding guarantees that collections like `HashSet` and `HashMap` correctly identify duplicates, perform efficient lookups, and maintain integrity.

Understanding and correctly implementing these methods is fundamental for Java developers, especially when working with collections or designing classes used as keys or elements in sets and maps.
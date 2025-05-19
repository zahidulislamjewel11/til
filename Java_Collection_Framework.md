## Java Collection Framework Questions

### **1. What is the difference between `ArrayList` and `LinkedList` in Java?**

**Answer:**

* `ArrayList` uses a dynamic array, while `LinkedList` uses a doubly linked list.
* Access time: `ArrayList` provides O(1) for random access; `LinkedList` is O(n).
* Insert/Delete: `LinkedList` performs better for insertions/deletions in the middle of the list (O(1) if node is known), whereas `ArrayList` may require shifting (O(n)).
* `LinkedList` consumes more memory due to node pointers.

---

### **2. Why is `HashSet` faster than `TreeSet`?**

**Answer:**

* `HashSet` is backed by a `HashMap` and offers constant-time performance (O(1)) for basic operations.
* `TreeSet` is backed by a `TreeMap` (a Red-Black tree), and provides log(n) time for operations, as it maintains elements in sorted order.

---

### **3. What happens if you insert a duplicate key in a `HashMap`?**

**Answer:**
The old value associated with the key is replaced by the new value. The key remains unique.

---

### **4. How does `HashMap` work internally?**

**Answer:**

* Uses an array of `Node<K,V>`.
* The key's hashCode is processed through `hash()` function.
* The hash is mapped to an index in the array.
* If a collision occurs, a linked list (or tree after threshold) handles it.
* Java 8+ uses tree (Red-Black Tree) when number of elements in a bucket exceeds 8 and array size is more than 64.

---

### **5. Why should you override `equals()` and `hashCode()` when using objects as keys in a `HashMap`?**

**Answer:**
Because:

* `hashCode()` determines the bucket location.
* `equals()` determines key equality in that bucket.
  If not overridden properly, you can have incorrect or unexpected behavior (e.g., duplicates or key not found).

---

### **6. Can you store null in a `HashMap`?**

**Answer:**

* Yes. `HashMap` allows **one null key** and **multiple null values**.
* `Hashtable` does **not** allow null keys or values.

---

### **7. What is the difference between `HashMap` and `ConcurrentHashMap`?**

**Answer:**

* `HashMap` is **not thread-safe**.
* `ConcurrentHashMap` is **thread-safe** using segment locks (Java 7) or synchronized blocks (Java 8+).
* It doesn’t allow null keys or null values.
* ConcurrentHashMap is designed for high concurrency.

---

### **8. What is fail-fast vs fail-safe in collections?**

**Answer:**

* **Fail-fast**: Throws `ConcurrentModificationException` if structure is modified while iterating (e.g., `ArrayList`, `HashMap`).
* **Fail-safe**: Allows modification during iteration without throwing exceptions (e.g., `CopyOnWriteArrayList`, `ConcurrentHashMap`) — uses a separate copy.

---

### **9. What is the time complexity of key operations in `HashMap` and `TreeMap`?**

**Answer:**

* `HashMap`: O(1) for `put()`, `get()`, and `remove()` in ideal case; O(n) in worst case.
* `TreeMap`: O(log n) for all key-based operations.

---

### **10. How does `CopyOnWriteArrayList` work?**

**Answer:**

* On each write (add/remove), it **creates a new copy** of the underlying array.
* It is thread-safe and ideal for scenarios with **more reads and fewer writes**.
* Iterators do not throw `ConcurrentModificationException`.

---

### **11. When would you prefer `LinkedHashMap` over `HashMap`?**

**Answer:**

* When you need predictable **iteration order** (insertion order or access order).
* Useful in implementing **LRU cache** by overriding `removeEldestEntry()`.

---

### **12. How is `TreeSet` implemented? Can it store duplicate elements?**

**Answer:**

* Internally uses a `TreeMap`.
* Does **not** allow duplicates.
* Elements must be **Comparable** or you must provide a **Comparator**.

---

### **13. Difference between `Iterator` and `ListIterator`?**

**Answer:**

* `Iterator`: Unidirectional (only forward), applicable to all collections.
* `ListIterator`: Bidirectional (forward and backward), only for List implementations.
* `ListIterator` can add elements; `Iterator` cannot.

---

### **14. How is the `Collections.synchronizedList()` different from `CopyOnWriteArrayList`?**

**Answer:**

* `synchronizedList()` is a wrapper; operations are synchronized on the list object — potential bottleneck.
* `CopyOnWriteArrayList` offers better concurrency performance by copying on write.

---

### **15. What is the difference between `Comparable` and `Comparator`?**

**Answer:**

* `Comparable`: Defines **natural ordering** of a class. Implemented by the class itself (`compareTo()` method).
* `Comparator`: Defines **custom ordering**. Separate class with `compare()` method.

---

### **16. Why is `EnumSet` so fast?**

**Answer:**

* It’s backed by a bit vector representation.
* Since enums are constants and known at compile-time, operations like `add()`, `contains()` are extremely efficient.

---

### **17. Can you make a collection immutable?**

**Answer:**
Yes. Using `Collections.unmodifiableXXX()` or `List.of()`, `Set.of()` (Java 9+).

```java
List<String> list = Collections.unmodifiableList(new ArrayList<>());
```

---

### **18. How does `PriorityQueue` work?**

**Answer:**

* Implemented using a **heap** (binary heap).
* Elements are ordered based on **natural ordering** or a provided **Comparator**.
* Offers O(log n) for insertion and removal.

---

### **19. Why is `Hashtable` considered obsolete?**

**Answer:**

* It is synchronized on every method, leading to performance issues.
* Replaced by `ConcurrentHashMap` for thread-safe scenarios and `HashMap` for single-threaded use.

---

### **20. What is the difference between structural and reference equality in collections?**

**Answer:**

* Structural equality (`equals()`): Compares contents.
* Reference equality (`==`): Compares memory addresses.
  `Set.contains(x)` uses `equals()` to check if an element exists.


Great decision to revise the Java Collection Framework — it's one of the most commonly tested areas in Java interviews, especially for 2+ years of experience. Below are **some of the trickiest and most crucial Java Collection Framework questions**, along with **elaborate explanations and answers**, aimed at making you interview-ready.

---

### 31. **What is the difference between `HashMap`, `LinkedHashMap`, and `TreeMap`? When should you use each?**

* **HashMap** is an implementation of the `Map` interface that stores key-value pairs with no ordering. It allows one null key and multiple null values. It is the fastest among the three for insert and lookup operations but **does not maintain any order**.

* **LinkedHashMap** maintains a **doubly linked list** running through all its entries. This allows it to maintain **insertion order**, or access order if constructed with `accessOrder=true`. It's slightly slower than `HashMap` due to the added overhead of maintaining order.

* **TreeMap** implements `NavigableMap` and stores keys in a **sorted order (natural ordering or comparator-based)**. It is based on a **Red-Black Tree**, hence operations like `put()`, `get()` are **O(log n)** compared to `O(1)` in HashMap.

**Use cases:**

* Use `HashMap` for fastest access without ordering.
* Use `LinkedHashMap` when you need predictable iteration order.
* Use `TreeMap` when you need sorted keys or range-based operations (like `subMap()`).

---

### 32. **Why are `HashSet` elements unordered, and how does it ensure uniqueness?**

`HashSet` internally uses a `HashMap` to store elements. When you add an element to a `HashSet`, it is actually added as a key to the internal `HashMap` with a dummy constant value.

Uniqueness is enforced because:

* Keys in a `HashMap` must be unique.
* Hashing + equals logic ensures that duplicates are not stored.

The order is **not preserved** because:

* HashMap uses the hashCode of the key to determine the bucket location.
* Bucket position is not tied to insertion order and may change due to rehashing.

Hence, no predictable order is maintained.

---

### 33. **What will happen if the `hashCode()` is overridden but not the `equals()` method, or vice versa?**

The `hashCode()` and `equals()` methods must be **consistent** with each other when used in hash-based collections like `HashMap`, `HashSet`.

* If you **override `hashCode()` but not `equals()`**, two objects with the same content may end up in the same bucket but still be treated as different due to default `equals()` behavior (which uses reference equality).

* If you **override `equals()` but not `hashCode()`**, logically equal objects might end up in **different buckets**, making `contains()` or `get()` fail unexpectedly.

This **breaks the contract** and leads to subtle and hard-to-find bugs in hash-based collections.

---

### 34. **What is the difference between `ArrayList` and `LinkedList`?**

Both implement `List`, but have different internal structures:

* **ArrayList** uses a dynamic array. It offers:

  * Fast random access (`get(index)` is O(1))
  * Slow inserts/removals in the middle (`O(n)` due to shifting)

* **LinkedList** uses a doubly linked list. It offers:

  * Slow random access (`O(n)` for `get(index)`)
  * Fast inserts/removals at the beginning/middle (`O(1)` if node reference is known)

**When to use what:**

* Use `ArrayList` for frequent read and occasional add/remove at the end.
* Use `LinkedList` for frequent insertions/removals in the middle or from both ends (also implements `Deque`).

---

### 35. **Why is `ConcurrentModificationException` thrown and how to avoid it?**

This exception occurs when a collection is **structurally modified** (add/remove) while iterating **using iterator**, except through the iterator’s own `remove()` method.

It is a **fail-fast behavior** implemented to prevent unpredictable behavior due to concurrent modifications.

**Avoid it by:**

* Using iterator’s `remove()` method instead of collection’s `remove()`.
* Using concurrent collections like `CopyOnWriteArrayList` or `ConcurrentHashMap`.
* Using a standard `for` loop (with index) on `ArrayList` instead of iterator.

---

### 36. **What is the difference between `fail-fast` and `fail-safe` iterators?**

* **Fail-fast**: Throws `ConcurrentModificationException` if collection is modified while iterating.

  * Examples: Iterators of `ArrayList`, `HashMap`.

* **Fail-safe**: Works on a **clone of the collection**, so modifications don’t affect iteration.

  * Examples: Iterators of `CopyOnWriteArrayList`, `ConcurrentHashMap`.

Fail-safe iterators do **not guarantee real-time consistency**, but they **do not throw exceptions** on modification.

---

### 37. **What is the difference between `Comparable` and `Comparator`?**

* **Comparable** is used to define **natural ordering** of objects.

  * Implemented by the class itself via `compareTo()` method.
  * Example: `String`, `Integer` implement `Comparable`.

* **Comparator** is used to define **custom ordering**.

  * Implemented separately via `compare()` method.
  * Passed to methods like `Collections.sort()` or to `TreeMap`.

Use `Comparable` when default ordering makes sense across all contexts. Use `Comparator` when ordering is dynamic or context-specific.

---

### 38. **How does `HashMap` work internally?**

* Each entry in a `HashMap` is stored in a **bucket**.
* The bucket index is calculated from the key’s `hashCode()` using bitwise operations.
* Collisions are handled using:

  * **Linked list chaining** (before Java 8)
  * **Balanced tree (Red-Black Tree)** when number of items in a bucket > 8 (from Java 8 onwards)

Steps:

1. Key’s `hashCode()` is computed and used to find the bucket index.
2. If bucket is empty, the entry is inserted.
3. If not, it checks using `equals()` if key already exists.
4. If yes, value is updated. If not, new entry is added.

---

### 39. **Why is `ConcurrentHashMap` preferred over `HashTable` in multithreaded code?**

* `Hashtable` is **synchronized on every method**, which creates **a performance bottleneck**.
* `ConcurrentHashMap` uses **bucket-level locking (segment or bin-level)**. Only a part of the map is locked during write operations, allowing **higher concurrency**.

From Java 8 onwards, it uses **lock-free algorithms** (CAS - Compare And Swap) and synchronized blocks only when absolutely needed.

So, it is more efficient and scalable in concurrent scenarios.

---

### 40. **What are `WeakHashMap` and `IdentityHashMap`? When should you use them?**

* **WeakHashMap**:

  * Keys are stored using **weak references**.
  * If no strong references to a key exist, it can be **garbage collected**, and the entry will be removed.
  * Used for memory-sensitive caches (e.g., metadata cache for classes or images).

* **IdentityHashMap**:

  * Uses `==` instead of `equals()` to compare keys.
  * Treats two keys as the same **only if they are the same object in memory**.
  * Used for special scenarios like **object reference tracking**.

These maps are not suitable for general-purpose key-value storage but are important for edge cases.

---

### 41. **What are the differences between `Collections` and `Collection`?**

* `Collection` is the **root interface** in the collection hierarchy (List, Set, Queue all extend it).
* `Collections` is a **utility class** with static methods like `sort()`, `shuffle()`, `unmodifiableList()` etc.

A common confusion due to the similar names. Think of `Collection` as a **type**, and `Collections` as a **helper class**.

---

### 42. **Why is `EnumSet` highly efficient compared to other sets?**

* `EnumSet` is specifically designed for use with enum types.
* Internally implemented using a **bit vector**, so operations like `add`, `remove`, `contains` are **extremely fast**.
* It is **memory-efficient** and outperforms `HashSet` or `TreeSet` when dealing with enums.

However, it can only be used when the key type is an `enum`.


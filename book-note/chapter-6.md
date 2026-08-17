## 6. Different Layer, Different Abstraction 
### 6.1. Pass-Through Methods

A **pass-through method** is a method that performs no functionality of its own other than calling another method with a similar or identical signature (Figure a).

=> It increases interface complexity without increasing the overall functionality of the system and creates unnecessary dependencies between classes.

**Refactoring solutions:**
  - Allow callers to access the lower-level class directly and completely remove that responsibility from the upper-level class (Figure b).
  - Redistribute functionality between classes to avoid methods calling through multiple layers (Figure c).
  - Merge classes together if their responsibilities cannot be separated cleanly (Figure d).

### 6.2. When Is Interface Duplication Appropriate?

Having methods with the same signature is not necessarily bad design, as long as each method contributes important and distinct functionality. Duplication becomes problematic when it creates **pass-through methods** that do not provide any new functionality.

**Some legitimate use cases:**
  - **Dispatcher:** A method that uses its arguments to select and invoke one of several other methods, passing most or all of the arguments to the selected method. Although a dispatcher often has the same signature as the methods it calls, it still provides useful functionality by filtering and routing work (for example, a URL dispatcher in a web server).

  - **Multiple implementations:** For example, disk drivers in an operating system. Each driver supports a different type of disk but follows the same interface. This reduces cognitive load for programmers because once they understand how one method works, they can easily work with other methods that have the same interface without learning everything from scratch. These methods are typically located at the same level and do not call one another.

### 6.3. Decorator
The **Decorator pattern** (or "wrapper") is an object that wraps around another object to extend its functionality. It provides an API that is similar or identical to the original object. Decorator classes tend to be **shallow** because they introduce a large amount of boilerplate code while providing very little new functionality. They often contain many **pass-through methods**.

=> Overusing Decorators for every small feature can lead to an explosion of shallow classes, increasing the overall complexity of the system.

**Alternatives to consider before using a Decorator:**
  - Integrate the new functionality directly into the original class if the feature is general-purpose or needed by most use cases.
  - If the functionality is highly specialized for a particular use case, integrate it directly into the code for that use case instead of creating a new class.
  - Integrate the new functionality into an existing Decorator to create a deeper class rather than creating many shallow Decorators.
  - Design the new functionality as a completely independent class that is unrelated to and does not wrap the existing class.

### 6.4. Pass-Through Variables

A **pass-through variable** is a variable that is continuously passed through a long chain of intermediate methods before reaching the method that actually needs to use it.

**Consequences:**
  - It increases system complexity because all intermediate methods are forced to know about the variable's existence, even though they do not use it themselves.
  - It makes maintenance more difficult: if a new variable needs to be passed down to a lower layer, the programmer may have to modify the interfaces and signatures of many intermediate methods along the path.

**Solutions proposed by the author:**
  - **Shared object:** Check whether there is already an object shared between the first and last methods. If so, store the information in that object instead of passing it through all the intermediate methods.

  - **Global variable:** This avoids passing the variable through multiple methods, but it often introduces other problems, such as conflicts in multithreaded execution or difficulties during testing.

  - **Context object:** This is the solution the author uses most often. Create a context object that stores all global application state, such as configuration options and performance counters. References to this context object are stored in the system's major objects and appear primarily in constructors, effectively eliminating pass-through variables.

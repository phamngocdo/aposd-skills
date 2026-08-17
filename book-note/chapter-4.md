## 4. Information Hiding

### 4.1. Information Hiding

**Information hiding** is one of the most important techniques for creating **deep modules**. Each module should encapsulate certain knowledge or design decisions within its implementation and should not expose them through its interface.

Information that should be hidden includes **algorithms, complex data structures, implementation details**, and other internal design decisions.

=> Reduces **cognitive load**.

=> Makes the system easier to develop and modify without affecting external modules.

### 4.2. Information Leakage

A design decision is reflected across multiple modules.

=> This creates **dependencies between modules**.

**Signs of Information Leakage:** Information can leak directly through the interface, making the interface more complicated, or indirectly through **back-door leakage**.

**Example:** Two different classes may need to know about and process the same file format, even though that format is supposed to be an internal implementation detail.

**Refactoring Information Leakage:** If information leakage is discovered between classes, the system should be refactored in one of two ways:

- **Combine the classes** into a single class if they are small and closely related to the leaked information.

- **Separate the information** and create a new class to encapsulate it independently. This is effective only if the new class provides a simple interface that abstracts away its internal details.

**General Solution:** Instead of focusing primarily on the **order in which tasks are performed** when designing modules, developers should focus on the **amount of knowledge required to perform each task** in order to achieve information hiding.

Core mechanisms that share the same flow of information should be grouped together in the same module.

### 4.3. A Use Case for Information Hiding

#### General Context: HTTP Server

Building classes for processing the HTTP protocol requires receiving requests and sending responses in text form. The protocol consists of a start line, headers, and a body.

#### Problem 1 — Temporal Decomposition Causes Information Leakage

**Problem:** The process of receiving a request is divided into two separate classes based on the order of execution: one class reads raw data into a string, while another class parses the syntax.

This causes information leakage because the reading class still needs to understand part of the request structure, such as the `Content-Length` header, in order to know when it has finished reading.

The caller also experiences increased cognitive load because it must call the two classes in the correct order.

**Solution:** Combine them into a single class that handles both reading and parsing the data.

Group all source code related to the same specific capability in one place. This hides implementation details and creates a **deep class** with a simple interface.

#### Problem 2 — A Shallow Interface Exposes Internal Storage Structure

**Problem:** The `HTTPRequest` class provides a public method such as:

`public Map<String, String> getParams()`

This returns the entire internal map data structure.

This exposes the internal representation and forces callers to search through the data, perform type conversions themselves, and potentially risk modifying the object's internal state.

**Solution:** Hide the internal data structure behind specific methods such as:

`getParameter(String name)`

The class can also provide methods that automatically convert common data types, such as:

`getIntParameter(String name)`

This reduces the amount of boilerplate code that callers need to write.

#### Problem 3 — The API Lacks Reasonable Default Values

**Problem:** The caller is forced to manually provide required information when creating a response object, such as the HTTP version, which should normally match the request, or the `Date` header.

This causes information leakage and unnecessarily increases the developer's cognitive load.

**Solution:** Design the interface around the **common case**.

Classes should automatically "do the right thing" by providing reasonable default values, such as automatically taking the HTTP version from the request and generating the current timestamp.

Apply information hiding partially: hide these fields and provide separate override methods only for rare situations where customization is actually required.

## 4.4. Information Hiding Without Using Classes

**Designing private methods:** Developers should design private methods so that each method encapsulates a specific piece of information or capability and completely hides it from the rest of the class.

**Minimize variable scope:** Try to minimize the number of locations where each instance variable is used.

**Reduce internal dependencies:** Although some variables may necessarily need to be accessed widely, limiting the number of places where other variables are used helps eliminate unnecessary dependencies within the class, thereby reducing the overall complexity of the class itself.

**Information hiding is valuable only when the information is unnecessary to external users.** The goal is to minimize the amount of information that external code needs to know. If a module can automatically configure or adjust itself, it does not necessarily need to expose that configuration to the outside. However, developers must carefully determine **which information is actually necessary for external users** before deciding what to hide.
# Note from book 'Aphilosophy of software design"
 
---
## Table of contents
- [1. The Nature of Complexity](#1-the-nature-of-complexity)
- [2. Working Code Isn't Enough](#2-working-code-isnt-enough)
- [3. Module Design](#3-module-design)
- [4. Information Hiding](#4-information-hiding)
- [Summary of Design Principles](#summary-of-design-principles)
---

## 1. The Nature of Complexity

### 1.1. Concept of Complexity

**Complexity** is anything related to the structure of a software system that makes the system difficult to understand and modify.

=> The more complex a system is, the harder it is to modify, even when implementing a small feature.

**Complexity is determined by the activities that are performed frequently.**

Some parts of a system may be complex, but if they are rarely touched or modified, they do not have a significant impact on the overall complexity of the system.

### 1.2. Symptoms of Complexity

**Change Amplification:** Small changes in the code require modifications in many different places.

**Example:** Changing the background color requires modifying multiple pages.

=> **Solution:** Define a single background-color variable and have all pages use that variable. Then, the color only needs to be changed in one place.

**Cognitive Load:** A symptom of software complexity that refers to the amount of information a programmer must know and keep in mind in order to complete a specific task.

The higher the cognitive load
=> The more important information the developer has to keep track of
=> The easier it is to introduce bugs or overlook important parts of the system.
**Note:** Sometimes, adding more code can actually help reduce cognitive load if it makes the system easier to understand.

**Unknown Unknowns:** It is unclear which parts of the code need to be modified to complete a task, or what information a developer needs in order to complete the task successfully.

=> This is the **worst form of complexity** because developers do not even know what they do not know.

**Example:** Some pages on a website use a variable called **emph** to control the background shadow. Normally, a developer only needs to change **bannerBg** to change the background color. However, they may not realize that **emph** also needs to be changed on each page to keep the design consistent. The developer may not even know that the **emph** variable exists. Even if they notice it, they still have to search through the codebase to find every place where this variable is used, which wastes time.

### 1.3. Causes of Complexity

**Dependencies:** To modify one piece of code, other related pieces of code also need to be modified. The component cannot be changed independently.

=> The goal is to **reduce dependencies** and make the remaining dependencies as simple as possible.

**Obscurity:** Important information is not clearly expressed or is difficult to discover. For example, variable names may be unclear, documentation may be too vague, or the relationship between different components may not be obvious.

=> The goal is to **simplify the system's design** and make important information more explicit.

---

## 2. Working Code Isn't Enough

**Tactical Programming:** Programming with the goal of making the product work as quickly as possible.
=> Focuses on completing the task quickly without considering the long-term consequences or future changes.

**Strategic Programming:** Prioritizes the long-term structure and maintainability of the code rather than simply making the code work.

=> Requires an **investment mindset**: looking for clean and sustainable solutions when designing the system.

=> May take more time in the short term, but increases development speed and productivity in the long term.

**How much time should be invested in software design?**

  - **Do not invest everything upfront:** The traditional **Waterfall** approach demonstrates that trying to fully design the system from the beginning is often ineffective because requirements and understanding evolve over time.

  - **Make small, continuous investments:** Good design should gradually emerge through iterative development. Continuously improve the design as you gain a better understanding of the system.

  - **Spend around 10–20% of development time on design and improvement:** This is small enough not to significantly slow down development, while being large enough to provide substantial long-term benefits.

---

## 3. Module Design

### 3.1. Module Design

**Definition:** Module design is the process of decomposing a software system into relatively independent components.Because modules need to interact with each other, **dependencies** arise between them.

=> The goal is to **minimize these dependencies**.

A module consists of two main components:

- **Interface:** Contains the information necessary to use the module.

- **Implementation:** The internal details of how the module is built. Users of the module do not need to know these details.

The best modules are **deep modules**: they provide a simple interface while hiding a large and complex implementation.

### 3.2. Interface

An interface consists of two types of information:

- **Formal information:** Information explicitly specified in the source code and checked by the programming language, such as function names, data types, and public variables.

- **Informal information:** High-level behavioral information that cannot be checked by the programming language. This often describes complex behavior that can only be documented through comments or other documentation.

### 3.3. Abstraction

**Definition:** Abstraction is a simplified view of an entity that deliberately ignores unimportant details. In module design, each **interface** is an abstraction.

**Criteria for Good Abstraction**: The more unimportant details an abstraction can hide, the better the abstraction is.

Two Common Design Mistakes

- **Including unimportant details:** This increases complexity and the cognitive load on developers.

- **Omitting important details (leaky or incorrect abstraction):** This creates obscurity and prevents developers from having enough information to use the module correctly.

**Core Principle**: The key to designing good abstractions is to understand **what information is important** and look for designs that minimize the amount of important information that must be exposed.

=> A **deep module** provides powerful functionality through a simple interface, making it easy for other developers to use.

### 3.4 Classitis

**Classitis** is the belief that **"the more classes, the better."**

It leads to excessive decomposition of methods and classes, creating a large number of **shallow modules**.

=> This increases overall system complexity rather than reducing it.

---
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

---

## Summary of Design Principles
1. Complexity is incremental: you have to sweat the small stuff.
2. Working code isn’t enough.
3. Make continual small investments to improve system design.
4. Modules should be deep.
5. Interfaces should be designed to make the most common usage as simple as possible.
6. It’s more important for a module to have a simple interface than a simple implementation.
7. General-purpose modules are deeper.
8. Separate general-purpose and special-purpose code.
9. Different layers should have different abstractions.
10. Pull complexity downward.
11. Define errors (and special cases) out of existence.
12. Design it twice.
13. Comments should describe things that are not obvious from the code.
14. Software should be designed for ease of reading, not ease of writing.
15. The increments of software development should be abstractions, not features.
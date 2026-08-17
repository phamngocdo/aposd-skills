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
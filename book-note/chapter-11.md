## 11. Why Write Comments? The Four Excuses

### 11.1. The Role of Documentation in Source Code (In-code Documentation)

**Helps developers understand and work effectively:** Comments help developers understand the system quickly and work more accurately.

**Plays a core role in abstraction:** The goal of abstraction is to hide complexity. Without comments, developers are forced to read the code to understand how it works. This completely breaks the abstraction and exposes all the underlying complexity of the system.

**A tool for improving design:** The process of writing comments, when done properly, can help developers identify inappropriate design decisions and directly improve the system's design.

### 2. Four Common Excuses Developers Use to Avoid Writing Comments

Many developers skip writing comments or consider them a tedious task. They commonly justify this with the following four reasons:

**Excuse 1: "Good code is self-documenting":**
  - *Reality:* This is largely a myth. Code can express formal aspects such as a method signature (name, data types), but informal aspects (what the method does, the meaning of its return value, and the conditions under which it should be called) can only be fully described using natural language, such as comments.

  - *Consequence:* If developers expect users to read the code to understand it, they tend to break the code into many extremely short methods. This creates a large number of **shallow methods**, which increases the overall complexity of the system.

  - *Example:* Consider a substring extraction method with two parameters, `start` and `end`. From the method declaration alone, it is impossible to know whether the character at position `end` is included, or what happens when `start > end`. A comment written in natural language can complete this abstraction by explaining the intended behavior.

**Excuse 2: "I don't have time to write comments":**
  - *Reality:* Skipping comments is often the result of a **tactical mindset**. With an **investment mindset**, writing good comments is a long-term investment that improves maintainability and saves time in the future.

  - *Cost analysis:* The time spent typing code accounts for only around 10% of the total development time, which also includes design, testing, compilation, debugging, and other activities. Even if writing comments took as much time as writing code, it would increase the initial development time by at most around 10%. The benefits gained later can quickly outweigh this cost.

**Excuse 3: "Comments will become outdated and misleading when the code changes":**
  - *Reality:* Keeping comments up to date does not require excessive effort if proper design principles are followed, such as avoiding redundant comments and placing comments close to the code they describe. Out-of-date comments can also be identified relatively easily during the **code review** process.

**Excuse 4: "All the comments I've ever seen are worthless, so why bother?":**
  - *Reality:* This is the most reasonable excuse because much of the existing documentation is indeed mediocre. However, writing good comments is not difficult when developers understand the right methodology and mindset.

### 11.3. Specific Benefits of Good Comments in Reducing Complexity

Complex systems typically exhibit three symptoms: **Change Amplification**, **Cognitive Load**, and **Unknown Unknowns**. Good comments directly help address the latter two:

- **Reduce Cognitive Load:** Provide exactly the information developers need to make a change, allowing them to ignore irrelevant information without having to read large amounts of source code and infer the necessary information themselves.

- **Reduce Unknown Unknowns:** Clarify the structure and behavior of the system, helping developers know which pieces of code or information are directly relevant to the change they are about to make.

- **Clarify Dependencies and Eliminate Obscurity:** Fill in the information gaps that cannot be expressed by the programming language itself.
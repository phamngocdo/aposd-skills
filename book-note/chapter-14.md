## 14. Write The Comments First

### 14.1. The Cost of Delaying Comments

Programmers often wait until the code is stable before writing documentation. This makes it easy to forget writing comments altogether.

Even if they eventually write them, the quality is often poor because the design decisions are no longer fresh in their memory. As a result, comments tend to merely repeat what is already obvious from the source code.

### 14.2. A Process for Writing Comments Early

1. For a new class, start by writing comments for the **class interface**.

2. Write the interface and signatures for the most important public methods, but leave their method bodies empty.

3. Revise these comments until the basic structure of the class feels reasonable.

4. Declare and write comments for the most important **instance variables** of the class.

5. Fill in the method bodies with implementation code, adding **implementation comments** when necessary.

**Note**: Whenever you discover the need for a new variable or method, write its comment **before** writing the code.

### 14.3. Comments as a Design Tool

The most important benefit of writing comments first is that they help improve the system's design. They allow programmers to evaluate and refine abstractions before writing the implementation code.

Comments act as a **"canary in a coal mine"**—an early warning sign of complexity.

**Red Flag — Hard to Describe:** If a method or variable requires a very long and complicated comment to describe it fully, it may indicate that you have not found a good abstraction or that the interface is **shallow**.

=> Writing comments first helps identify and fix these design problems early.

### 14.4. Does Writing Comments Early Cost More?

The argument that comments should be delayed to avoid the cost of updating documentation when the code changes is not accurate.

The time spent writing code and comments accounts for at most around **10% of the total development time**, so updating comments only represents a small fraction of the overall development effort (around **5%**).

On the contrary, writing comments first helps stabilize abstractions before writing the code. This can save time spent implementing code and reduce the number of times the source code needs to be modified compared with a code-first approach.
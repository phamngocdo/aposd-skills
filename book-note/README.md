# Note from book 'Aphilosophy of software design"
 
---
## Table of contents
- [1. The Nature of Complexity](./chapter-1.md)
- [2. Working Code Isn't Enough](./chapter-2.md)
- [3. Module Design](./chapter-3.md)
- [4. Information Hiding](./chapter-4.md)
- [5. General-Purpose Modules are Deeper](./chapter-5.md)
- [6. Different Layer, Different Abstraction ](./chapter-6.md)
- [7. Pull Complexity Downwards](./chapter-7.md)

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

---
## Summary of Red Flags

Here are a few of of the most important red flags discussed in this book. The presence of any of these symptoms in a system suggests that there is a problem with the system’s design:
1. **Shallow Module**: the interface for a class or method isn’t much simpler than its implementation.
2. **Information Leakage**: a design decision is reflected in multiple modules.
3. **Temporal Decomposition**: the code structure is based on the order in which operations are executed, not on information hiding.
4. **Overexposure**: An API forces callers to be aware of rarely used features in order
to use commonly used features.
5. **Pass-Through Method**: a method does almost nothing except pass its arguments to another method with a similar signature.
6. **Repetition**: a nontrivial piece of code is repeated over and over.
7. **Special-General Mixture**: special-purpose code is not cleanly separated from general purpose code.
8. **Conjoined Methods**: two methods have so many dependencies that its hard to understand the implementation of one without understanding the implementation of the other.
9. **Comment Repeats Code**: all of the information in a comment is immediately obvious from the code next to the comment.
10. **Implementation Documentation Contaminates Interface**: an interface comment describes implementation details not needed by users of the thing being documented.
11. **Vague Name**: the name of a variable or method is so imprecise that it doesn’t convey much useful information.
12. **Hard to Pick Name**: it is difficult to come up with a precise and intuitive name for an entity.
13. **Hard to Describe**: in order to be complete, the documentation for a variable or method must be long.
14. **Nonobvious Code**: the behavior or meaning of a piece of code cannot be understood easily.
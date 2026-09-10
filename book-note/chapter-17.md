## 17. Code Should be Obvious

### 17.1. Keeping Code Obvius

**Definition of "Obvius":** Code is obvius when another person can quickly skim through it without having to think too much, while their first impression of the code's behavior and meaning is still completely accurate. Clarity exists in the **reader's mind**, not the writer's. If someone reviewing the code says that it is not obvius, then it is actually not obvius.

Techniques for Making Code More Obvius

- **Choose good names and maintain consistency:** (Covered in Chapters 13 and 16).

- **Use whitespace appropriately:** Format the code and insert blank lines to separate major logical blocks within a method.

  - **Example of whitespace:** Cramming document parameters tightly together makes them difficult to read. Adding whitespace and proper alignment immediately makes the structure more obvius and easier to scan.

    Similarly, a line of code such as:

    `for (int pass = 1; pass >= 0 && !empty; pass--)`

    is much easier to read when appropriate whitespace is used rather than cramming everything together.

- **Compensating comments:** When you are forced to write code that cannot be made sufficiently obvius, use comments to provide the missing information needed by the reader.

---

### 17.2. Things That Make Code Less Obivius

Event handler functions are called indirectly through function pointers or interfaces, making it difficult for readers to follow the application's control flow.

=> Add obvius comments at the function's interface to explain when and under what circumstances the function will be triggered.

Using classes such as `Pair` in Java or `std::pair` in C++ can reduce code clarity because the elements are accessed through generic names such as `result.getKey()` or `result.getValue()`.

=> Spend some time defining a dedicated class or structure (`struct`) with meaningful, specific field names, such as `totalChars`.

Declaring one type but allocating another. For example, declaring a variable as `List` but assigning an `ArrayList` to it. This can cause readers to misunderstand the variable's performance characteristics or thread-safety properties.

Code that violates the reader's expectations, **Example:** A typical `main` function is expected to terminate the application when it finishes. However, if `main` calls a constructor such as `new RaftClient(...)`, which implicitly creates background threads, the application may continue running even after `main` has finished. This unexpected behavior makes the code harder to understand. The programmer must add a comment at the end of `main` warning the reader about this special behavior.
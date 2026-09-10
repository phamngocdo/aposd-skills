## 8. Better Together Or Better Apart? 
### 8.1. Together or Apart?

**The problem:** When there are two different pieces of functionality, the question is whether they should be designed together in one place (combined) or separated at all levels (functions, classes, services).

=> **Design goal:** Minimize the overall complexity of the system and improve modularity.

**The consequences of excessive subdivision:** Trying to split a system into extremely small components can create new complexity by itself:

  - **Increasing the number of components:** This makes the system harder to navigate and search, while also creating more interfaces.

  - **More complex source code:** The code that calls these components has to take on additional logic to manage and connect multiple objects at the same time.

  - **Creating physical separation:** Code is moved farther apart, such as into different classes or files. If the pieces have dependencies on each other, developers have to switch back and forth between files, making it easier to miss dependencies and introduce bugs.

  - **Creating code duplication:** Code that only needed to exist in one place before the separation may now have to be duplicated across multiple smaller components.

**Signs that pieces of code are related and should be kept together:**
  - They share common information (for example, they both depend on the syntax of a particular type of document).
  - They are used together in both directions (users of one will also use the other, and vice versa).
  - They overlap conceptually (for example, substring searching and case conversion are both types of "string operations").
  - It is difficult to understand one piece of code without looking at the other.

### 8.2. Three Main Reasons to Bring Code Together

**Bring together if information is shared:**

  **Example:** An HTTP server project separates reading a request from a socket and parsing that request into two methods in two different classes, even though the operations occur sequentially. This creates information leakage because the reading method cannot determine when the request ends without parsing the header lines to obtain the request length.

  => **Solution:** Combining the reading and parsing operations in one place makes the code shorter and simpler.

**Bring together if it will simplify the interface:**

  When modules are combined, intermediate processing steps can be hidden, creating a simpler and easier-to-use interface.

  **Example:** If the Java I/O library combined `FileInputStream` and `BufferedInputStream` into a default buffered class, most users would no longer need to know about the buffering mechanism. The resulting interface would be shallower and more concise.

**Bring together to eliminate duplication:**
  - **Extract a method:** If the same piece of code is repeated multiple times, extract it into a separate method, provided that the code is long enough and the new method has a simple signature. If the snippet is too short or interacts too heavily with local variables, extracting it may actually increase complexity because of a more complicated method signature.

  - **Restructure control flow:** Use a `goto` statement (in languages that support it) to consolidate repeated cleanup code from multiple nested error-handling branches into a single location at the end of the function.

  - **Warning sign — Repetition:** If the same or nearly identical code appears repeatedly, it is a sign that you may not have found the right abstraction.

### 8.3. Separate General-Purpose and Special-Purpose Code

- A lower-level module should provide only a **general-purpose mechanism**. **Special-purpose code** that serves a particular use case of the application should be pushed to higher layers.

- **Warning sign — Special-General Mixture:** This occurs when a general-purpose mechanism is embedded with code specific to a particular use case. This makes the mechanism more complex and causes information leakage. Future changes to the use case will then force modifications to the underlying general-purpose mechanism.

### 8.4. Real-World Examples of Separating / Combining Code

**Insertion Cursor and Selection:**
  - **Problem:** One group combined the insertion cursor and selection into a single object because they frequently occur together when clicking or dragging the mouse. The result was a bulky object. Higher-level code still had to treat them as two separate entities when manipulating them and had to use complicated indirect Boolean variables to determine which end of the selection the cursor was positioned at.

  - **Solution:** Separate them and instead define a general-purpose `Position` class consisting of `(line number, character number)`. A selection can be represented by two `Position` objects, while a cursor can be represented by a single `Position`. This makes both the interface and implementation simpler.

**Separate Class for Logging:**
  - **Problem:** A programmer creates a `NetworkErrorLogger` class containing extremely shallow and short methods (only one line of code), such as `logRpcOpenError` and `logRpcSendError`. This forces readers to switch back and forth between files to understand what the logging functions actually do and increases the number of unnecessary interfaces.

  - **Solution:** Remove the class and place the logging statements directly where the errors are detected.

**Editor Undo Mechanism:**
  - **Correct design:** Separate the general-purpose core (`History`, which manages the list of actions, undo, redo, and grouping actions using fences) from the special-purpose action handling.

  - External concrete classes, such as `Text` or `UI`, define their own specialized action objects, such as `UndoableInsert` and `UndoableSelection`, and add them to `History`. This allows the `History` mechanism to be reused across different types of applications without leaking application-specific information into it.

### 8.5. Separating and Combining Methods / Functions

**Do not split functions just because of their length:** Many developers apply rigid rules, such as "a function longer than 20 lines must be split." This can lead to too many shallow functions and increase system complexity. A long function containing several independent sequential blocks of code, or blocks that interact with each other in complex ways, may be better than splitting them and forcing readers to jump back and forth between functions to understand the flow.

**Split a function only when doing so creates cleaner abstractions:**

  - **Extract a completely independent subtask:** The reader of the parent function should not need to know how the child function works, and the reader of the child function should not need to know the context of the parent function.

  - **Split into two independent functions at the same level:** This is useful when the original function tries to do too many unrelated things, making its interface overly complex. The interfaces of the two new functions should be simpler than the original one, and callers should typically need to call only one of the two functions rather than always calling both in sequence.

**Warning sign — Conjoined Methods:** If two methods are physically separated, but you cannot understand or modify one without looking at the other, the design is likely flawed.
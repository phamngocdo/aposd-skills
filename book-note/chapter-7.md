## 7. Pull Complexity Downwards
### 7.1. Core Philosophy

**Carry the complexity:** When developing a new module, if you encounter complexity that cannot be avoided, the developer should handle it **internally within the module** rather than pushing it onto the users/callers.

**Benefit the majority:** A module usually has many more users than developers. Therefore, it is better for the developer to "suffer" once by handling the complexity internally and simplifying the experience for all users.

**Simple interface > simple implementation:** Having a simple interface is much more important than having a simple implementation.

### 7.2. The Common Mistake: Pushing Complexity Upward

- **Avoiding difficult problems:** Developers often tend to solve the easy parts and push the difficult parts onto others. For example, they may throw exceptions that higher-level code must handle, or define too many configuration parameters that force system administrators to figure out the appropriate values themselves.

- **Unpredictable consequences:** This approach increases the overall complexity factor, potentially forcing dozens of people to deal with a problem that could have been handled cleanly by a single person—the module's author.

### 7.3. Detailed Examples from the Chapter

#### Example 1: The Text Management Module of a GUI Editor

- **Wrong approach — pushing complexity upward:** Design a line-oriented interface with methods for reading, inserting, and deleting entire lines of text.

  This makes the implementation inside the module very easy, but the UI layer rarely operates on entire lines. For example, users typically type individual characters or select and cut/paste arbitrary portions of text.

  As a result, the UI code has to handle the complexity of splitting and joining lines itself.

- **Correct approach — pulling complexity downward:** Design a character-oriented interface that allows arbitrary strings to be inserted or deleted at arbitrary positions.

  The complexity of handling line splitting and joining is then completely encapsulated inside the text class, making the UI source code much simpler.

#### Example 2: Configuration Parameters

- **The downside of configuration:** Providing too many configuration parameters, such as buffer size or the number of retry attempts, is often an excuse to avoid making proper design decisions.

  Users or administrators usually have no way of knowing which values are optimal.

- **Solution — pulling complexity downward:** Instead of requiring users to configure the retry interval when packets are lost, a network protocol can measure actual response times and automatically calculate and dynamically adjust this value.

  Configuration parameters should be minimized, and the system should try to automatically determine reasonable default values whenever possible.

### 7.4. Limitations of the Principle — Avoid Taking It Too Far

Do not take the idea of pulling complexity downward to the extreme of putting all the functionality of an entire application into a single class. This would be meaningless and would create an even more complex design.

Complexity should be pulled downward only when:

  - The complexity is **closely related to the current responsibilities** of the class.
  - Doing so **significantly simplifies the source code** elsewhere in the application.
  - Doing so **simplifies the interface** of the class itself.

If pulling complexity downward accidentally introduces unrelated knowledge into the module—for example, putting UI logic into the underlying text-processing class—it causes **Information Leakage**.
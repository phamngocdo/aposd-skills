## 5. General-Purpose Modules are Deeper 

### 5.1. Make Classes Somewhat General-Purpose

When designing a new module, a good approach is to make its **implementation** satisfy the current requirements, while making its **interface** general enough to support multiple use cases. However, this should not be taken too far, as excessive generalization can make the module unnecessarily complex and difficult to use for the current requirements.

This approach helps create **simpler and deeper interfaces** compared to a special-purpose approach. It can also save development time in the future if the class is reused. Even when the class is only used for its original purpose, a general-purpose interface can still be better because of its simplicity.

### 5.2. A Use Case for Designing a General-Purpose Module

**Exercise context:** Students are asked to design an in-memory text management class for a GUI text editor, supporting features such as multiple windows, Undo/Redo, and text selection.

**A mistake: designing a special-purpose API:** Many student groups designed the text class API around specific features visible in the user interface (UI). They created separate methods such as `backspace(Cursor cursor)` (delete to the left), `delete(Cursor cursor)` (delete to the right), and `deleteSelection(Selection selection)` (delete the selected text).

- **Consequences:**
  - **Shallow methods:** This approach creates many shallow methods in the text class, with many methods being called in only one place. It increases the cognitive load for UI programmers because they have to learn and remember too many methods.
  - **Information leakage:** UI-specific concepts such as the backspace key and text selection leak into the text class. This reduces the independence between the modules, making future UI changes more likely to require modifications to the text class.

#### A More General-Purpose API:
**Generalizing the API:** Instead of designing the API around the UI, the text class API should be defined around core text manipulation capabilities. The text class only needs two methods to modify its data:
    `insert(Position position, String newText)` and `delete(Position start, Position end)`.

**Using an abstract data type:** Replace the `Cursor` type, which is tightly coupled to the UI, with a more general `Position` type representing a position in the file. The text class can also provide a `changePosition(Position position, int numChars)` method for moving a position.

**Benefits to the code:**
  - The UI source code may be slightly longer when calling these general-purpose methods (for example, to implement backspace or delete), but it is clearer and easier to understand. A UI programmer can look at the code and immediately see which characters are being deleted without having to inspect the text class implementation.

  - The system has less code overall because many specialized methods are replaced by a small number of general-purpose methods.

  - **Increased reusability:** A general-purpose text class can be reused in other applications, such as an application that automatically searches and replaces strings, instead of being limited to a GUI text editor.

#### Generality Leads to Better Information Hiding:
**Clearer module separation:** A general-purpose approach creates a stronger separation between the text class and the UI class. The text class does not need to know anything about UI function keys, providing better information isolation. New UI features can be added without requiring new supporting methods in the text class.

**Eliminating false abstractions:** The `backspace` method in the original design is a **false abstraction**. It attempts to hide information about which character is being deleted, but the UI programmer actually needs to know this detail. Hiding important information only creates **obscurity**. When a detail is important, it is often better to make it explicit and clear.

### 5.3. Questions to Ask Yourself When Designing

**Is the API simple enough?**

Reduce the number of methods to increase generality, but make sure that each method itself does not become overly complex.

**How many times can this method be used?**

If a method serves only a single situation, it is a warning sign that the method may be too specialized.

**Is the API easy to use for the current requirements?**

If the API is so general that you have to write a large amount of additional code at higher layers just to use it, the design has gone too far and is not effective.
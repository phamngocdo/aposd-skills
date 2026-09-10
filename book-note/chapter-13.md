## 13. Choosing Names

### 13.1. The Importance of Naming

Choosing names for variables, methods, and other entities is an often-underestimated aspect of software design. Good names act as a form of documentation that makes source code clearer and easier to understand, reduces mistakes, and minimizes the need for comments. 

On the other hand, poor names increase complexity and can easily lead to bugs. Since a system may contain thousands of variables, the cumulative effect of poor naming can have a significant impact on overall complexity.

  **Example:** The author once spent six months finding a serious bug in the Sprite distributed operating system because a variable named `block` was used for two different purposes: the physical block number on the disk and the logical block number within a file. A programmer accidentally used the variable containing the logical block number in a context that required the physical block number, causing data on the disk to be overwritten. If they had used precise names such as `fileBlock` and `diskBlock` from the beginning, this error would likely not have occurred.

### 13.2. Create an Image

The goal of naming is to create a clear mental image in the reader's mind about the nature of the entity being named, allowing them to infer what the entity refers to (and what it does not refer to) even when viewed in isolation.

Names are a form of abstraction. They should focus on the most important aspects of an entity while leaving out secondary details.

Names should generally be short, around 2–3 words, to avoid unnecessary verbosity.

### 13.3. Names Should Be Precise

Overly generic or ambiguous names are among the most common naming mistakes. They force readers to explore the code to figure out what the name actually means.

- **Example 1 (Too Generic):** Instead of using `getCount()` for a class that manages indexlets (`IndexletManager`), a more precise name would be `getActiveIndexlets()` or `numIndexlets`. This allows readers to understand immediately what is being counted without having to look up the documentation.

- **Example 2 (Ambiguous in GUI):** Using `x` and `y` to represent a character's position in a file can be confusing because they may be mistaken for pixel coordinates on the screen. Better names would be `charIndex` and `lineIndex`.

- **Example 3 (Ambiguous Boolean Variable):** A variable named `blinkStatus` does not clearly indicate what is blinking or what `true` and `false` mean. Replacing it with `cursorVisible` (`true` means the cursor is visible, `false` means it is hidden) is much more intuitive. Boolean variables should ideally be named as predicates.

- **Exception:** Short and generic names such as `i` and `j` are acceptable for loop variables when the loop is short and spans only a few lines.

- **Example of an Overly Specific Name:** A text-deletion method with an argument such as `void delete(Range selection)` is too specific because the method can delete any text range, not necessarily the currently selected range. The parameter should instead be named `range`.

### 13.4. Use Names Consistently

When variables are repeatedly used for the same purpose throughout a system (such as block numbers, IDs, etc.), choose one consistent name and use it everywhere. Consistency reduces cognitive load because readers can reuse knowledge they have already acquired.

  **Requirements:**

  - (1) Always use the same common name for the same purpose.
  - (2) Never use that name for a different purpose.
  - (3) The purpose represented by the name should be narrow enough that all variables using that name have the same behavior.

**Example:** If two variables of the same type are needed within a function (such as a source block and a destination block), keep the common name and add distinguishing prefixes:

```text
srcFileBlock
dstFileBlock
```
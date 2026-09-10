## 12. Comments Should Describe Things that Aren’t Obvious from the Code 

### 12.1. Core Principle

**Core principle:** The only reason to write a comment is that the programming language cannot capture everything the developer had in mind when writing the code. Comments should supplement information that readers—especially developers seeing the code for the first time—cannot easily infer from the surrounding source code.

**Establish conventions:** The team should agree on conventions for comment formatting and documentation styles (such as Javadoc, Doxygen, or godoc) to ensure consistency and make developers less likely to skip writing comments.

  The main categories of comments include:
  - **Interface**
  - **Data structure member**
  - **Implementation**
  - **Cross-module**

### 12.2. Don't Repeat the Code

A common mistake is writing comments that describe exactly what the code does at the same level of detail, or simply reusing words from variable/function names to form a sentence.

**Example:** A comment such as:

  `// Convert PARAMETER to TYPE`

  for a function: `private static Object downCastParameter(String parameter, String type)` is completely useless because the code and function name already make this clear.

  Instead, if we have: `textHorizontalPadding = 4` rather than writing: `// Horizontal padding of the text` it is more useful to write: `// Amount of space left on the left and right sides of each line of text, measured in pixels.`

### 12.3. Lower-Level Comments Add Precision

These comments are useful when declaring variables, parameters, or return values. Code declarations are often ambiguous, so comments should clarify details such as:
  - Units of measurement
  - Boundaries (whether endpoints are included)
  - The meaning of a `null` value
  - Invariants

  When writing comments for a variable, think about the **noun** (what the variable represents) rather than the **verb** (how the variable is being manipulated or changed).

**Example:** Instead of the vague comment:

  `// Current offset in respBuffer`

  write something more precise:

  `// Position in the buffer of the first object that has not yet been returned to the client.`

### 12.4. Higher-Level Comments Enhance Intuition

These comments are useful inside methods or large blocks of code. Developers often get too focused on implementation details. Higher-level comments should step back from those details and explain the overall purpose—what the code is trying to accomplish—or why the code is being executed.

**Example:** Instead of explaining each repeated condition in an `if` statement, a good comment could simply say: `// Try to insert the current hash key into an existing RPC to the target server that has not yet been sent.`

### 12.5. Interface Documentation

This is where the abstraction is defined. It must be completely separated from implementation comments.

  Good interface documentation should describe the function's behavior from the caller's perspective, including:
  - Parameter constraints
  - Side effects
  - Exceptions
  - Preconditions

**Red Flag:** Do not let implementation details contaminate the interface. Users should not be required to understand the internal algorithm or the names of internal network-calling functions in order to use the function.

### 12.6. Cross-Module Design Decisions

Some design decisions affect multiple classes or modules. For example, a network protocol may affect both the sender and the receiver. It can be difficult to find an appropriate place to put comments for these decisions.

**Solution:** If there is a clear central location, such as an error catalog represented by an `enum Status`, place the comment there and list all other files or locations that need to be updated accordingly.

  If there is no specific central location, create a centralized file called **designNotes**. Organize the decisions into clear sections.

  In the related code, use only a short reference such as:

  `// See the "Zombies" section in designNotes.`
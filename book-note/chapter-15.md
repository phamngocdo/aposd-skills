## 15. Modifying Existing Code

### 15.1. Always Stay Strategic

When modifying code, such as fixing a bug or adding a feature, programmers often tend to think tactically by looking for the smallest possible change that makes the system work. This approach creates special cases, tangled dependencies, and gradually degrades the overall system design.

*Instead, program strategically:* after making a change, the system's structure should be in an optimal state, as if it had been designed from the beginning to accommodate that change. Try to improve the system's design a little whenever you modify the source code.

### 15.2. Place Comments Close to the Code They Describe
When modifying code, programmers can easily forget to update comments, causing the documentation to become outdated. A good practice is to place comments as close as possible to the source code they describe, so that programmers will see them immediately when modifying the code.

**Example:** For a long function with three processing stages, do not write one detailed comment covering the entire function at the beginning. Instead, write a high-level strategic comment at the beginning of the function:

  `// The process consists of three stages: Stage 1... Stage 2...`

Then, move the detailed comments to the beginning of the corresponding code block for each stage.

=> **General rule:** The farther a comment is from the code it describes, the more abstract and general the comment should be.

### 15.3. Comments Belong to the Source Code, Not the Commit Log

When solving a subtle problem or fixing a hidden bug, important details should be documented directly in comments within the code rather than only being recorded in the commit message.

Otherwise, future developers may not see the information, may accidentally remove the change, and cause the bug to reappear.

### 15.4. Avoid Repeating Comments
To make documentation easier to maintain when the code changes, each design decision should be documented only once, at the clearest possible location (for example, next to the declaration of the affected variable). Other relevant locations only need a short note pointing to the main comment.

*Example of referencing external documentation:* If you implement an existing protocol such as HTTP, there is no need to reproduce the entire protocol documentation in the code. Instead, provide a URL linking to the official specification on the Web.

Similarly, if a function implements a command that is already described in a user manual, you can simply write:

  `// Implements the Foo command; see the user manual for details.`

### 15.5. Check the Diffs

Before committing changes to a version control system such as Git, spend a few minutes reviewing all the changed lines (diffs) to make sure that the related comments have also been updated correctly.

### 15.6. High-Level Comments Are Easier to Maintain

High-level comments that are abstract and describe the code in an intuitive way are less likely to be affected by small changes to individual lines of code. This makes the documentation easier to maintain and update over time.
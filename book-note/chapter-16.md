## 16. Consistency

### 16.1. Levels of Applying Consistency

**Names:** Use consistent names for the same purpose throughout the entire system.

**Coding Style:** Follow style guides for indentation, bracket placement, declaration order, and other formatting conventions.

**Interfaces:** A single interface with multiple implementations (such as similar disk drivers) reduces the amount of knowledge developers need to understand.

**Design Patterns:** Using standardized and widely recognized solutions (such as Model-View-Controller) makes the code easier to understand.

**Invariants:** Ensure that a property is always true (e.g., every text line must end with a newline character) to reduce the number of special cases.

### 16.2. How to Maintain Consistency

**Documentation:** Document important conventions in an easily accessible place, such as the project's Wiki.

**Automatic Enforcement:** Create tools or scripts that automatically check for convention violations before code is committed.

**Example:** The author's project once encountered problems because Unix developers used a newline character (`\n`), while Windows developers used both a carriage return and a newline (`\r\n`). This caused unnecessary noise in the file modification history.

The author solved the problem by writing a script that automatically rejected a commit if a file contained the `\r` character, eliminating the problem completely.

**"When in Rome...":** When modifying an existing file, carefully observe and follow the structural conventions and naming conventions already used in that file, such as `camelCase` or `snake_case`.

**Don't Change Conventions Arbitrarily:** Do not break consistency simply because you have a "better idea", unless the idea is significantly better and the organization is willing to invest the time required to update the existing codebase accordingly.

**Note:** Consistency is only useful when things that are **similar are treated similarly**. If you force things that are genuinely different into the same pattern (or use the same variable name for different concepts), the system will become even more confusing.
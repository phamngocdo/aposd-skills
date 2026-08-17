## 1. The Nature of Complexity

### 1.1. Concept of Complexity

**Complexity** is anything related to the structure of a software system that makes the system difficult to understand and modify.

=> The more complex a system is, the harder it is to modify, even when implementing a small feature.

**Complexity is determined by the activities that are performed frequently.**

Some parts of a system may be complex, but if they are rarely touched or modified, they do not have a significant impact on the overall complexity of the system.

### 1.2. Symptoms of Complexity

**Change Amplification:** Small changes in the code require modifications in many different places.

**Example:** Changing the background color requires modifying multiple pages.

=> **Solution:** Define a single background-color variable and have all pages use that variable. Then, the color only needs to be changed in one place.

**Cognitive Load:** A symptom of software complexity that refers to the amount of information a programmer must know and keep in mind in order to complete a specific task.

The higher the cognitive load
=> The more important information the developer has to keep track of
=> The easier it is to introduce bugs or overlook important parts of the system.
**Note:** Sometimes, adding more code can actually help reduce cognitive load if it makes the system easier to understand.

**Unknown Unknowns:** It is unclear which parts of the code need to be modified to complete a task, or what information a developer needs in order to complete the task successfully.

=> This is the **worst form of complexity** because developers do not even know what they do not know.

**Example:** Some pages on a website use a variable called **emph** to control the background shadow. Normally, a developer only needs to change **bannerBg** to change the background color. However, they may not realize that **emph** also needs to be changed on each page to keep the design consistent. The developer may not even know that the **emph** variable exists. Even if they notice it, they still have to search through the codebase to find every place where this variable is used, which wastes time.

### 1.3. Causes of Complexity

**Dependencies:** To modify one piece of code, other related pieces of code also need to be modified. The component cannot be changed independently.

=> The goal is to **reduce dependencies** and make the remaining dependencies as simple as possible.

**Obscurity:** Important information is not clearly expressed or is difficult to discover. For example, variable names may be unclear, documentation may be too vague, or the relationship between different components may not be obvious.

=> The goal is to **simplify the system's design** and make important information more explicit.
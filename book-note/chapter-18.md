## 18. Designing for Performance

### 18.1 How to Think About Performance

**Blindly optimizing every line of code** can slow down development and introduce unnecessary complexity. On the other hand, completely ignoring performance can make a system 5–10 times slower due to accumulated small inefficiencies (**death by a thousand cuts**).

The best approach is to choose **"naturally performant" solutions** based on a basic understanding of expensive operations, such as network communication, disk I/O, dynamic memory allocation, or cache misses, while keeping the code clean and simple.

**Example:** Using a hash table instead of an ordered map when ordering is not required can improve performance by 5–10 times while keeping the code simple.

**Simple code and deep classes** often run faster than complex code because they eliminate special cases and reduce the number of **layer crossings**.

### 18.2 Measure Before Modifying

A programmer's intuition about performance is often unreliable. Therefore, **never optimize based solely on personal assumptions**, as this can unnecessarily increase complexity.

Always **measure the current system** to accurately identify the specific parts that consume the most time, and use these measurements as a **baseline** for comparison after making changes. If a modification does not provide a significant improvement, **roll back** to the original code to avoid retaining unnecessary complexity.

### 18.3 Design Around the Critical Path

Once you identify a piece of code that actually slows down the system, redesign it around the **critical path** — the minimum amount of code that must execute in the most common case.

Temporarily ignore the existing class structure and special cases to imagine the **cleanest and most concise "ideal" implementation**. Then, build a new design that comes as close as possible to this ideal implementation.

Move special cases **out of the critical path** by checking for them early with a single `if` statement. If a special case is detected, branch to separate handling elsewhere, keeping the critical path smooth and unobstructed.

### 18.4 Practical Example: RAMCloud Buffers

The `Buffer` class in the **RAMCloud** project was initially written without performance optimization. It contained multiple **shallow layers** that called one another sequentially. For example, `Buffer::alloc` called `allocateAppend`, which then called another function with a similar purpose. As a result, the critical path had to check up to **six separate conditions**.

The development team restructured the class by consolidating the entire critical-path logic into a **single method** and introducing a new variable called `extraAppendBytes` to handle all special cases with a **single `if` statement**.

**Result:** The new design not only made the code cleaner and reduced the class's lines of code by **20%**, but also **doubled its performance**. The insertion time decreased from **8.8 ns to 4.75 ns**.
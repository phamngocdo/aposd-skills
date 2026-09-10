## 10. Design it twice

**Software design is difficult:** Software design is a challenging task, so the first idea that comes to a programmer's mind is rarely the optimal solution. You can achieve much better results by considering multiple alternatives for each major design decision.

**The "Design Twice" approach:**
  - When designing the interface or structure of a module, instead of immediately choosing the first idea that comes to mind, sketch out at least two or three fundamentally different approaches.

  - Even when you are convinced that there is only one feasible solution, try to come up with a second alternative. Analyzing the weaknesses of that alternative and comparing it with the original approach will help you gain a deeper understanding of the system.

**Evaluation and selection:**
  - After sketching out the alternatives, list the advantages and disadvantages of each based on criteria such as ease of use for higher-level code, generality, and implementation performance.

  - The final decision may be to choose one of the alternatives or combine the best aspects of several alternatives to create a new and potentially better design.

**This principle can be applied at multiple levels**, ranging from designing the external interface of a class (class API), designing the internal algorithmic structure (implementation), to decomposing large modules across the entire system.

Considering alternative designs for a small module typically takes only one or two hours. This is a very small amount of time compared to the days or weeks spent writing code, while the benefits of a good design can pay off many times over.

**The author notes** that highly intelligent people often develop the bad habit of immediately implementing their first idea because they are accustomed to doing so when solving short problems in school. However, for large software systems, no one is skilled enough to design everything correctly on the first attempt.
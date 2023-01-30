+++
title = "Automation Maturity Levels"
description = "📊 Automation Maturity Levels"
date = "2023-01-25"

[taxonomies]
categories = ["development"]
tags = ["automation"]
+++

Software automation can be classified with the following levels of maturity:

1. **Chaos:** There is no process.

2. **None:** The process is documented, but is executed manually by a human.

3. **Blind:** Does one thing, without question.

4. **Speaks:** Does one thing, writes messages when executing, and reports success or failure.

5. **Listens:** Takes in parameters, and does different things.

6. **Multi-tasks:** Executes work in parallel.

7. **Thinks:** If the work is already done, does not re-do it.

8. **Translates:** Writes output in human-friendly or computer-friendly formats.

9. **Empathizes:** Presents information in a way that is not overwhelming.

10. **Communicates:** Explains errors at a level that the user understands.

11. **Guides:** Suggests ways to fix errors.

12. **Protects:** Guards users from mistakenly using values that incur high costs.

13. **Saves:** Guards users from mistakenly executing actions with permanent consequences.

14. **Forgives:** Allows users to stop and revert a process when it wasn't intended.


Small scale automation such as shell scripts tend to be at level 5, as the number of users is usually small, and their ability to understand technical errors is high.

As the complexity of the automated process increases, higher levels of maturity are necessary to:

* Enable users with less technically-focused roles to use the automation.
* Reduce the mental and emotional load on technical users who have to manage large systems.

Large scale automation may reach higher maturity levels for a subset of the overall process. This means large scale automation still requires substantial effort from users to understand the parts of the automation that are not at those higher levels.

+++
title = "The Need For A New Framework"
description = "The Need For A New Framework"
date = "2023-01-25"

[taxonomies]
categories = ["development"]
tags = ["automation", "purpose"]
+++

> When there is no automation, you do the work.
>
> When there is automation, you work for the automation to do the work.

Whenever there is automation, the following work exists:

* Development of the automation tool.
* Learning how to use the tool.
* Collecting parameters to pass to the tool.
* Checking the state of things, before running the automation.
* Recording execution progress and outcome for investigation.
* Fixing things when the automation encounters an error.
* Cleaning up when things are no longer needed -- reduce resource usage / cost.

Common issues with defining and using automation are:

* Not handling all cases.
* Not knowing how long a process takes.
* Not understanding what an error means.
* Not knowing how to resolve an error.
* Unnecessary / repetitive waiting.
* Manually copying values between different automation processes.
* Starting an unstoppable process, then realizing something is wrong shortly after.
* Forgetting to clean up.
* Cleaning up, then realizing there is a mistake.

The impacts of these issues include:

* 🧠 Mental stress.
* 💙 Emotional stress.
* 🥹 Loss of morale.
* ✊ Inefficient use of effort.
* ⏲️ Inefficient use of time.
* 💸 Inefficient use of money.

To reduce these negative impacts, an effective solution should encompass the following concerns / considerations:

* Low effort to prototype a hacked up solution.
* Progressive effort to turn the prototype into the actual automation tool.
* Compiler support for tracking execution paths.
* Automation based on psychology, not philosophy.
* Present information suitably for the current usage.
* Steps are executed with parallellism and concurrency.

As much as possible, the solution should shoulder the burden of development from the automation developer -- if developers can focus on business logic, and automatically receive the user-friendly features, then the development experience will not be degraded, which allows them to maintain visible output and provide good user experience.

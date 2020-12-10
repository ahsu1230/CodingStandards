# Balancing Development Time

Story of the Tortoise and the Hare (outlined in *Clean Architecture* Chapter 1)

- Hare: Building features and releasing as fast as possible
- Tortoise: Taking things a bit slower to make the code more development-friendly. This could mean writing tests, refactoring to encapsulate components, or re-writing code to make it easier to read and understand.
- Moving fast is overconfidence speaking
- At some point, you WILL hit a wall and your codebase will be hard to understand and hard to develop. There will be many components that are hard to understand and even scary to change (if I'm not careful with my change, I'll break a feature).
- You could totally do a huge refactor, but if you're frequently rewriting the codebase, you'll end up running to the same problems again and actually take MORE development time than if you had slowed down to consider your architecture & design.

## The Eisenhower Matrix

![Eisenhower Matrix](../images/eisenhower_matrix.png)

Introduced in *Clean Architecture* Chapter 2. There are 2 questions to consider in the Eisenhower Matrix:
- Is this task important?
- Is this task urgent?
- Combined, we get 4 classifications in which a task can be placed into.

As a software developer, it is **your** responsibility to classify each of your task into one of the classifications. The challenge comes when figuring out a task is "important but not urgent" when people say it is "urgent". And as a developer (whether you're a lead or intern), you must always stress the importance of architecture over urgency. 

As a software architect, it is absolutely **your** job to focus on architecture over feature functions. It is your role to be the tortoise when everyone else (stakeholders, project managers, ops, sales, etc.) wants to be the hare. You have a responsibility to the codebase which you frequently work with and a responsibility to your technical coworkers to provide the best software as possible (software that is easy to develop on, easily modified, and easily extended).

## Tactical programming vs Strategic programming

Tactical programming
Strategic programming
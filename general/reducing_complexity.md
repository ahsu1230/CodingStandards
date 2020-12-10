# Reducing Codebase Complexity

> Complexity is anything related to the structure of software system that makes it hard to understand or modify the system.
>
> A Philosophy of Software Design ~ John Ousterhout (Chapter 2)

## Symptoms of a complex codebase:

- Change amplification
  - A simple change requires modifications in many places
- Cognitive Load
  - How much a developer needs to know about the codebase (context, history, etc.) in order to complete their task.
  - This could be number of files / classes they need to consider, but not necessarily. Sometimes more lines of code can encapsulate components and reduce cognitive load.
- Unknown unknowns
  - It's not clear what needs to change and it's not clear what the developer needs to know in order to safely make that change.
  - This is definitely the worst!


> The most important goal for a codebase system is being obvious

A system that is obvious means a developer can make a quick educated guess and be confident of where a change should be made or where code should be extended from.

## Causes of complexity:

- Dependencies
  - When a given piece of code cannot be modified in isolation. Changing a piece of code requires knowledge from other places in the codebase. Even worse if the knowledge resides in a different component of the codebase.
  - Having dependencies could be useful in reducing change amplification and cognitive load, but could also increase it.
  - Beneficial example: you could have the background-color of the entire website set in an `app.css` file. Now all webpages will have to same background color and don't have to worry about it (reduce cognitive load). And if you want to change the background-color, you only have to change it in one file (reduce change amplification).
  - Harmful example: if you have a strict API between a web-service and a web-client, in order to change this API behavior, you may have to change BOTH the client and server code (increased cognitive load & change amplification). But this is not the end of the world...
  - It's okay to have dependencies in a codebase. But we want to make sure the dependency relationships are obvious and easy to understand.

- Obscurities
  - When important information is not obvious
  - Examples: Unclear variable / function / class names. Or inadequate documenation.
  - Obscurities are super bad because they create unknown unknowns!!!
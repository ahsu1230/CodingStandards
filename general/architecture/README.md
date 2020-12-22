# Best Practices to Writing Clean Architecture

> The goal of software architecture is to minimize human resources required to build and maintain the system.
> 
> ~ Clean Architecture - Robert C. Martin (Chapter 1)

## Modular Design

The act of encapsulating your entire software into components so you do not expose a programmer to all the complexity of the codebase at once. Software architecture strives to split up work to a group of developers so that individual components eventually work together to produce results.

There was an "old way" of doing this and a more "modern" way of doing this.

### Waterfall Model

- 5 steps: definitions, design, coding, testing, maintenance
- Design is "frozen in place" into a *schema* and once confirmed, subsequence components are completed one at a time in order.
- Once fully defined & designed, you start coding ALL features. Once coding has completed, you test ALL features, etc.
- Problem: if the project requirements change or problems with implementation details come up, you'll have to update the schema. This will probably mean your original project definitions will have to change and you may have to start over the process again. Alternatively, you might force things to fit and you'll introduce hacks and make the codebase more complex.

### Agile Development

- Quick development and iterations over a subset of product features.
- Focus on a subset of full project requirements and iterate. For example, think of the MVP (Minimum Viable Product) with the most rudimentary working features. Develop these features first and then improve.
- Each iteration exposes some problems with the original design and the schema changes a bit before the next iteration starts.
- Software becomes malleable enough to easily allow significant design changes.

---

## Why Architecture is important. The Great Debate - Making it work vs. Making it easy to change

~ Clean Architecture - Robert C. Martin (Chapter 2)

- "Making it work" means getting a product feature (usually requested by stakeholders) working and available to users as soon as possible.
- "Making it easy to change" means writing this product feature so it can be easily understood (by other developers) and extended / maintained in the future.

From a developer perspective, it's easy to guess we should always strive for the latter. But when it comes to deadlines, time-crunches, etc... you'll often be pulled into the "Making it work" side and rush to get a feature out.

As a developer though, you should try to lean to the latter as much as possible. *Make software "soft" (easy to change).*

- If a feature works, but is not easy to change... when the feature requirements inevitably change, the program is now useless! Or now the program is in a messy state where it's difficult to change it. The cost of changing the feature may outweigh the benefits of the new feature... in which case you'll have to punt or drop the feature anyway! SAD, NO FEATURE!
- However, if the feature doesn't work... but the codebase is easy to change, then eventually the feature will be completed while still maintaining a clean codebase! 

---

## The 3 concerns of Architecture

~ Clean Architecture - Robert C. Martin (Chapter 3)

- Function
  - Proving that our sofware works as intended.
  - With tests! Tests don't prove our software 100% works... but they show the *absence of bugs*.
  - We show correctness of our program by using tests to prove we don't fail.
  - With structured programming (functions), we can decompose our entire program into small provable functions.

- Separation of Components
  - Encapsulation with Object-Oriented Programming!
  - Using encapsulation, inheritance and polymorphism, you can modularize your program so that high level functions call lower level functions.
  - This allows programmers to have absolute control of code flow and can direct code dependencies.
  - If done correctly, certain modules (components) of the software can be changed and deployed separately from the rest of the application.

- Data Management
  - Mutability vs. Immutability
  - Functional programming can basically solve all race conditions, deadlocks, and concurrency problems because anonymous functions do NOT mutate control variables (unlike a loop). The anonymous function keeps immutability over the control variable and thus will never cause race conditions.
  - Separate your components into mutable and non-mutable. This includes variables and classes (abstract classes vs. implemented classes).
  - If they're separated correctly, you can protect the mutable components (via locks, for example).


<details>
    <summary>
        Another approach to immutable data management: Event Sourcing
    </summary>
    Store immutable transactions and do not keep track of a "final state". You can compute the "final state" by adding up all transactions to re-calculate the current state. The advantage of this is there's no more CRUD only CR (Create and Retrieve) - so no updates or deletes that are prone to race conditions. But the problem is that you would need infinite storage and high processing power to recompute the current state from the beginning. But instead, you could "cache" a recent state and compute from there instead of the beginning of time. Kafka and Source Control apps (Git) use this approach!
</details>

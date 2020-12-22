# Writing Good Comments

~ Philosophy of Software Design - Chapters 12, 13, 14, 15, 18

## Debunking Myths

- "Good code is self documenting"
  - If users must read code of a method, then there is no clear abstraction - this means it's complex!
  - In order to have clear functions, each function should be short and only do one thing. But, you'll have lots of shallow methods and up tracing a lot of code - complex!

- "Comments become out of date and can be misleading"
  - Keep code comments close to the corresponding code!
  - And do NOT duplicate documentation either!

- "No time to write comments! Code more important!"
  - Start writing comments first!
> If you want a clean architecture, which will allow you to work efficiently over the long-term, then you must take some extra time up front in order to create that structure. 

- "Comments are usually pretty useless"
  - This is probably the best excuse.
  - So how to make GOOD comments then?

### So why are GOOD comments important?
- Comments capture information that was in the mind of the designer but couldn't be represented in code.
- Good comments will help with *Cognitive Load* and *Unknown unknowns*

## Four types of comments
 - *Interface* (comments about a method, interface, or class)
 - *Data structure member* (comments about a specific attribute / variable / method)
 - *Implementation details* (comments inside an implementation of a method)
 - *Cross-module* (comments that cross module boundaries)

### Writing Interface comments

- Overview comments about a class, interface, or method.
- These should be things a dev needs to know BEFORE using a function or class
- They should NOT include implementation details
  - Will pollute and over-saturate comments, making them harder to change and easy to forget.
  - It's bad to have comments in two places (implementation details in class overview comments AND method comments).

### Writing Member comments

- 

### Writing Implementation Details comments

- Implementation comments aren't bad - they should just be used sparingly.
- Comments should help readers understand WHAT code is doing (not HOW, code should do this for you).
- Can be useful for organizing your code into code blocks.
Example:
```
function plantTree() {
  // Dig a hole <- 1 or 2 line comment that describes WHAT the code is doing
  ...

  // Place acorn in
  ...

  // Cover hole
  ...
}
```

### Writing Cross-module comments

- Hardest comments to make readable.
- Should either be placed in a place that's often used or viewed (near code that gets used frequently)
- OR it should be in a single file / doc repo elsewhere - a place where a dev can easily reference and know where to look for things if they're confused.

## KEY IDEA
> If a reader or reviewer says a code is not obvious, then it is not obvious! Do not argue with them. Instead, understand what is not clear or confusing to them and see if you can fix it using better comments or code.
>
> A Philosophy of Software Design ~ John Ousterhout

## How to write code with good comments

- Start with an interface comment (general overview of the class you are writing)
- Write interface comments for all the public methods and method signatures
- Write declaration comments for each instance variable
- Once these general comments are done, and the skeleton class has been marked out, then you can start filling in with implementation details.

Delaying comments to after implementation is a bad habit. Usually by that time, it won't happen or memories of the design process and decision making is fuzzy and lost, so you end up writing comments that repeat the code!

### Do NOT repeat your code when writing comments

- Good comments use "different words" to describe your code. To do this, think about "lower-level" comments and "higher-level" comments.
  - Lower-level comments add precision. They talk about units, boundaries, limits, nulls, invariants, etc.
  - High-level comments enhance intuition. High level comments talk about the general idea of the code flow. Why does this code belong here? How does this code fit in the general picture?
  - Both of these comment types do not talk about the current code you're working on. Instead, they act as a bridge to other places in the codebase that are related but not in the current scope.
- If comments repeat the code you just wrote, the comments become useless and actually make the codebase harder to maintain. You essentially have written code that exists in two places and must change together to be accurate.
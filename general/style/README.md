# General Coding Style Tips

The key behind good coding style is to make code obvious!

If code is not obvious, it obscures potentially important information from a developer.

## EZ Tips to make code obvious

- Precise and meaningful names
- Consistent naming
- Following conventions (either universal best practices or practices set by the codebase) and conforming to expectations
- Use of horizontal indentation & vertical whitespace to separate "ideas"

For more details, view the different pages to read more.

---

### Examples of not so good code

<details>
    <summary>
        Using the Pair<> class.
    </summary>
    The problem with the Pair class is that it can obscure the type values of both the `key` and `value`. If not used carefully, a reader will require additional context to undestand what these values are.

    ```
    function example(Pair<String, Integer> pair) {
        ...
        doStuff(pair.key(), pair.value());
        ...
    }
    ```

    In the above case, I have no idea what `pair.key()` refers to and have no idea what `pair.value()` is giving me. I understand they are giving me a String and an integer respectively, but what do those values represent?
</details>

<details>
    <summary>
        A main method which spawns new threads
    </summary>
    
    Usually, an application completely finishes when the main function returns. However, what if the main function spawns additional threads that continue running even when the main function ends?

```
function main() {
    App app = new App();
    app.run();
}
```

From this snippet, I have NO idea that there are threads being spawned and I'll need to stop those threads if I want to completely close my application. Or, I may need to have comments here to let the reader know that there are threads being spawned and are handled elsewhere in the codebase.
</details>

---

### Examples of cleaner code

<details>
    <summary>
        Using indentations and white spaces to separate ideas. 
    </summary>
    Imagine if you had a complex function that does multiple tasks.

```
function plantTree() {
  // Dig a hole
  ...
  ...

  // Place acorn in
  ...
  ...

  // Cover hole
  ...
  ...
}
```

Of course, maybe you could have 3 private methods, one for each sub-task. But, if the tasks are primitive enough, the above code is fine enough because you can tell how each sub-task of the method is separated into 3 ideas. The weakness of having too many private methods is that it forces a reader to have to "context-switch" around source code in order to follow the codeflow.

```
function plantTree() {
    digHole(...);
    placeAcorn(...);
    coverHole(...);
}
```

</details>

<details>
    <summary>
    Explain Yourself in Code
    </summary>

There are certainly times when code makes a poor vehicle for explanation. Unfortunately, many programmers have taken this to mean that code is seldom, if ever, a good means for explanation. This is patently false. Which would you rather see? This:

```
// Check to see if the employee is eligible for full benefits
if (employee.flags & HOURLY_FLAG) && (employee.age > 65)
```

Or this?

```
if (employee.isEligibleForFullBenefits())
```

It takes only a few seconds of thought to explain most of your intent in code. In many cases it's simply a matter of creating a function that says the same thing as the comment you want to write.
</details>
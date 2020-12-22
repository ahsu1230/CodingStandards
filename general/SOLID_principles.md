# SOLID Principles

*Clean Architecture* (Chapters 7-11)

Five code design principles by Robert C. Martin that help with writing maintainable software.

- Single Responsibility Principle
- Open-Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

> A *module* is defined as a set of functions and data structures (like a source file).

## Single Responsibility Principle (SRP)

- Code that is used by different "actors" (users / stakeholders) should be separated.
- A module should be responsible to one and only one actor.

<details>
    <summary>
        Example: The Employee Class
    </summary>
    Imagine if we had an Employee class and inside it we had attributes / methods for calculating pay and logging overtime hours. If `overtimeHours()` is used by the COO, the CFO uses the `calculatePay()` method. Now since `calculatePay()` depends on `overtimeHours()`, if we change the behavior of logging overtime hours, it will affect pay results.

    This is a source of complexity and unclean architecture! We have two groups in the company that are potentially changing the same files for different reasons. And we also risk merge conflicts where both parties don't have full context of why & how the other party is changing code.

    Solution:
    Instead, use a "Delegate" class. This class doesn't contain much code, but instead delegates logic to other supporting classes.
    For instance, instead of `calculatePay()` residing in the Employee class, instead have it reside in the `PayCalculator` class. The logic for logging hours can then reside in an `HourReporter` class. The Employee class can retain it's `getPay()` and `reportHour()` methods, but these methods will have no logic and instead call methods from the other classes.

    Now, changes to this workflow are segmented. One change by one party will not affect the other. Changing the pay logic will not affect the overtime logging logic. And now it's clear who owns what: the CFO owns `PayCalculator` while the COO owns `HourReporter`.
</details>

## Open-Closed Principle (OCP)

- A software artifact should be open for extension, but closed for modification.
- Ideally, when a new feature is to be added to a codebase, only new lines of code should be added and zero (or very minimal) lines of existing code should by modified.
- Making a codebase easy to change means making the codebase easy to extend from.

<details>
    <summary>
        Example: Multiple Views from a single Presenter
    </summary>
    Let's say we're building a web application and our first users are desktop users and mobile users. To simplify our codeflow, we could have a `WebsiteView` and a `MobileView` that both display contents based on their respective viewport. Both of these classes should be implementations of an abstract class called `View`. We can then have a `Presenter` class which contains an instance of the abstract `View` class (doesn't matter which View class) and simply calls `View.display()`.

    Now, let's say we needed a way to render our data onto printed paper in a neat way. It would be easy to *extend* our current codebase by adding a `PrintedPaperView`. With this new class, the other 3 classes (WebsiteView, MobileView, and Presenter) don't change at all - we just have to a provide the `Presenter` with this `PrintedPaperView` class as input and it will call `View.display()` as usual.
</details>

## Liskov Substitution Principle (LSP)

- An extension of OCP - objects of a superclass should be replaceable with objects of subclasses without any potential break in the application.
- Similarly, methods in the subclass must accept the same input parameters of the superclass.

<details>
    <summary>
        Bad Example: Rectangle vs. Square
    </summary>
    Imagine if you have an abstract Rectangle class (with width and heigh attributes). A Square class can extend from this Rectangle class to re-use functionalities. However, a user of this class takes in a Rectangle class as input.
    The problem here is that the user does not know if it's using a Rectangle or using a Square. And because of that, the user doesn't know that if it's using a Square, the width and height must always be equal and change together!
    This makes it more complex because now a developer has to keep this edge case in mind! This is NOT a "substituable" interface type.
</details>

## Interface Segregation Principle (ISP)

- Do not let classes extend off of interfaces with functions you don't need.
- Do not let a dependency carry baggage that may cause unexpected trouble in the future.

## Dependency Inversion Principle (DIP)

- The most flexible systems have source code dependencies refer only ABSTRACTIONS and not concretions.
- So a source code should not depend on a concrete module (implemented class). Instead, it should depend on an abstract class / interface.
- A change to an abstract interface means a change to to the concrete implementation. However, a change in the concrete implementation does NOT always mean a change to the interface.
- Do NOT override concrete functions - always use an interface instead.
- **Dependency inversion** - imagine a line that separates abstract classes from concrete classes. The "dependency rule" is that when we have one class depend on another, we ALWAYS have a concrete class depend / implement an abstract class. We should NEVER have an abstract class depend on a concrete class.

<details>
    <summary>
        Factories can be used as an abstraction layer to create concrete classes.
    </summary>
    For instance, an Application can refer to a ServiceFactory (abstract). A ServiceFactoryImpl implements it to create a Service class. So the Application only needs to refer to the ServiceFactory and Service classes, it does not need to know about the FactoryImpl or the ConcreteServiceImpl. All the high level business rules are in abstract layers and are separated from the concrete layers. Dependency inversion!
</details>

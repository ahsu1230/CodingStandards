# Testing

> One of the tenets of agile development is that testing should be tightly integrated with development, and programms should write tests for their own code.
> 
> Philosophy of Software Design (Chapter 19.3)

> Test code is just as important as production code. It is not a second-class citizen. It requires thought, design, and care. It must be kept as clean as production code.
>
> Clean Code (Chapter 9)

> It is unit tests that keep our code flexible, maintainable, and reusable. If you have tests, you do not fear making changes to the code! Without tests every change is a possible bug. [...] If you don't keep your tests clean, you will lose them. And without them, you lose the very thing that keeps your production code flexible. [...] Having an automated suite of unit tests that cover the production code is the key to keeping your design and architecture as clean as possible.
> 
> Clean Code (Chapter 9)


## Two types of tests

- Unit tests - Small code snippets that test the behavior of a small section of code (like a single method). Unit tests can be run in isolation and can be run without setting up a complicated system environment (like a database).
- System tests (aka integration tests) - ensure that the different components of an application all are working together. This is typically done by running the tests in a production environment (or environment similar to what's going on in production).

Both types of tests play important roles in software design because they give us software developers confidence that certain features work. And if we ever want to make a slight change or a large re-organization of the codebase, the tests will tell us whether those changes affected our code's behavior or not. Without a test suite, it becomes very dangerous to deploy new code - it's much better to catch bugs before users start using your new feature!

However, whether a test you write is a unit test or a system test, they follow the same rules when it comes to writing "good" tests.

---

## FIRST

~ Clean Code (Chapter 9)

Tests should follow these 5 rules:

F - Fast. Tests should run quickly. If tests run slow, you won't want to run them frequently, which means you won't find problems to fix easily.

I - Independent. Tests should NOT rely on each other. Tests should be able to run in any order and each test is isolated so they don't affect each other - this is especially useful so that they can be run in parallel.

R - Repeatable. Tests should be repeatable in any environment (with or without a network, on any laptop, etc.)

S - Self-validating. Tests should have a boolean output. Either they pass or it fails. You should not have to read through a log file to know whether a test passed or not. You can however, have a log file or print statements to "record things" in case the test fails. This way, you'll have a way to debug things.

T - Timely. Test code is written alongside the production code. In other words, when you make a commit, the production code you write and the test code should be in the same commit. This gives confidence that your production code works and there isn't a commit in the codebase where the production code could potentially be flawed.

---

## What makes a clean test?

Readability! What makes a unit test readable? Same thing as what makes code readable - clarity, simplicity, and density of expression.

<details>
    <summary>BUILD-OPERATE-CHECK pattern</summary>

Consider this code:

```
public void test1() throws Exception {
    crawler.addPage(root, PathParser.parse("TestPageOne"), "test page");

    request.setResource("TestPageOne");
    request.addInput("type", "data");
    Responder responder = new SerializedPageResponder();
    SimpleResponse response = (SimpleResponse) responder.makeResponse(...);
    String xml = response.getContent();

    assertEquals("test/xml", response.getContentType());
    assertSubString("test page", xml);
    assertSubString("<Test", xml);
}
```

The above code is very cumbersome and there are a lot of details fit into the test that may be confusing or overwhelming for readers. This is a test NOT built for readability. Compare this to the following code below:

```
public void test1() throws Exception {
    buildPageWithContent("TestPageOne", "test page");
    
    submitRequest("TestPageOne", "type:data");

    assertResponseIsXml();
    assertResponseContains("test page", "<Test");
}
```

From a short series of statements, it's much more clear how the test is separated into 3 steps (Build, Operate, Check). And it will be easy to re-use these helper methods for other tests - which cements a pattern in the test file, making it much easier to read for readers.

*Note* Here we are demonstrating the technique of building a "domain-specific language". In this case, it's creating a "language" that is used solely for the purpose of making your tests easier to write and understand. For example, `assertResponseIsXml` and `assertResponseContains` are just very simple helper methods, but these methods are named and formed for the sole purpose of making it easy to write the behavior of the test function and will probably be used in many different unit tests. So you're basically ***creating a new language just for testing***. Hence, "domain-specific language".

</details>

---

<details>
    <summary>Different Standards</summary>
    Production code and test code have two different priorities. While production code will usually emphasize performance, test code must prioritize readability. It doesn't need to be as efficient as production code - more importantly, it must be simple, succinct and expressive.
</details>

---

<details>
    <summary>One Assert per Test? Single Concept per Test.</summary>

A school of thought says that every test function should only have one `assert` statement. This might be too strict of a rule, but it's a good guideline. To have well-organized tests, you want to make sure your tests are testing different things and that you are not duplicatively asserting the same thing in multiple tests.

For instance, let's say you want to test a `multiply` function. You could have the following test functions:

```
testMultiply(5,5)
testMultiply(3,2)
testMultiply(2,3)
testMultiply(3,3)
testMultiply(1,2)
testMultiply(2,1)
testMultiply(1,1)
testMultiply(0,1)
testMultiply(1,0)
testMultiply(0,0)
testMultiply(1,-1)
testMultiply(-1,1)
testMultiply(-1,-1)
```

Having many tests are great, but the problem here is that you actually have many "duplicate" tests. The test functions all use different inputs, but some tests aren't adding any benefits to the entire suite and are actually testing the same multiply behavior. With these repetitive tests, you end up cluttering your test suite with unnecessary code...

Consider instead, having a single purpose for each of the tests. If I wanted to test multiplicity, there are a few properties I need to test to make sure it is correct.

1) Anything multiplied by 0 is 0
2) Anything multiplied by 1 is itself (Identity property)
3) Anything multiplied by a negative number becomes its negative
4) Test the reflexive property (A * B = B * A)
5) Test the associative property (A * B) * C = A * (B * C)

So your test suite can now be condensed like so:

```
testMultiplyByZero() => test 4 * 0
testMultiplyByOne() => test 4 * 1
testMultiplyPositiveByPositive() => test 4 * 2
testMultiplyPositiveByNegative() => test 4 * -2
testMultiplyNegativeByNegative() => test -4 * -2
testReflexive() => test 4 * 2 and test 2 * 4. Make sure they're equal.
testAssociative() => test 4 * 2 * 3 and test 2 * 3 * 4. Make sure they're equal.
```

If all these tests pass, then you can have more confidence that your multiply function works. There's no need to test 0 * 4 (for example) because this situation is already covered by both the `testMultiplyByZero` and `testReflexive` test functions.

When you switch to testing "concepts" instead of testing via "inputs", you can condense your test suite to only include key important situations. Your tests will be cleaner because there's a clear idea being targeted behind each test.
</details>

## Integration Tests into your Architecture

Tests follow the Dependency Rule; they are very detailed and concrete; and they always depend inward toward the targeted production code being tested. In other words, nothing in the system depends on tests and the tests always depend on some component of the system.

> The extreme isolation of the tests, combined with the fact that they are not usually deployed, often causes developers to think that tests fall outside of the design of the system. This is a catastrophic point of view. Tests that are not well integrated into the design of the system tend to be fragile, and they make the system rigid and difficult to change.
> 
> Clean Architecture (Chapter 28)

### The Fragile Tests Problem

The issue described above is called **coupling**, where a change in a strongly coupled system component can cause hundreds or thousands of tests to break. This makes the code harder to change and thus makes the system rigid.

*Solution* To get around the problem of an overly coupled test system is to create a "testing API". This testing API is a specific API, solely used for testing, that allows tests to verify all business rules, and decouple the structure of tests from the structure of the application. (This is the domain-specific test language we mentioned above!). This API should also have superpowers that force the system into testable states like avoiding security constraints, bypass expensive resources (like skipping a database call via mocking), etc.

### Structural Coupling (AVOID!)

Imagine a test suite that has a test class for every production class and a set of test methods for each production method. The test suite basically reflects the application structure. This is NOT really a good idea because a change to the production code (like a change in the method or a refactor of the class) will cause a huge change in the tests.

Ideally, the test API should hide the structure of the application from the test. This allows the production code to be refactored in a way that doesn't affect the tests. It also allows the tests to be refactored in a way that doesn't affect the production code.

This separation is necessary because as time passes, ideally, the tests become more increasingly concrete while the production code should become more abstract! If tests and application code is too strongly structurally coupled, it prevents the production code from being flexible and abstracted.

### Test Driven Development (TDD) is BAD!

Test-driven development is an approach to software development where the programmer writes unit tests BEFORE writing the code. This allows the programmer to define the resulting behavior of the method before even writing any code. At first, all tests fail, but as the programmer slowly implements the method, eventually all the tests pass!

It's not a bad idea, but what happens is that the developer focuses more on getting things to work instead of thinking about design - this is tactical programming! Tests can easily get messy, especially because they get placed on the back-burner often so thinking about an organized, coherent test suite design, a good testing API, etc. are all very important to maintain clean tests.

The nugget of truth behind TDD is that it helps form good habits when it comes to writing tests. Tests should be written alongside production code in order to verify the behavior of the codebase. For example, if there's a bug in the codebase, first write a unit test that fails from the bug. When the bug is fixed, that newly written test should now pass. This ensures that your new code truly fixed the bug!
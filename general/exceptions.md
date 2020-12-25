# Handling Exceptions

## Quotes 

> Exception handling is one of the worst sources of complexity in software systems. Code that deals with special conditions is inherently harder to write than code that deals with normal cases.
>
> Principles of Software Design (Chapter 10)

> Most programmers are taught that it's important to detact and report errors.; they often interpret this to mean "the more errors detected, the better." This leads to an over-defensive style where anything that looks suspicious is rejected with an exception, which results in proliferation of unnecessary exceptions that increase the complexity of the system.
>
> Principles of Software Design (Chapter 10)

> The best way to reduce the complexity damage caused by exception handling is to reduce the number of places where exceptions have to be handled.
>
> Principles of Software Design (Chapter 10)

## Simplifying your error handling

*Principles of Software Design* provides 4 techniques to prevent you from riddling your codebase with exception handlers, making your codebase unnecessarily complex.

- Define errors out of existence
  - Change the behavior of methods / signatures to not error.
  - Example1: Java's String substring method could not throw IndexOutOfBoundsException if the indices are "out of bounds". Instead, they could just select the characters between the range, regardless of those indices values.
  - Example2: Deleting files that are currently opened (in Windows vs. Unix). In Windows, files can't be deleted if they are in use (returns exceptions here). In Unix, delete-file command returns success. The current process using the file keeps track of the "old state" of the file while no new processes can open the "deleted" file. In this case, no error possible!
  
- Mask exceptions
  - Exceptions are handled in low-level system so higher-level systems do not need to worry aobut these exceptions.
  - The idea is if low-level system can't function from exceptions, there's nothing the higher-level system can do anything about it - so it's a meaningless error.
  - Example: Having a network file server just hang if it loses connection instead of aborting its operations. If the system aborts, it may be unclear where in the operation process things failed and we have to start over our work! We'll have to reset to a good state, etc. in order to do this. Or we notify the user... but in many cases, the user won't know what to do anyway and will have to restart the operation anyway. If we hang, no errors, no need to reset states, we just start from where we were when the server comes back live again. In the meantime, we can print `NFS server not responding... still trying` messages to notify what's going on.

- Exception aggregation
  - Instead of handling errors in different parts of the system. Have lower-level systems propogate their errors to a higher-level handler that catches all / any exception that occurs. Then, exceptions are handled in one place! 

- Just crash?
  - There are times where there's nothing you can do about an error that occured - think OutOfMemory errors. In these cases, trying to fix this problem usually outweighs the benefits of handling this exception - or reveals a bigger bug in the application.
  - There are also situations where exceptions happen so infrequently, it would be best to just crash instead of trying to handle them. For example, a network socket cannot be opened or a hard disk error - it would be better to just let the application crash instead of trying to handle that error.

- However, these 4 techniques can be taken too far. The idea is to make sure important errors are exposed while non-important errors can be hidden away. The more non-important errors hidden, the better. But sometimes these errors can be useful for exposing bugs and problems!
  - Example: Java substring method - if indices are out-of-bounds, incorrect or unexpected, it means there's something that went wrong!

## Defining your exceptions

~ Clean Code (Chapter 7)

- Provide context with your exceptions
  - Where did the exception occur (source)?
  - What type of exception occured (network failure, device failure, programming failure)?
  - How are these exceptions caught?

- Do not return null! Things should be success, or an exception! 
- Similarly, do not pass nulls to a method! If nulls are passed to a method, you may get a NullPointerException. Otherwise, you'll have to add null checks inside that method - this is bad! It will complicate your code even more!

## Example: Encapsulate your error handling

~ Clean Code (Chapter 10, page 108)

Consider the following code where you can have an assortment of exceptions that occur from a single statement.

```
ACMEPort port = new ACMEPort(portNum);

try {
    port.open();
} catch (DeviceException e) {
    reportError(e);
    logger.log("Device exception occured", e);
} catch (NetworkException e) {
    reportError(e);
    logger.log("Network exception occured", e);
} catch (RandomException e) {
    reportError(e);
    logger.log("Application exception occured", e);
} finally {
    ...
}
```

This code is super verbose and there's a lot of duplication in how the code handles exceptions. From any exception, we need to report and log that error in order to proceed. Instead, we can simplify our code like so:

```
MyPort port = new MyPort(portNum);
try {
    port.open();
} catch (PortDeviceFailure f) {
    reportError(f);
    logger.log(f.getMessage(), f);
} finally {
    ...
}
```

Now `MyPort` is a wrapper class along with a custom-defined `PortDeviceFailure` class that encapsulates different types of failures.

```
public class MyPort {
    private ACMEPort innerPort;

    public MyPort(int portNumber) {
        innerPort = new ACMEPort(portNumber);
    }

    public void open() {
        try {
            innerPort.open();
        } catch (DeviceException e) {
            throw new PortDeviceFailure(e);
        } catch (...) {
            throw new PortDeviceFailure(e);
        } catch (...) {
            throw new PortDeviceFailure(e);
        }
    }
}
```

Then, you can have PortDeviceFailure handle the different messages for the exceptions as well. The point here is that the code looks similar to what we had before, but with more "extra work". But we've simplified the main business logic (code near `port.open()`) BY A LOT in terms of readability. The frequently visited code is much more readable now, while the nitty-gritty details of potential exceptions are encapsulated and hidden away.

**Lesson: when handling exceptions, we want to make the "normal codeflow" be as easy to read as possible. Try to minimize the exception handling in the normal codeflow as much as possible.**
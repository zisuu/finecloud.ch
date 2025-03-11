---
title: "Java Basics: Exceptions"
meta_title: "Java Basics: Exceptions"
description: "Learn about Java exceptions, the different types of exceptions, and how to handle them."
date: 2023-04-10T12:05:00Z
categories: ["Software Development", "Java"]
author: "David"
tags: ["java", "exceptions"]
draft: false
---

## What are Exceptions?

An exception is an event, commonly it is a problem that arises during the execution of a program. When an Exception occurs the normal progris disrupted and the program/Application terminates abnormally, which is not recommended. Unexpected termination of an Application is never wanted and this is why exceptions should be handled, so that the Application lets the User know that some Error or Problem exists, but the Application itself should not just die without any information.

The most common scenarios where an exception occurs are:

* the user entered invalid data
* a file needs to be opened but cannot be found
* a network connection has been lost during communication
* the JVM has run out of memory

## Types of Exceptions

There are three main types of Java Exceptions:

* Checked Exceptions
* Runtime Exceptions
* Error Exceptions

Checked and Runtime Exceptions both extend the same superclass, which is Exception. Exception class itself extends from a superclass called Throwable.

## Example Scenario #1

Given the following code:

```java
public class ExceptionTests {
    public static void main(String[] args) {
        String[] array = {"one", "two", "three"};
        int num = 0;
        System.out.println(array.length / num);
        System.out.println(array[2]);
    }
}
````

if we execute this application it crashes like this:

```bash
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at section11_loose_ends.ExceptionTests.main(ExceptionTests.java:7)

Process finished with exit code 1
````

this should not be surprising, since the fact that a devision by zero is considered to be impossible. And so the JVM generated this arithmetic exception. And what happens then is that once we get to this line in that arithmetic exception is thrown. The program just dies.

💀 Right then and there.

So line six is never even reached because that exception got thrown.

### Try / Catch

What we will do next here is really just for illustrative purposes. This this isn't actually, generally speaking, a good idea, but we can handle this exception or we can catch this particular exception like this:

```java
public class ExceptionTests {
    public static void main(String[] args) {
        String[] array = {"one", "two", "three"};
        int num = 0;
        try {
            System.out.println(array.length / num);
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println(array[2]);
    }
```

This is called a try catch block. So now we've got this keyword try. So we're now telling the JVM essentially try executing this line. And if anything goes wrong, maybe we'll catch it and do something about it down here.

So now if I run this again, let's see what happens:
```bash
java.lang.ArithmeticException: / by zero
	at section11_loose_ends.ExceptionTests.main(ExceptionTests.java:8)
three

Process finished with exit code 0
```

So we still have an exception happening here. The divide by zero, but the next line line 10 is executing now. Please note, this is generally not a good idea, this is just for illustrative purposes so that you can learn how exceptions work and how they can be caught and when to catch them and how and why, and all of that.

So what just happened now is that an exception was still generated in throne when the JVM realised that we were about to attempt a divide by zero operation here. So it generated the arithmetic exception and it threw it. But now that we have this try catch block here, we cut the exception. And so that's why they used these terms to throw and to catch right.

### The Exception

You can think of it as an object, it's an object that's sort of being thrown. And if you set up a catcher to catch it, then you can do something sometimes with that exception. In this particular case, all we're doing with it is simply printing its stack trace. Basically, it's printing the details of the error that occurred. And that's how we get to see this divide by zero.

Now, previously, before I surrounded this line with a try catch, we were still seeing this, this output here, and that was happening because the exception was being handled by the JVM itself. Beyond the main method here, we're doing all of this inside of one main method. Something called the main method, though. And so what happened before we had this try catch block here is that the exception was thrown and it made its way outside of the main method. And then the JVM were the thing that called our main method handled it. And in its handling of the method, it displayed the stack trace just like we're doing now.

Now, because we are now handling this method with try and catch here, as soon as we call the division by zero line, the App does not crash because we're handling it and we're not killing the application as a result of it. This allows us to proceed to the next line and print the array content.

But in this particular case, it's not really a good idea at all, because with this divide by zero happening here, we're not able to fix anything. All we're doing is just saying, Oh yeah, that divide by zero occurred. Let's move on. It's kind of like if your house was on fire and you're driving by and you see your house is on fire and you say, Oh, look, my house is on fire and then you just keep driving. 🔥

**We are not really handling the exception in any way.**

All I'm doing is just noting yet there's that exception and I'm printing it out. Oftentimes, the better thing to do in a case like this might be to just let the program die, which would be the default behavior of the JVM.

## Example Scenario #2
```java
public class ExceptionTests {
    public static void main(String[] args) {
        String[] array = {"one", "two", "three"};
        int num = 1;
        try {
            System.out.println(array.length / num);
            System.out.println(array[3]);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("The array index was thrown here");
        }
        System.out.println("You made it to the end");
    }
}
```
output:
```bash
3
The array index was thrown here
You made it to the end

Process finished with exit code 0
```

So we are able to say what these catch blocks here, what type of exception we are expecting to catch and then do something accordingly to that. In this example we have something called array index out of bounds exception. So this is a subclass of exception. And one interesting thing we can do with this is we could actually be much more specific in what exception we're actually looking for here.

So what happens if we change num back to zero?

```java
public class ExceptionTests {
    public static void main(String[] args) {
        String[] array = {"one", "two", "three"};
        int num = 0;
        try {
            System.out.println(array.length / num);
            System.out.println(array[3]);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("The array index was thrown here");
        }
        System.out.println("You made it to the end");
    }
}
```
output:
```bash
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at section11_loose_ends.ExceptionTests.main(ExceptionTests.java:8)

Process finished with exit code 1
```

So now we're getting an exception here, and it's the familiar divide by zero exception, which is an arithmetic exception. We're not even making it to the end of the program anymore, even though we've got to try catch block,  we're not making it to the end of the program anymore. And the reason for that is because we are no longer catching this arithmetic exception.

**Well, we never were catching an arithmetic exception. Initially, we were catching just an exception. And now we're catching an array index out of bounds exception. However, since arithmetic exception and array index out of bounds exception, both extend from exception. And we were explicitly catching exception. That catch was handling both types and so many more, by the way.  You can always catch the more generic exception data type.**

Sometimes it can also be a bad thing if you're not aware that that could be happening in general. You do actually want to catch the most specific exception data type that is meaningful for whatever it is that you're doing.

### Multiple catches

We can actually specify multiple catches:
```java
public class ExceptionTests {
    public static void main(String[] args) {
        String[] array = {"one", "two", "three"};
        int num = 0;
        try {
            System.out.println(array.length / num);
            System.out.println(array[3]);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("The array index was thrown here");
        } catch (ArithmeticException e) {
            System.out.println("This is due to the arithmetic exception being thrown");
        }
        System.out.println("You made it to the end");
    }
}
```
output:
```bash
This is due to the arithmetic exception being thrown
You made it to the end

Process finished with exit code 0
```
Because we are now handling that exception with this explicit catch here, the program in this case isn't dying. So in general, you can have as many catches as you need to handle, whatever the situations are.

What if we wanted to do basically the same error handling for both for either of these catches? Sure we just copy-paste the same code for both catches, but you always want to eliminate duplicate code, so we need to consolidate this because otherwise we have duplicated code. So what we can do is actually combine these two bits into one catch block here, and the way we can do that is simply within order. This is called multi-catch:

```java
public class ExceptionTests {
    public static void main(String[] args) {
        String[] array = {"one", "two", "three"};
        int num = 0;
        try {
            System.out.println(array.length / num);
            System.out.println(array[3]);
        } catch (ArrayIndexOutOfBoundsException | ArithmeticException e ) {
            System.out.printf("Exception type: %s. Message: %s%n", e.getClass(), e.getMessage());
            e.printStackTrace();
        }
        System.out.println("You made it to the end");
    }
}
```

output:

```bash
Exception type: class java.lang.ArithmeticException. Message: / by zero
You made it to the end
java.lang.ArithmeticException: / by zero
	at section11_loose_ends.ExceptionTests.main(ExceptionTests.java:8)

Process finished with exit code 0
```

That's at least giving us a little bit more of a hint of what exactly went wrong. Especially since now we're handling it all in the same code with the same single catch. 

### Stack Trace

So let's talk a little bit more about the Stack Trace business now. What if we create a new method with all of that code in it? All the try catch business like before, but our main method is still going to be the starting point into our program. So the main method is going to start up and we're going to call this second level method:

```java
public class ExceptionTests_v2 {
    public static void main(String[] args) {
        doSecondLevel();
        System.out.println("You made it to the end");
    }

    private static void doSecondLevel() {
        String[] array = {"one", "two", "three"};
        int num = 0;
        try {
            System.out.println(array.length / num);
            System.out.println(array[3]);
        } catch (ArrayIndexOutOfBoundsException | ArithmeticException e ) {
            System.out.printf("Exception type: %s. Message: %s%n", e.getClass(), e.getMessage());
            e.printStackTrace();
        }
    }
}
```

let's run this and see what what we get here:

```bash
Exception type: class java.lang.ArithmeticException. Message: / by zero
You made it to the end
java.lang.ArithmeticException: / by zero
	at section11_loose_ends.ExceptionTests_v2.doSecondLevel(ExceptionTests_v2.java:13)
	at section11_loose_ends.ExceptionTests_v2.main(ExceptionTests_v2.java:5)

Process finished with exit code 0
```

So we got that familiar output, but then we got at exception tests that do second level exception test Java 13, and then we see another line here. Exception tests dot main. So what this is showing us is the tracing of the path that our thread took. A thread is just like this little guy who is actually stepping through each line of our program and executing those lines, and then he can jump to new lines as needed. So when our program is running, the thread will start at line to write and it'll it'll bring in, it'll carry in, so to speak, the any arguments that we passed in. Then it sees Line five do second level and it knows that this is a method call. So then the thread jumps down to the second level method and then it starts executing each of these lines.

**Why are we seeing "you made it to the end" before we are seeing the stack trace?**

So just know that the way these stack traces are generated and the timing and order of the thread stuff, you should not expect that to print out in the exact order that you would intuitively assume it would. In this stack trace, we're seeing the most immediate method first. So it's kind of like the most recently called method will be at the top. So it's kind of backward to how a lot of people will tend to think. Intuitively you'll kind of start at the bottom. **Typically, though, with a stack trace, the line of code where the exception occurred will be at the top.**

## Runtime Exceptions

So far, we're looking at this array index out of bounds and the arithmetic exception. These two exceptions are actually both runtime exceptions, and that means that they don't actually have to be caught. As you already saw when we first started writing this, I'm going to just comment these out. Java is not forcing us to surround any of these lines with a try catch block.

And that is because of a couple of things. First off, the IDE and Java do not know at this point in time that either of these calls could result in an exception. It won't know that until runtime when we're actually running the the code. And that's one of the reasons why the type of exceptions that can be thrown from these kinds of errors are runtime exceptions. They can't easily be known before you start running. A human could analyze this particular code and see that we're passing a zero in and we're about to try to do a divide by zero. But the JVM can't really see it until you actually do it. And there's no guarantee that every time you divide by this variable that it's always going to be zero.

## Checked Exceptions

Lets take for example that Code Snippet:

```java
Long result = Files.lines(Path.of("/Users/User/Downloads/Hr5m.csv"))
```

We know for a fact the creator of this method line's method knows that it is possible that the path that we are passing into it might not be a valid path. And because he or she knew at the time of writing this method that that could happen. They declared that this method is capable of throwing an IO exception and they made their IO exception class extend not from runtime exception, but directly from exception. And because it extends directly from exception, it is a checked exception, which means that we have to do something with it. And so what we're doing here is we're surrounding all of this code inside of a try catch block:
```java
    public static void main(String[] args) {
        try {
            long startTime = System.currentTimeMillis();
            Long result = Files.lines(Path.of("/Users/Dave/Downloads/Hr5m.csv"))
                    .skip(1)
//                    .limit(10)
                    .map(s -> s.split(","))
                    .map(arr -> arr[25])
//                    .forEach(System.out::println);
                    .collect(Collectors.summingLong(sal -> Long.parseLong(sal)));

            long endTime = System.currentTimeMillis();
//            System.out.println(moneyFormat.format(result));
            System.out.printf("$%,d.00%n", result);
            System.out.println(endTime - startTime + "ms");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```
However, one other way that we could implement this is by adding the throws IOException at the end of the main Method signature:
```java
public class ExceptionTests_v4 {
    public static void main(String[] args) throws IOException {
        Files.lines(Path.of("blababla"));
    }
}
```
This looks cleaner, right? But be careful with this. So now the main Method just declared to the JVM as a whole that, there is content that is capable of throwing an IO exception, but the main Method don't want to be the one to handle that. What so what I'm going to do is I'm going to take that particular exception if it is thrown and I'm going to re throw it back to whoever called me. So whatever code calls the main method, will have to handle whatever happens with this exception now. So we're kind of passing the buck on to the code that called this main method, which in this case is the JVM itself. And so the JVM is just going to blow up all the same. So in this particular case, I suppose you're not really losing much in such a simple program like this. However, imagine if we were writing a program with a user interface and we asked the user to select a file, right? Like it's a graphical program right in like a file open dialogue.

## Finally
```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class ExceptionTests_v4 {
    public static void main(String[] args) {
        try {
            Files.lines(Path.of("blababla"));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("We are unable to open the file");
        } finally {
            System.out.println("Make sure this runs no matter what...");
        }
    }
}
```
output:
```bash
java.nio.file.NoSuchFileException: blababla
	at java.base/sun.nio.fs.UnixException.translateToIOException(UnixException.java:92)
	at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:106)
	at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:111)
	at java.base/sun.nio.fs.UnixFileSystemProvider.newFileChannel(UnixFileSystemProvider.java:181)
	at java.base/java.nio.channels.FileChannel.open(FileChannel.java:298)
	at java.base/java.nio.channels.FileChannel.open(FileChannel.java:357)
	at java.base/java.nio.file.Files.lines(Files.java:4132)
	at java.base/java.nio.file.Files.lines(Files.java:4227)
	at section11_loose_ends.ExceptionTests_v4.main(ExceptionTests_v4.java:10)
We are unable to open the file
Make sure this runs no matter what...

Process finished with exit code 0
```
So what's happening here is that sometimes depending on what you're trying to do inside of your try block, you may need to clean up or finish up some things before the program completely dies. Because as we've seen here, in many cases with a try catch block, the program is going to just die. Even though you may have that catch, you might be handling things in such a way that the program is still going to just die when it's done, but before it dies, you may want to clean a few things up.

One example of this would be in some kinds of applications where we might be opening files to read or write to them, or making connections to databases or things of that sort. If something goes wrong with any of those opening of connections and files and stuff, we don't want to just leave the file that we may have been able to open in in in a strange state. We want to ensure that even if something went wrong while we were reading data from somewhere, we still close that connection or closed that file gracefully and we can do that in a final block.

So what happens with the final block is that any code that we put here is strongly attempted by the JVM to be executed regardless of what went wrong in in the try in the try block.

## Optimizing the Example Szenarios

What do we do to protect ourselves from this type of situation? And it's actually not that hard to protect ourselves from some of these situations in this particular. For example we could simply just make sure that num can not be zero and that the array length is not exceeded with if blocks:
```java
public class ExceptionTests_v5 {
    public static void main(String[] args) {
        doSecondLevel(0);
        System.out.println("You made it to the end");
    }

    private static void doSecondLevel(int num) {
        String[] array = {"one", "two", "three"};
        if (num != 0) {
            System.out.println(array.length / num);
        }
        int index = 3;
        if (index < array.length) {
            System.out.println(array[3]);
        }
    }
}
```
But please, don't put every line wrapped in some defensive if block. You got to try to find a balance.

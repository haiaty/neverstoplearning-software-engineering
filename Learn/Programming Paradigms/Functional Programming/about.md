

* [knowledge aggregation](#functional-programming)
* [resources](#resources)

# Functional Programming

Functional programming is not a framework or a tool, but a way of writing code; thinking functionally is radically different from thinking in object-oriented or imperative terms. 

In simple terms, FP is a software development style that places a major emphasis on the use of functions. In this regard, you might consider it a procedural programming paradigm (based on procedures, subroutines, or functions), and at its core it is, but with very different philosophies. You might say, “well, I already use functions on a day-to-day basis at work; what’s the difference?” As I mentioned earlier, functional programming requires you to think a bit differently about how to approach the tasks you are facing. Your goal will be to abstract entire control flows and operations on data with functions in order to avoid side effects and reduce mutation of state in your application. 

While based on very simple concepts, FP requires a shift in the way you think about a problem. This isn’t a new tool, a library, or an API, but a different approach to problem solving that will become intuitive once you’ve understood its basic principles, design patterns, and how they can be used against the most complex tasks. Also, it’s not an all or nothing solution. 

Thinking functionally involves treating parameters as not just simple scalar values but also as functions themselves that provide additional functionality; it also involves using functions or (callables) as just pieces of data that can be passed around anywhere. The end result is that we end up evaluating and combining lots of functions together that individually don’t add much value, but together solve entire programs.

Above all, is important the process of decomposing a program into smaller pieces that are more reusable, reliable, and easier to understand; then they are combined to form an entire program that is easier to reason about as a whole. Thinking about 
simple functions individually is very easy, and separating the concerns makes your programs easier to test. Every functional program follows this fundamental principle.

#### Pure functions

Functional programming is based on the premise that you will build immutable programs based on pure functions as the building blocks of your business logic. A pure function has the following qualities:

It depends only on the input provided and not on any hidden or external state that may change during its evaluation or between calls.

It doesn’t inflict changes beyond its scope, like modifying a global object or a parameter passed by reference, after its run.

In functional programming, functions should behave like reusable artifacts that can be evaluated in any order and continue to yield correct results.

Generally speaking, functions have side effects when reading from or writing to external resources. 

Generally, the goal is to create functions that do one thing and combine them together instead of creating large monolithic functions.

##### Declarative coding

Functional programming is foremost a declarative programming paradigm. This means they express a logical connection of operations without revealing how they’re implemented or how data actually flows through them.

#### Designing for immutability and statelessness

Coding with immutable variables has many benefits such as:

One of the main causes of bugs in software is when the state of an object inadvertently changes, or its reference becomes null. Immutable objects can be passed around to any function and their states will always remain the same. You can count on having the peace mind that state is only permitted to grow but never change. It eases the “cognitive load” (the amount of state to keep track of in your head) of any single component of your system. Immutable data structures are important in shared memory multithreaded applications.

# Resources

[Functional PHP](https://leanpub.com/functional-php/read)

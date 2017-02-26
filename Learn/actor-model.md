



The Actor model defines a relatively simple but powerful way for designing and implementing applications that can distribute and share work across all system resources—from threads and cores to clusters of servers and data centers. 
 
The Actor model is used to provide an effective way for building applications that perform tasks with a **high level of concurrency** and increasing levels of **resource efficiency**. Importantly, the Actor model also has well-defined ways for **handling errors and failures gracefully**, ensuring a level of **resilience** that isolates issues and prevents cascading failures and massive downtime.

Actors **exchange messages** with each other to communicate. These messages are sent **asynchronously**, very much in the way humans exchange text messages. Due to this asynchronous behavior, actors are designed not only for the expected happy path, they are also **designed and implemented to handle the unhappy path**. In the Actor model, failure handling and recovery is an architectural feature, and not treated as an afterthought

This simplicity and intuitive behavior of the actor as a buildingblock allows for designing and implementing very elegant, highly efficient applications that natively know **how to heal themselves when failures occur**.

The Actor model is receiving “renewed interest as cloud concurrency challenges grow” in enterprises building **microservices architectures**.

## implementation

The only way to contact a software actor **is to send it a message**, much like how we exchange text messages on mobile devices. 

When an actor sends a message to another actor, **it does not wait for a response**; it is free to do other things, such as send messages to other actors.

Actors that can send and receive messages are prepared for multiple possible outcomes: a response may be received quickly, there may be no response at all, or there may be a response that is too late to be useful. The point here is that **the actor may be implemented to handle one or more of these possible outcomes**.


# Resources

* [Designing reactive systems The Role Of Actors In Distributed Architecture By Hugh McKee]

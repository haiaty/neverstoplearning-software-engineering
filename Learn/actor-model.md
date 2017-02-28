



The Actor model defines a relatively simple but powerful way for designing and implementing applications that can distribute and share work across all system resources—from threads and cores to clusters of servers and data centers. 
 
The Actor model is used to provide an effective way for building applications that perform tasks with a **high level of concurrency** and increasing levels of **resource efficiency**. Importantly, the Actor model also has well-defined ways for **handling errors and failures gracefully**, ensuring a level of **resilience** that isolates issues and prevents cascading failures and massive downtime.

Actors **exchange messages** with each other to communicate. These messages are sent **asynchronously**, very much in the way humans exchange text messages. Due to this asynchronous behavior, actors are designed not only for the expected happy path, they are also **designed and implemented to handle the unhappy path**. In the Actor model, failure handling and recovery is an architectural feature, and not treated as an afterthought

This simplicity and intuitive behavior of the actor as a buildingblock allows for designing and implementing very elegant, highly efficient applications that natively know **how to heal themselves when failures occur**.

The Actor model is receiving “renewed interest as cloud concurrency challenges grow” in enterprises building **microservices architectures**.

## implementation

The only way to contact a software actor **is to send it a message**, much like how we exchange text messages on mobile devices. 

When an actor sends a message to another actor, **it does not wait for a response**; it is free to do other things, such as send messages to other actors.

Actors that can send and receive messages are prepared for multiple possible outcomes: a response may be received quickly, there may be no response at all, or there may be a response that is too late to be useful. The point here is that **the actor may be implemented to handle one or more of these possible outcomes**.

# Actor Supervisors and Workers

Actors may **create other actors**. When one actor creates another actor, the creator is known as the supervisor and the created actor is known as the worker. Worker actors may be created for many reasons, but among the most common reasons **is for delegating work**. The supervisor creates one or more worker actors and **delegates work to them**.

The supervisor may send messages to each of its workers, which run **independently and concurrently**.


There are a number of benefits to this approach of delegating the work out to worker actors. One key benefit is **performance**. Because the workers run concurrently, the performance is based on the over Actor Supervisors and Workers all time it takes for the supervisor to respond and reduced to the response time of the **slowest worker**.

The asynchronous approach also scales more efficiently.The **cost of adding more workers is much less** than in a synchronous implementation.

Another benefit of the asynchronous delegated approach is related to **failure resilience and recovery**.  If a
worker does not respond in time, then the supervisor decides how to proceed. If a worker runs into a problem, it suspends itself and **notifies its supervisor** of the failure. When the supervisor is notified **it determines what should be done ** to handle the problem and how to get the worker actor back into a healthy state.


# Actor System

Actors exist within an actor system, and the process of actors sending asynchronous messages to each other is handled by the actor system.

In an actor system, threads are allocated to actors that have messages to process. When the actor has no messages to process, **the thread is allocated to other actors that have messages to process** and that have something to do, so that they are not sitting idle while waiting for something like an I/O to complete.

Even when an actor still has more messages to process, it is given a limited amount of time with a thread. **The system is costantly moving the threads around to various actors trying to maintain a fair distribution of thread usage** without letting some actors dominate their time with threads or, conversely, let some actors be starved for attention.

The end result is that asynchronous actor systems can **handle many more concurrent requests with the same amount of resources ** since the limited number of threads never sit idle while waiting for I/O operations to complete.

# Resources

* Designing reactive systems The Role Of Actors In Distributed Architecture By Hugh McKee

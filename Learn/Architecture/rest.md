

REST is an architectural style for building distributed systems based on hypermedia. 

A primary advantage of the REST model is that it  **does not bind the implementation of the model or the client applications** that access it to any specific implementation. For example, a REST web service could be implemented by using the Microsoft ASP.NET Web API, and client applications could be developed by using any language and toolset that can generate HTTP requests and parse HTTP responses.

REST is actually **independent of any underlying protocol** and is not necessarily tied to HTTP. However, most common implementations of systems that are based on REST utilize HTTP as the application protocol for sending and receiving requests.

The important point to understand **is that REST defines a stateless request model**. HTTP requests should be independent and may occur in any order, so attempting to retain transient state information between requests is not feasible. The only place where information is stored is in the resources themselves, and each request should be an atomic operation. Effectively, a REST model implements a finite state machine where a request transitions a resource from one well-defined non-transient state to another.
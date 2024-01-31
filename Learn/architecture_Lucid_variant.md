# proposed architecture: Lucid principles + FaaS + Functional Composition + hexagonal architecture

---

## Context

How to develop our applications to be maintainable and simple over time.

## Decision

We decided to apply different concepts taken from experience from different architectures and projects and added them
upon Lucid principles.

Lucid principles can be found here: https://docs.lucidarch.dev/principles

The principles that we use:

* **Features shall serve a single purpose**: Favour creating as many of them as you wish rather than complicating a
  single one.
* **Features shall not call other features**: Run as many jobs and operations as you like, but never a feature.
* **Jobs shall perform a single task**: No job should do two things at a time, it will only get confusing the more you
  do it.
* **Jobs shall not call other jobs**: This is your business logic, keep it concise by avoiding nesting hell.
* **Operations shall not call other operations**: Run as many jobs as you like, but never any other unit.
* **Domains shouldn't cross**: When working within a domain, strive to preserve the boundaries by not using
  functionality from other domains. If you encounter a case where you should, consider **Foundation**, **Operations**
  and **Features** by rethinking your design.
* **Write code that humans can read**: Machines will run it nonetheless, it is us who will suffer.

For more concepts of Lucid see:

* **Features**: https://docs.lucidarch.dev/features/
* **Operations**: https://docs.lucidarch.dev/operations/
* **Jobs**: https://docs.lucidarch.dev/jobs/

BEWARE: the concept of **services** in the Lucid documentation is not what we mean in our architecture: for us a **service** is a unit that may compose different **features** and is callable from **ports** (HTTP (e.g. Fastify
handlers), CLI, etc.); think of it like a FaaS (Function as a Service), a thing that you may call and does its things.

----

We have added other principles to it:

* You can compose your functionality in services and features. For example a service could be composed of  2 features + 1 operation + 1 job.
  Another example: a feature may be composed of 3 operations + 2 jobs...
  Another example: a service could be composed of only 1 feature
  Anoher example: a service could be composed of 1 feature + 1 job
  And so on...
 It's kind like functional composition ("In computer science, function composition is an act or mechanism to combine simple functions to build more complicated ones."),
but here is these building blocks (jobs, operations, features) that is being combined to build complicated ones.

* If you need only to call a **Feature** as it is, without using data from other **features**, and without the need to
  do something in particular, you can use **features** directly from the `ports` (ex: Fastify handlers, hooks, or CLI,
  or even from other modules), example:
  ``` javascript
  fastify.get("example", async function(req, resp) {
    let input = req.body;
    let data = ExampleFeature(input);
    // elaborate data
    return resp.send(data);
  });
  ```
* If you have a more complex use case, where you want to put together more **features** or do something more after **features**, you MUST use a **service**.
* **Services**, which we mean a function that may call different **features** and is called by the framework (for
  example by Fastify handlers), example:
  ``` javascript
  fastify.get("example", async function(req, resp) {
    let input = req.body;
    let data = ExampleService(input);
    // elaborate data
    return resp.send(data);
  });
  ```
* We have also added a folder `drivers`, in the root of the project. The meaning of the files inside it is to provide
  connections to external world, like database or HTTP clients. It can be used by **Jobs**, **operations**, **features**, **services**.
  (concepts elaborated after taken from HEXAGONAL ARCHITECTURE)

* The only way to talk to the application, or the module if it is a modular monolith, is using **services** or **features**. It is forbidden to call an **operation** or **job** from another module.
* Reduce at the minimun the cogntive load by using semantic variables names, adding semantic comments, separating visualy part of the code
* Avoid state or a shared global state. All things needed for jobs/services/operations should be passed to it as its arguments


ATTENTION: we decided not to treat framework-related details and to let it be handled case by case. We want to be more
framework-agnostic as possible inside our "core" or "business logic" or "everything inside src".


## Consequences

More maintainable code and easier to reason about after returning on the project after a long time.

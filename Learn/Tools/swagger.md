

Swagger is a powerful open source framework backed by a large ecosystem of tools that helps you design, build, document, and consume your RESTful APIs.

Swagger is a set of rules (in other words, a specification) for a format describing REST APIs. The format is both machine-readable and human-readable. As a result, it can be used to share documentation among product managers, testers and developers, but can also be used by various tools to automate API-related processes.

# examples

```
swagger: "2.0"
info:
  version: "1.0"
  title: "Hello World API"
paths:
  /hello/{user}:
    get:
      description: Returns a greeting to the user!
      parameters:
        - name: user
          in: path
          type: string
          required: true
          description: The name of the user to greet.
      responses:
        200:
          description: Returns the greeting.
          schema:
            type: string
        400:
          description: Invalid characters in "user" were provided.
```

In the example above we describe a simple “Hello World API”. It has a single URL exposed – “/hello/{user}”. “user” is the single parameter to the API, defined as part of the path, and we say it’s a string. We also describe the successful response, and mention it’s a string as well. A generic 400 error can be received if the “user” contained invalid characters. You can also see that throughout the operation itself we provide additional documentation.

# Resources

* [Swagger explained ](http://bfanger.nl/swagger-explained/#/infoObject)
* [Getting started with swagger - what is swagger?](http://swagger.io/getting-started-with-swagger-i-what-is-swagger/)
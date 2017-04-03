

REST is an architectural style for building distributed systems based on hypermedia. 

A primary advantage of the REST model is that it  **does not bind the implementation of the model or the client applications** that access it to any specific implementation. For example, a REST web service could be implemented by using the Microsoft ASP.NET Web API, and client applications could be developed by using any language and toolset that can generate HTTP requests and parse HTTP responses.

REST is actually **independent of any underlying protocol** and is not necessarily tied to HTTP. However, most common implementations of systems that are based on REST utilize HTTP as the application protocol for sending and receiving requests.

The important point to understand **is that REST defines a stateless request model**. HTTP requests should be independent and may occur in any order, so attempting to retain transient state information between requests is not feasible. The only place where information is stored is in the resources themselves, and each request should be an atomic operation. Effectively, a REST model implements a finite state machine where a request transitions a resource from one well-defined non-transient state to another.

The purpose of REST **is to map business entities and the operations that an application can perform on these entities to the physical implementation of these entities**, but a client should not be exposed to these physical details.

One of the primary motivations behind REST is that **it should be possible to navigate the entire set of resources without requiring prior knowledge of the URI scheme**. Each HTTP GET request should return the information necessary to find the resources related directly to the requested object through hyperlinks included in the response, and it should also be provided with information that describes the operations available on each of these resources. This principle is known as **HATEOAS, or Hypertext as the Engine of Application State**. . The system is effectively a finite state machine, and the response to each request contains the information necessary to move from one state to another; no other information should be necessary.
## Designing Restful Apis

- you should seek to keep URIs relatively simple. Bear in mind that once an application has a reference to a resource, it should be possible to use this reference to find items related to that resource.
- Adopt a consistent naming convention in URIs; in general it helps to use plural nouns for URIs that reference collections.
- The URIs exposed by a REST web service should be based on nouns (the data to which the web API provides access) and not verbs (what an application can do with the data).
- Avoid designing a REST interface that mirrors or depends on the internal structure of the data that it exposes. REST is about more than implementing simple CRUD (Create, Retrieve, Update, Delete) operations over separate tables in a relational database.
- Avoid requiring resource URIs more complex than collection/item/collection.
- It may be beneficial to denormalize data and combine related information together into bigger resources that can be retrieved by issuing a single request. This in order to avoid load on the web server and avoid a client to submit several requests to find all the data. However, you need to balance this approach against the overhead of fetching data that might not be frequently required by the client.
- Avoid introducing dependencies between the web API to the structure, type, or location of the underlying data sources. For example, if your data is located in a relational database, the web API does not need to expose each table as a collection of resources. Think of the web API as an abstraction of the database, and if necessary introduce a mapping layer between the database and the web API. In this way, if the design or implementation of the database changes (for example, you move from a relational database containing a collection of normalized tables to a denormalized NoSQL storage system such as a document database) client applications are insulated from these changes.
-  it might not be possible to map every operation implemented by a web API to a specific resource. You can handle such non-resource scenarios through HTTP GET requests that invoke a piece of functionality and return the results as an HTTP response message.
- Use Versioning. select one strategy: URI versioning, Query string versioning,Header versioning, Media type versioning 

URI versioning: Each time you modify the web API or change the schema of resources, you add a version number to the URI for each resource. The previously existing URIs should continue to operate as before, returning resources that conform to their original schema. This versioning mechanism is very simple but depends on the server routing the request to the appropriate endpoint. However, it can become unwieldy as the web API matures through several iterations and the server has to support a number of different versions. Also, from a purist’s point of view, in all cases the client applications are fetching the same data (customer 3), so the URI should not really be different depending on the version. This scheme also complicates implementation of HATEOAS as all links will need to include the version number in their URIs.

- It is highly recommended to always return error messages in the same format so that the consumer does not have to handle different semantics on different resources
- paginate all results that return a list of items
- consider implementing a rate-limit early on.
- You must provide a good documentation
- The API consumer doesn't always need the full representation of a resource. The ability select and chose returned fields goes a long way in letting the API consumer minimize network traffic and speed up their own usage of the API.
  Use a fields query parameter that takes a comma separated list of fields to include. For example, the following request would retrieve just enough information to display a sorted listing of open tickets:
GET /tickets?fields=id,subject,customer_name,updated_at&state=open&sort=-updated_at

-f you're following the approach in this post, then you've embraced JSON for all API output. Let's consider JSON for API input.
 
 Many APIs use URL encoding in their API request bodies. URL encoding is exactly what it sounds like - request bodies where key value pairs are encoded using the same conventions as one would use to encode data in URL query parameters. This is simple, widely supported and gets the job done.
 
 However, URL encoding has a few issues that make it problematic. It has no concept of data types. This forces the API to parse integers and booleans out of strings. Furthermore, it has no real concept of hierarchical structure. Although there are some conventions that can build some structure out of key value pairs (like appending [ ] to a key to represent an array), this is no comparison to the native hierarchical structure of JSON.
 
 If the API is simple, URL encoding may suffice. However, complex APIs should stick to JSON for their API input. Either way, pick one and be consistent throughout the API.
 
 An API that accepts JSON encoded POST, PUT & PATCH requests should also require the Content-Type header be set to application/json or throw a 415 Unsupported Media Type HTTP status code.
 
- Pretty print by default & ensure gzip is supported
  
  An API that provides white-space compressed output isn't very fun to look at from a browser. Although some sort of query parameter (like ?pretty=true) could be provided to enable pretty printing, an API that pretty prints by default is much more approachable. The cost of the extra data transfer is negligible, especially when you compare to the cost of not implementing gzip.
  
  Consider some use cases: What if an API consumer is debugging and has their code print out data it received from the API - It will be readable by default. Or if the consumer grabbed the URL their code was generating and hit it directly from the browser - it will be readable by default. These are small things. Small things that make an API pleasant to use!
  
  But what about all the extra data transfer?
  
  Let's look at this with a real world example. I've pulled some data from GitHub's API, which uses pretty print by default. I'll also be doing some gzip comparisons:
  
  
  $ curl https://api.github.com/users/veesahni > with-whitespace.txt
  $ ruby -r json -e 'puts JSON JSON.parse(STDIN.read)' < with-whitespace.txt > without-whitespace.txt
  $ gzip -c with-whitespace.txt > with-whitespace.txt.gz
  $ gzip -c without-whitespace.txt > without-whitespace.txt.gz
  The output files have the following sizes:
  
  without-whitespace.txt - 1252 bytes
  with-whitespace.txt - 1369 bytes
  without-whitespace.txt.gz - 496 bytes
  with-whitespace.txt.gz - 509 bytes
  In this example, the whitespace increased the output size by 8.5% when gzip is not in play and 2.6% when gzip is in play. On the other hand, the act of gzipping in itself provided over 60% in bandwidth savings. Since the cost of pretty printing is relatively small, it's best to pretty print by default and ensure gzip compression is supported!
  
  To further hammer in this point, Twitter found that there was an 80% savings (in some cases) when enabling gzip compression on their Streaming API. Stack Exchange went as far as to never return a response that's not compressed!
  

# Examples

Suppose that a quick analysis of an ecommerce system shows that there are two collections in which client applications are likely to be interested: orders and customers. Each order and customer should have its own unique key for identification purposes. The URI to access the collection of orders could be something as simple as /orders, and similarly the URI for retrieving all customers could be /customers. Issuing an HTTP GET request to the /orders URI should return a list representing all orders in the collection encoded as an HTTP response:

```
GET http://adventure-works.com/orders HTTP/1.1
...
```

The response shown below encodes the orders as a JSON list structure:

```
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
[{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1},{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2},{"orderId":3,"orderValue":16.60,"productId":2,"quantity":4},{"orderId":4,"orderValue":25.90,"productId":3,"quantity":1},{"orderId":5,"orderValue":99.90,"productId":1,"quantity":1}]
```

To fetch an individual order requires specifying the identifier for the order from the orders resource, such as /orders/2:

```
GET http://adventure-works.com/orders/2 HTTP/1.1
...
```

```
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2}
```

## example of HATEOAS

to handle the relationship between customers and orders, the data returned in the response for a specific order should contain URIs in the form of a hyperlink identifying the customer that placed the order, and the operations that can be performed on that customer.

```
GET http://adventure-works.com/orders/3 HTTP/1.1
Accept: application/json
...
```

The body of the response message contains a links array that specifies the nature of the relationship (Customer), the URI of the customer (http://adventure-works.com/customers/3), how to retrieve the details of this customer (GET), and the MIME types that the web server supports for retrieving this information (text/xml and application/json). This is all the information that a client application needs to be able to fetch the details of the customer. Additionally, the Links array also includes links for the other operations that can be performed, such as PUT (to modify the customer, together with the format that the web server expects the client to provide), and DELETE.
The Links array should also include self-referencing information pertaining to the resource that has been retrieved. These links have been omitted from the previous example, but are highlighted in the following code. Notice that in these links, the relationship self has been used to indicate that this is a reference to the resource being returned by the operation:

```
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[{"rel":"self","href":" http://adventure-works.com/orders/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" self","href":" http://adventure-works.com /orders/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"self","href":" http://adventure-works.com /orders/3", "action":"DELETE","types":[]},{"rel":"customer",
"href":" http://adventure-works.com /customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" customer" (customer links omitted)}]} 
```

# Resources

* [Some rest concepts from API Design microsoft](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
* [10 Design Tips For APIs](https://phraseapp.com/blog/posts/best-practice-10-design-tips-for-apis/)
* [Best practices for a pragmatic restful api](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
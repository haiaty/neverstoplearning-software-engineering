# HTTP
## Get all the namespaces 

to make a request to know which namespaces are present  using HTTP you should use the url:

http://127.0.0.1:8000/sql

headers:

  - Accept: application/json

  - basic auth using the username and the password


and add a body of type 'raw' having this statement:

``
INFO FOR KV;         #this take all the information
``

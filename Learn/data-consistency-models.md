 
## Strong consistency model ## 

the database guarantees that the applications will always read the last write. 
 
## Sequential consistency ##

the database assures that the order of data you read is consistent with the order in which it was written to the database. 

## Eventual consistency model ##

the distributed database promises to synchronize and consolidate the data between the database replicas behind the scenes. Therefore, if you write your data to one database replica and read it from another, it’s possible that you won’t read the latest copy of the data.



source: https://www.infoworld.com/article/3305321/nosql/when-to-use-a-crdt-based-database.html

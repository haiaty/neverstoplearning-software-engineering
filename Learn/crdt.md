

CRDT stands for Conflict-free Replicated Data Type. In sum, CRDTs provide a way for you to merge concurrent modifications, always, in any order. SECs guarantee all actors will eventually converge to the same state without data loss. These guarantees are tailor-made for a distributed asynchronous world.

How do CRDTs provide Autonomy and Consistency? By relaxing ACID consistency into what is called Strong Eventual Consistency (SEC).

### NO SQL IMPLEMENTING CRDT ###



ANTIDOTE
https://antidotedb.gitbook.io/documentation/
https://github.com/AntidoteDB/antidote

ROSHI
- https://github.com/soundcloud/roshi
https://developers.soundcloud.com/blog/roshi-a-crdt-system-for-timestamped-events


RIAK
https://github.com/basho/riak

## sources ###
http://muawia.com/when-to-use-a-crdt-based-database/
https://serverless.com/blog/crdt-explained-supercharge-serverless-at-edge/

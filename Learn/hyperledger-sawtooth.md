## Utils

### REST API

##### see the batch list from the beginning

usare 'reverse' senza nulla dopo '='  ,  example:

http://10.122.88.24:8008/batches?limit=1000&reverse=

----

### WITH DOCKER

##### See validator logs to see if a transaction handler has connected

```
 docker exec -it <container_id> tail -f /var/log/sawtooth/validator-debug.log
```

##### See the logs of the rest api

```
docker exec -it ce49762117e8 tail -f /var/log/sawtooth/rest_api-debug.log
```

### SAWTOOTH COMMANDS SHELL

##### see the list of sawtooth network settings.

```
sawtooth settings list  --url http://rest-api:8008
```

Da <https://sawtooth.hyperledger.org/docs/1.2/sysadmin_guide/setting_allowed_txns.html> 

---

##### See the list of blocks from shell container

```
sawtooth block list --url http://rest-api:8008
```
----

##### See block details

```
sawtooth block show --url http://rest-api:8008 {BLOCK_ID}
```

----

#### See the state data on the blockchain

```
sawtooth state list --url http://rest-api:8008
```

---

##### See the data of an address (a node in the merkle-radix database)

```
sawtooth state show --url http://rest-api:8008 000000a87cb5eafdcca6a8c983c585ac3c40d9b1eb2ec8ac9f31ff5ca4f3850ccc331a
```

The output shows the bytes stored at that address and the block ID of the "chain head" that the current state is tied to,

Da <https://sawtooth.hyperledger.org/docs/1.2/app_developers_guide/installing_sawtooth.html> 

---

### MISCELLANOUS

##### Where are logs

In each container, the Sawtooth log files for that component are stored in the directory 

```
/var/log/sawtooth
```

Da <https://sawtooth.hyperledger.org/docs/1.2/app_developers_guide/installing_sawtooth.html>

## CONSENSUS
### PBFT 
#### How PBFT works

https://www.hyperledger.org/blog/2019/02/13/introduction-to-sawtooth-pbft

https://sawtooth.hyperledger.org/docs/1.2/pbft/introduction-to-sawtooth-pbft.html

https://github.com/hyperledger/sawtooth-rfcs/blob/main/text/0019-pbft-consensus.md

#### Trasanction per seconds calculation

To calculate TPS, you have to know 
* the block time (Block time is the average time it takes to create a new block in a chain.)
* the average transaction size, 
* and the block size. 

The TPS may vary on sawooth based on different factors:

* how many transactions there are per batch
* how many batches there are per block. A block having 3 batches with every batch having between 10-12 transactions is different from a block having only one batch and only one transaction.
* the latency of network for 2f + 1 nodes communication

##### ===> a TPS tranaction test results (with a network of 4 nodes)


Block size: 31k

26k (preso dalla dimensione risposta del payload dalla chiamata via http alla rest per singolo blocco)
31k (preso dalla dimensione dalla risposta del commando 

sawtooth block show --url http://rest-api:8008 43ecccd7888d94d0bb6b8ff16df3a037e2851f46def6748547b85c41098f619d1fccff78fd50096c918dfa77dc82f54d2f2bee27378469777f4204b05f036490 | wc -c )


Average Transaction size:  450 byte

Block time (time to produce a block): 

SENZA LIMITE NEL BATCH PER BLOCK: Piu o meno dai 3 ai 4 sec of block time so (31k/450)/3 or 4   -----> dai 17 TPS ai 30 TPS sec
 
times of blocks
(4,3 + 1 + 2,1+ 1,9 ,2, 3 + 4,2 +6,8 + 9,9 + 3,3 + 3,3) /10


WITH A LIMIT OF 1 BATCH PER BLOCK AND 1 TRANSACTION PER BATCH:
(1,2 + 0.99 + 1 + 1 + 0,98 + 1 + 0.95 + 1,05 + 0,96)   ~~  1 sec   ----> (31k/450)/1 = 68 TPS


 

#### How to configure PBFT

https://sawtooth.hyperledger.org/docs/1.2/pbft/configuring-pbft.html

##### how to change configuration 'max_batches_per_block'

NOTE: if you are using docker, you can enter into the validator container to execute the command
```
sawset proposal create --url <url_to_rest_api> --key <path_to_private_key example: /etc/sawtooth/keys/validator.priv> sawtooth.publisher.max_batches_per_blo
ck=1
```


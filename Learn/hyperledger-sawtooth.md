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

#### How to configure PBFT

https://sawtooth.hyperledger.org/docs/1.2/pbft/configuring-pbft.html


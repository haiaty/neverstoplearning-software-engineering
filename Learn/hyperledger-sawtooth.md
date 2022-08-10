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

----

### POET

##### configuring a network with POET and docker

1. In the node where you will create the genesis node:

- create folder poet-sawtooth-node
- cd poet-sawtooth-node && touch docker-compose.yml

copy this content in the 'docke-compose.yml'

```
# Copyright 2018 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ------------------------------------------------------------------------------

version: "2.1"

volumes:
  poet-shared:

services:
  shell:
    image: hyperledger/sawtooth-shell:nightly
    container_name: sawtooth-shell-default
    entrypoint: "bash -c \"\
        sawtooth keygen && \
        tail -f /dev/null \
        \""

  validator-0:
    image: hyperledger/sawtooth-validator
    container_name: sawtooth-validator-default-0
    expose:
      - 4004
      - 5050
      - 8800
    ports:
      - "4004:4004"
      - "8800:8800"
    volumes:
      - poet-shared:/poet-shared
      - /path/to/poet-sawtooth-node/data:/var/lib/sawtooth/ #to backup ledger
      - /path/to/poet-sawtooth-node/configs:/etc/sawtooth
    command: |
      bash -c "chmod +x /etc/sawtooth/poet-start-run-inside-docker.sh && /etc/sawtooth/poet-start-run-inside-docker.sh"
    environment:
      PYTHONPATH: "/project/sawtooth-core/consensus/poet/common:\
        /project/sawtooth-core/consensus/poet/simulator:\
        /project/sawtooth-core/consensus/poet/core"
    stop_signal: SIGKILL


  rest-api-0:
    image: hyperledger/sawtooth-rest-api
    container_name: sawtooth-rest-api-default-0
    expose:
      - 8008
    ports:
      - "8008:8008"
    command: |
      bash -c "
        sawtooth-rest-api \
          --connect tcp://validator-0:4004 \
          --bind 0.0.0.0:8008
      "
    stop_signal: SIGKILL

  settings-tp-0:
    image: hyperledger/sawtooth-settings-tp:nightly
    container_name: sawtooth-settings-tp-default-0
    expose:
      - 4004
    command: settings-tp -v -C tcp://validator-0:4004
    stop_signal: SIGKILL

  poet-engine-0:
    image: hyperledger/sawtooth-poet-engine:nightly
    container_name: sawtooth-poet-engine-0
    volumes:
      - poet-shared:/poet-shared
      - /path/to/poet-sawtooth-node/configs/keys:/poet-sawtooth-node/keys
    command: "bash -c \"\
        if [ ! -f /poet-shared/poet-enclave-measurement ]; then \
            poet enclave measurement >> /poet-shared/poet-enclave-measurement; \
        fi && \
        if [ ! -f /poet-shared/poet-enclave-basename ]; then \
            poet enclave basename >> /poet-shared/poet-enclave-basename; \
        fi && \
        if [ ! -f /poet-shared/simulator_rk_pub.pem ]; then \
            cp /etc/sawtooth/simulator_rk_pub.pem /poet-shared; \
        fi && \
        while [ ! -f /poet-sawtooth-node/keys/validator.priv ]; do sleep 1; done && \
        poet registration create -k /poet-sawtooth-node/keys/validator.priv -o /poet-shared/poet.batch && \
        cp /poet-sawtooth-node/keys/validator.priv /etc/sawtooth/keys/validator.priv &&
        poet-engine -C tcp://validator-0:5050 --component tcp://validator-0:4004 \
    \""

  poet-validator-registry-tp-0:
    image: hyperledger/sawtooth-poet-validator-registry-tp
    container_name: sawtooth-poet-validator-registry-tp-0
    expose:
      - 4004
    command: poet-validator-registry-tp -C tcp://validator-0:4004
    environment:
      PYTHONPATH: /project/sawtooth-core/consensus/poet/common
    stop_signal: SIGKILL


```

the content of 'poet-start-run-inside-docker.sh' is


``` bash
#---------------------------------
# Check if we need to generate keys
#--------------------------------

if [ ! -e /etc/sawtooth/keys/validator.priv ]; then
        sawadm keygen --force
else
  echo 'Keys already created'
fi



#---------------------------------
# WAIT FOR POET ENGINE TO SETUP
#---------------------------------

# we must wait the container 'poet-engine' to generetae these files
# since we need them to start a new poet network
echo "waiting poet-enclave-measurement generation"
while [ ! -f /poet-shared/poet-enclave-measurement ]; do sleep 1; done

echo "waiting poet-enclave-basename generation"
while [ ! -f /poet-shared/poet-enclave-basename ]; do sleep 1; done

echo "waiting poet.batch generation"
while [ ! -f /poet-shared/poet.batch ]; do sleep 1; done

echo "waiting simulator_rk_pub.pem generation"
while [ ! -f /poet-shared/simulator_rk_pub.pem ]; do sleep 1; done
        



if [ ! -e /var/lib/sawtooth/block-chain-id ]; then
  echo "Blockchain not found. Starting genesis operations."
  
  #---------------------------------
  # COPYING poet batch from the poet-engine-container
  #---------------------------------

  cp /poet-shared/poet.batch / 
  
  echo '---> copyied poet batch from the poet-engine-container'
  
  #---------------------------------
  # creating the genesis configs batch
  #---------------------------------
 
  sawset genesis -k /etc/sawtooth/keys/validator.priv -o config-genesis.batch
  
  echo '---> created config-genesis.batch'
  
  #---------------------------------
  # creating the configs batch having the poet consensus
  #---------------------------------
  
  sawset proposal create -k /etc/sawtooth/keys/validator.priv sawtooth.consensus.algorithm.name=PoET sawtooth.consensus.algorithm.version=0.1 sawtooth.poet.report_public_key_pem="$(cat /poet-shared/simulator_rk_pub.pem)" sawtooth.poet.valid_enclave_measurements=$(cat /poet-shared/poet-enclave-measurement) sawtooth.poet.valid_enclave_basenames=$(cat /poet-shared/poet-enclave-basename) -o config.batch
  
  echo '---> created sawtoot consensus config config.batch'
  
  #---------------------------------
  # creating the poet settings batch
  #---------------------------------
  #sawset proposal create -k /etc/sawtooth/keys/validator.priv sawtooth.poet.target_wait_time=5 sawtooth.poet.initial_wait_time=25 sawtooth.publisher.max_batches_per_block=100 -o poet-settings.batch
  
  # with some settings for small netwworks ( < 12 nodes) according to https://sawtooth.hyperledger.org/faq/consensus.html
  sawset proposal create -k /etc/sawtooth/keys/validator.priv sawtooth.poet.target_wait_time=5 sawtooth.poet.initial_wait_time=25 sawtooth.publisher.max_batches_per_block=100 sawtooth.poet.block_claim_delay=1 sawtooth.poet.key_block_claim_limit=100000 sawtooth.poet.ztest_minimum_win_count=999999999   -o poet-settings.batch
  
  echo '---> created poet settings poet-settings.batch'
  
  #---------------------------------
  # generating the genesis final batch that will be executed first time
  #---------------------------------
  
  # This command generates a serialized GenesisData protobuf message and stores it in the genesis.batch file.
  sawadm genesis config-genesis.batch config.batch poet.batch poet-settings.batch
  
  echo '---> created genesis.batch'
  
  
fi
echo "  ";


#-------------------------
# starting validator
#-------------------------

echo "Starting validator"


sawtooth-validator -v --bind network:tcp://eth0:8800 --bind component:tcp://eth0:4004 --bind consensus:tcp://eth0:5050 --peering dynamic --endpoint tcp://10.146.101.36:8800 --scheduler parallel

```


2. run docker-compose up
3. go to terminal and run all of your transaction processors / transaction families 
4. execute a transaction from a client
5. check if the block has been committed
6. stop the network: docker-compose down -v  // -v to remove the volume

If you want to have other validator nodes, go to another machine and 

1. create same folders as the genesis validator (to keep consistency)
2. configure correclty 'validator.toml' and add this content to 'docker-compose' file:

```bash

# Copyright 2018 Intel Corporation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ------------------------------------------------------------------------------

version: "2.1"

volumes:
  poet-shared:

services:
  shell:
    image: hyperledger/sawtooth-shell
    container_name: sawtooth-shell-poet
    entrypoint: "bash -c \"\
        sawtooth keygen && \
        tail -f /dev/null \
        \""

  validator:
    image: hyperledger/sawtooth-validator
    container_name: sawtooth-validator-poet
    expose:
      - 4004
      - 5050
      - 8800
    ports:
      - "8800:8800"
      - "4004:4004"
    volumes:
      - poet-shared:/poet-shared
      - /root/poet-sawtooth-node/data:/var/lib/sawtooth/ #to backup ledger
      - /root/poet-sawtooth-node/configs:/etc/sawtooth #to backup keys, policies, etc..
    command: |
      bash -c "
        if [ ! -e /etc/sawtooth/keys/validator.priv ]; then
          sawadm keygen
        fi &&
        if [ ! -e /root/.sawtooth/keys/my_key.priv ]; then
          sawtooth keygen my_key
        fi &&
        sawtooth-validator -vv
      "
    environment:
      PYTHONPATH: "/project/sawtooth-core/consensus/poet/common:\
        /project/sawtooth-core/consensus/poet/simulator:\
        /project/sawtooth-core/consensus/poet/core"
    stop_signal: SIGKILL

  rest-api:
    image: hyperledger/sawtooth-rest-api
    container_name: sawtooth-rest-api-poet
    expose:
      - 8008
    ports:
      - "8008:8008"
    command: |
      bash -c "
        sawtooth-rest-api --connect tcp://validator:4004 --bind 0.0.0.0:8008
      "
    stop_signal: SIGKILL

  settings-tp:
    image: hyperledger/sawtooth-settings-tp:nightly
    container_name: sawtooth-settings-tp-poet
    expose:
      - 4004
    command: settings-tp -v -C tcp://validator:4004
    stop_signal: SIGKILL

  poet-engine:
    image: hyperledger/sawtooth-poet-engine:nightly
    container_name: sawtooth-poet-engine
    volumes:
      - poet-shared:/poet-shared
      - /root/poet-sawtooth-node/configs/keys:/poet-sawtooth-node/keys
    command: "bash -c \"\
        while [ ! -f /poet-sawtooth-node/keys/validator.priv ]; do sleep 1; done && \
        cp -a /poet-sawtooth-node/keys /etc/sawtooth && \
        poet-engine -C tcp://validator:5050 --component tcp://validator:4004 \
    \""

  poet-validator-registry-tp:
    image: hyperledger/sawtooth-poet-validator-registry-tp:nightly
    container_name: sawtooth-poet-validator-registry-tp
    expose:
      - 4004
    command: poet-validator-registry-tp -C tcp://validator:4004
    environment:
      PYTHONPATH: /project/sawtooth-core/consensus/poet/common
    stop_signal: SIGKILL

```




---> ERRORS PRESENTED WHILE SETUP NETWORK

1)
```
[2022-08-03 14:50:35.631 ERROR    zmq_driver] Uncaught driver exception
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/sawtooth_sdk/consensus/zmq_driver.py", line 82, in _driver_loop
    result = self._process(message)
  File "/usr/lib/python3/dist-packages/sawtooth_sdk/consensus/zmq_driver.py", line 179, in _process
    'Received unexpected message type: {}'.format(type_tag))
sawtooth_sdk.consensus.exceptions.ReceiveError: Received unexpected message type: 907![image](https://user-images.githubusercontent.com/12880451/182646586-478e224d-ca62-43cd-92a4-edd0cfd7c437.png)
```

What I did:

I change the docker images of the validator not to be "nightly" and left the poet-engine docker container to "nightly"

2) Error: I had the 'genesis' validator configured to 'static' while the others were configured to 'dynamic'.
So to solve the issue I changed the genesis validator configuration from 'static' to dynamic and restarted netwrok


#### PERFORMANCE TEST

So in the end both POET and PBFT are similar in terms of tps (note that depending on configurations they can vary by a few tens),

Those at sawtooth recommend using PBFT for small networks (like up to 12 nodes) while using POET for larger networks.

Regarding tps, it must be said that it depends on the following factors

- block size - block size (in bytes). In sawtooth, a block consists of several batches (which is a set of transactions)

- transaction size (in bytes): the larger the transaction and the more transactions that can be put inside a block, then 
the smaller the tps will be

- block time: this is the time it takes to create a block.

With this data, the formula to find the TPS is:


(block size / transaction size) / block time

---



The tests I did with both were:

4 nodes with 1 transaction per batch and 1 a batch per block. 
I made the calls to create a new batch at the same time at each node (to their rest api) in the network and measured the time it took to create the individual blocks.

The data were 

PBFT

block size: 31000 bytes 
transaction size: 450 bytes
max_batch_per_block = 100
block time: 3 or 4 s

TPS = (31000 / 450)/3 or 4 = 17 TPS to 30 TPS sec


block size: 31000 bytes 
transaction size: 450 bytes
block time: 1 s
max_batch_per_block = 1

TPS = (31000 / 450)/1 = 68


POET


block size: 31000 bytes 
transaction size: 450 bytes
max_batch_per_block = 1
block time: 4.86 s

TPS = (31000 / 450)/ 4.86 = 14




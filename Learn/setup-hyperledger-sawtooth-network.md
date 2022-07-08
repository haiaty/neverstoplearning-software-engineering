In the first node (it could be a centos or whatever you want because we will use docker and docker-compose) create folder with this structure
---
NOTE: this is not a obbligated structure of folder. If you want you can change as you prefer

```bash
sawtooth-node/
    configs/
        keys/
    data/
    transaction_families/
```

**configs folder** -> where to store validator.toml configs, and other things like config.batch

**configs/keys folder** -> where to store the validator private and public key

**data folder** -> where to store the blockchain data 

**transaction_families folder** --> where to put the transaction processor(with its handlers)

---
### create the 'docker-compose.yaml' inside the 'sawtooth-node' folder

```bash
cd sawtooth-node
touch docker-compose.yaml
```
---
###  copy the content of this template inside the docker-compose.yaml file

```bash
version: '3.6'

services:

# -------------=== rest api ===-------------

  rest-api:
    image: hyperledger/sawtooth-rest-api:chime
    container_name: sawtooth-rest-api
    expose:
      - 8008
    ports:
      - "8008:8008"
    command: |
      bash -c "
        sawtooth-rest-api --connect tcp://validator:4004 --bind 0.0.0.0:8008
      "
    stop_signal: SIGKILL

# -------------=== settings tp ===-------------

  settings-tp:
    image: hyperledger/sawtooth-settings-tp:chime
    container_name: sawtooth-settings-tp
    expose:
      - 4004
    command: settings-tp -C tcp://validator:4004
    stop_signal: SIGKILL

# -------------=== shell ===-------------

  shell:
    image: hyperledger/sawtooth-shell:chime
    container_name: sawtooth-shell
    volumes:
      - /root/sawtooth-node/data:/var/lib/sawtooth/
    command: |
      bash -c "
        if [ ! -e /root/.sawtooth/keys/root.priv ]; then
           sawtooth keygen
        fi &&
        tail -f /dev/null
      "
    stop_signal: SIGKILL

# -------------=== validators ===-------------

  validator:
    image: hyperledger/sawtooth-validator:chime
    container_name: sawtooth-validator
    expose:
      - 4004
      - 5050
      - 8800
    ports:
      - "8800:8800"
      - "4004:4004"
    volumes:
      - /root/sawtooth-node/data:/var/lib/sawtooth/ #to backup ledger
      - /root/sawtooth-node/configs:/etc/sawtooth #to backup keys, policies, etc..
    # NOTE: for the first node, in order to produce genesis values, we add the 'tail -f /dev/null'
       # so we can enter the container to generate the genesis configs
       # the command "sawtooth-validator -vv" will read configurations of the file under /etc/sawtooth/validator.toml (in the container). If there is no file, defaults will be applied
    command: |
      bash -c "
        tail -f /dev/null
        sawtooth-validator -vv
      "
# -------------=== pbft engines ===-------------

  pbft:
    image: hyperledger/sawtooth-pbft-engine:chime
    container_name: sawtooth-pbft-engine
    command: pbft-engine -vv --connect tcp://validator:5050

```

  
!!! ATTENTION !!! - once copied replace paths, ips or other things to match your current environment (for example check that the path for the keys inside the validator container matches your host path)
---
### start containers in the genesis node

```bash
cd 
cd sawtooth-node
docker-compose up
```

### enter the validator container and inside the container create key and genesis config

```bash
docker exec -it sawtooth-validator bash
```

* create the keypair for the validator
```bash
 cd /etc/sawtooth/
 sawadm keygen
```

* create the config genesis batch

```bash
 cd /etc/sawtooth/
 sawset genesis -k /etc/sawtooth/keys/validator.priv -o config-genesis.batch
```

* copy the public key and keep it in a notepad because you will use it in next command

```bash
 cat /etc/sawtooth/keys/validator.pub
```
---

### now, setup the other 3 validators, and take their keys

* foreach validator, create the validator.toml and ajust the configs in it (ips, etc..) in the folder of configs that 
will be mounted. Here is an example of validator.toml:

```yml
#
# Sawtooth -- Validator Configuration
#

# This file should exist in the defined config directory and allows
# validators to be configured without the need for command line options.

# The following is a possible example.

# Bind is used to set the network and component endpoints. It should be a list
# of strings in the format "option:endpoint", where the options are currently
# network and component.
bind = [
  "network:tcp://eth0:8800",
  "component:tcp://eth0:4004",
  "consensus:tcp://eth0:5050"
]

# The type of peering approach the validator should take. Choices are 'static'
# which only attempts to peer with candidates provided with the peers option,
# and 'dynamic' which will do topology buildouts. If 'dynamic' is provided,
# any static peers will be processed first, prior to the topology buildout
# starting.
peering = "static"

# Advertised network endpoint URL.
endpoint = "tcp://10.146.96.33:8800"

# Uri(s) to connect to in order to initially connect to the validator network,
# in the format tcp://hostname:port. This is not needed in static peering mode
# and defaults to None. Replace host1 with the seed's hostname or IP address.
# seeds = ["tcp://host1:8800"]

# A list of peers to attempt to connect to in the format tcp://hostname:port.
# It defaults to None. Replace host1 with the peer's hostname or IP address.
peers = ["tcp://10.146.93.24:8800", "tcp://10.146.93.30:8800", "tcp://10.146.133.36:8800"]

# The type of scheduler to use. The choices are 'serial' or 'parallel'.
scheduler = 'parallel'

# A Curve ZMQ key pair are used to create a secured network based on side-band
# sharing of a single network key pair to all participating nodes.
# Note if the config file does not exist or these are not set, the network
# will default to being insecure.
#network_public_key = 'wFMwoOt>yFqI/ek.G[tfMMILHWw#vXB[Sv}>l>i)'
#network_private_key = 'r&oJ5aQDj4+V]p2:Lz70Eu0x#m%IwzBdP(}&hWM*'

# The minimum number of peers required before stopping peer search.
minimum_peer_connectivity = 3

# The maximum number of peers that will be accepted.
maximum_peer_connectivity = 10

# The following settings are for tuning the thread allocation in several of the
# thread pools used by the validator.  These settings should be modified by
# advanced users only.

# The maximum number of threads in the component worker pool
# component_thread_pool_workers = 10

# The maximum number of threads in the network worker pool
# network_thread_pool_workers = 10

# The maximum number of threads in the signature verification worker pool
# signature_thread_pool_workers = 3

# The host and port for Open TSDB database used for metrics
# opentsdb_url = ""

# The name of the database used for storing metrics
# opentsdb_db = ""

# opentsdb_username = ""

# opentsdb_password = ""

# The type of authorization that must be performed for the different type of
# roles on the network. The different supported authorization types are "trust"
# and "challenge". The default is "trust".

# [roles]
# network = "trust"

# Any off-chain transactor permission roles. The roles should match the roles
# stored in state for transactor permissioning. Due to the roles having . in the
# key, the key must be wrapped in quotes so toml can process it. The value
# should be the file name of a policy stored in the policy_dir.

# [permissions]
# transactor = "policy.example"
# "transactor.transaction_signer" = "policy.example"
```


then go ahead create the same folder structure as you did on genesis node and creates the docker-compose file but use this docker-compose file template:

```bash

version: '3.6'

services:

# -------------=== rest api ===-------------

  rest-api:
    image: hyperledger/sawtooth-rest-api:chime
    container_name: sawtooth-rest-api
    expose:
      - 8008
    ports:
      - "8008:8008"
    command: |
      bash -c "
        sawtooth-rest-api --connect tcp://validator:4004 --bind 0.0.0.0:8008
      "
    stop_signal: SIGKILL

# -------------=== settings tp ===-------------

  settings-tp:
    image: hyperledger/sawtooth-settings-tp:chime
    container_name: sawtooth-settings-tp
    expose:
      - 4004
    command: settings-tp -C tcp://validator:4004
    stop_signal: SIGKILL

# -------------=== shell ===-------------

  shell:
    image: hyperledger/sawtooth-shell:chime
    container_name: sawtooth-shell
    volumes:
      - /root/sawtooth-node/data:/var/lib/sawtooth/
    command: |
      bash -c "
        if [ ! -e /root/.sawtooth/keys/root.priv ]; then
           sawtooth keygen
        fi &&
        tail -f /dev/null
      "
    stop_signal: SIGKILL

# -------------=== validators ===-------------

  validator:
    image: hyperledger/sawtooth-validator:chime
    container_name: sawtooth-validator
    expose:
      - 4004
      - 5050
      - 8800
    ports:
      - "8800:8800"
      - "4004:4004"
    volumes:
      - /root/sawtooth-node/data:/var/lib/sawtooth/ #to backup ledger
      - /root/sawtooth-node/configs:/etc/sawtooth #to backup keys, policies, etc..
    # NOTE: YOU MUST SET UP THE VALIDATOR.TOML    
    #     the command "sawtooth-validator -vv" will read configurations of the file under /etc/sawtooth/validator.toml (in the container). If there is no file, defaults will be applied
    #    - key generation if there is no key
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
# -------------=== pbft engines ===-------------

  pbft:
    image: hyperledger/sawtooth-pbft-engine:chime
    container_name: sawtooth-pbft-engine
    command: pbft-engine -vv --connect tcp://validator:5050


```

* run the containers

```bash
cd 
cd sawtooth-node
docker-compose up -d
```

* take all the 4 public keys of the validators and create the genesis batch
and the first proposal
ATTENTION: be sure that all the validators containers and the pbft-engine containers are running
in all the nodes

turn back to the genesis validator and enter the validator container in the genesis node (the node where you have created
the genesis config)

go to 

```bash
cd /etc/sawtooth/
```

and run:

```bash
sawset proposal create \
            -k /etc/sawtooth/keys/validator.priv \
            sawtooth.consensus.algorithm.name=pbft \
            sawtooth.consensus.algorithm.version=1.0 \
            # the keys used are the copied public keys in the step before
            sawtooth.consensus.pbft.members="[\"027b83784297e1ccb8208b6dac3c03c0c7b61ec6352e84e7a9271e00974e4029e1\", \"03baf8365ff0ddd4f81895ce089352837f888660b7d30d4b6d1d04bba711631dcd\", \"0328c0d7fa7bd53b29422454150bf4076485745504b1cdcb767c77c1d3786f3c41\", \"03e0500c76b7ec60b81db94c0df76540c94350a23aeefe0feadf049f95626a4847\"]" \
            sawtooth.publisher.max_batches_per_block=1200 \
            -o config.batch
```

it will generate a 'config.batch' file (a protobuf encoded with transactions to be sent to the netwrok)

At this point inside /etc/sawtooth/ you must have these two files:

config-genesis.batch and config.batch

Now run

```bash
sawadm genesis config-genesis.batch config.batch
```

it will generate the file: /var/lib/sawtooth/genesis.batch


now start the validator (be sure that all nodes are online and with pbft containers running)

```bash
sawtooth-validator
```

test the network:

```bash
curl http://localhost:8008/blocks
```

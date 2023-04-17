#### To have subtransaction privacy and to have an abstraction layer over a DLT? 

DAML



#### To have only an immutable ledger?

 This can be done with a merkle tree of events, where each new event in the merkle tree contains the event payload + hash of the previous event.
 The whole can be digitally signed twice (timestamped digital signature): our own signature with our own encryption key which is then encrypted with 
 another key from a third party and so it is certain that our data was signed on that specific date (no one can say that it was not signed on 
 that day at that time unless we argue that there is a corruption of the third party).


#### To have an immutable ledger + a distributed system + consensus? 
	- Ethreum com POA
	- Cosmos sdk - private network (wasm or maybe with ethermint)
  - DAML + (corda or Hyperledger sawtooth or other)
  - Substrate (polkadot)
  - Algorand private netwrok


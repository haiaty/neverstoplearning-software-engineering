
# using DAML technology

<ins>__Description of the solution:__</ins>

A backend that writes data to a DAML ledger with a centralized setup (one participant node)
using Postgresql as datastore. A nodejs listener that reacts to events of the transactions on the ledger and writes
data to mariadb tables. A frontend that contacts a BF (backend for frotend), which calls a public API (in nodejs) to write data to the DAML ledger. list of tech needed: daml, postgresql, javascript, nodejs,mariadb


__Effort__: medium

<ins>__technical pros__</ins>: 

+Daml is a "customized" version of Haskell, which is a functional programming. In general functional programming force the programmer to think more and don't let them make some types of mistakes.

+Daml, in theory, can abstract the underline ledger. In practice, there are a high effort to migrate.

+Daml provides subtransaction privacy, which means that not all actions in a transaction could be seen by all of the    parties. This may be valuable when there are different parties, within different participant nodes, but not so much in a centralized setup, in which the central trusted party has access to all the data transactions. 

+Daml, in the centralized setup with postgresql, does not need to handle public key crytography in order to make transactions. It uses JWT tokens for authorization. 

technical cons:

- the critical cons is that bugfixing and upgrading of smart contracts have a extremely high effort, since 
for any change in a contract all the parties involved MUST agree on the migration. If there are just a few of contracts, then it is not so hard but If there are thousands or milions  of contracts it becomes extremely hard to manage both in time and in operations. There is no suche things such as proxy contracts or there is no simple way to fix a bug in the smart contract without causing a massive migration (in the case when there a lot of contracts).

- code should be carefully put in packages, and any change in any part of a code in the package, generate a new package id, that must be deployed and eventually a migration on the contracts should be done. This increases complexity of the project.

- in a centralized setup there is no immutability of the stored data. In fact, it is possible to change the data in the postgresql database (however it is hard and difficult to do since it is coded)

- limited comunity

- limited tooling




Business Pro: + marketing - DAML is a enterprise "blockchain" 

Business cons:

- limited vendor support - DAML is developed and supported by a single company: Digital Asset. Relying on a single vendor for critical technology can be a business risk.

- finding developers with experience in the language may be difficult

- limited ecosystem




# ETHEREUM PRIVATE NETWORK WITH POA

description: an Ethereum private network (starting with a single node) using the clique (POA consensus) to store the smart contracts and data. A nodejs listener that reacts to events of the transactions on the ledger and writes
data to mariadb tables. A frontend that contacts a BF (backend for frotend), which calls a public API (in nodejs) to write data to the Ethereum network ledger. There is no need of merkle tree and digital signatures because the data is in the immutable ledger. There is a need of encryption in the frontend for the digital signature of the transaction to be sent to the nodejs api, wich will then send the transaction to the ethereum node.

List of tech: Ethereum clients (geth or parity), solidity, javascript, nodejs, mariadb

effort: high

tecnical pro: 

+ upgradable smart contracts trough some tecniques like proxy contract. So it is easier to deploy fixes and changes changes. the code could be seen by users.

+ more tools and libraries

+ a large community of developers 


tecnical cons:

- learning solidity, for some, may be challeging


business pro:

+Ethereum is a solid choice for decentralized applications and smart contracts

+ Innovation: Using Ethereum for a private network can enable businesses to experiment with new use cases and applications that were previously not possible. This can lead to new revenue streams and business opportunities.

+ improved transparency

+ it is possible to bridge assets from private network to the public mainet


business cons:



# NON BLOCKCHAIN TECHNOLOGY (PROPOSED BY ME)

An immutable datastore, both as a mariadb with merkle tree proofs or as a specific datastore such as immudb for example, to store records. The merkle proof is signed digitally with timestamped digital signature (requires a third party for the signature of the timestamp). A nodejs listener that reacts to events of the transactions on the ledger and writes data to mariadb tables. A frontend that contacts a BF (backend for frotend), which calls a public API (in nodejs) to write data to the immutable data store. There is no need of public key cryptography in the client side.

tecnical pro:

+ mostly done with javascript, so a huge community, tools and libraries

+ tech that we are familiar with

+ easier to maintain

techincal cons:

- there is no "smart contract" but only immutable data

business pro:

+ in comparison to the others, the effort is less and changes are more affordable

business cons:

- there is no smart contract and DLT involved but only the immutable data, so no possibility for marketing as using DLT technology




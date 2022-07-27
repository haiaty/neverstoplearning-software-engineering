concepts, information and my experience and other about working with daml (around 6 months working with it)

## CONCEPTS

#### CONTRACTS

* are immutable. In order to change a contract, a new one must be created and the old one is archived
* Daml contracts are created with a complete list of parties that can see them (signatories + observers + choice controllers, usually called “stakeholders” as a whole).
(https://discuss.daml.com/t/daml-contracts-and-visibility/3161/2)


---

#### ACTIONS

- a create command
- a exercise command and its list of conseguences ( so 1 for exercise and 1 for each conseguence)
- a fetch action. A Fetch behaves like a non-consuming exercise with no consequences, and can be repeated.
- a key assertion. , which records the assertion that the given contract key is not assigned to any unconsumed contract on the ledger.

---
#### COMMIT

this is basically information that contains the party that generated the transactions. in this case they are called 'requesters of the commit'.

in a commit there may be several 'requesters of the commit

---
#### STAKEHOLDERS

stakeholders of a contract are: signatories and observers

non stakeholders of a contract: choice observes. (they only affect the set of informees on an action, for the purposes of projection)

---
#### TRANSACTION PROJECTIONS

it  define the view that each party gets on a transaction.
 Intuitively, given a transaction within a commit, a party will see only the subtransaction consisting of all actions on contracts where the party is a stakeholder.

Ledger projections do not always satisfy the definition of consistency, even if the ledger does. For example, in P’s view, Iou Bank A is exercised without ever being created, and thus without being made active. Furthermore, projections can in general be non-conformant. However, the projection for a party p is always

- internally consistent for all contracts,
- consistent for all contracts on which p is a stakeholder, and
- consistent for the keys that p is a maintainer of.
Lastly, as projections carry no information about the requesters, we cannot talk about authorization on the level of projections.

---
#### PRIVACY
Daml Ledgers show to contract observers only the state changing actions on the contract. 
More precisely, Fetch and non-consuming Exercise actions are not shown to contract observers - except when they are also actors or choice observers of these actions.

#### INFORMEE

a party p is an informee of an action A if one of the following holds:

- A is a Create on a contract c and p is a stakeholder of c. (creazione di un contratto dove il party è o un signatory o un observer (non choice observer)
- A is a consuming Exercise on a contract c, and p is a stakeholder of c or an actor on A. Note that a Daml “flexible controller” can be an exercise actor without being a contract stakeholder.
- A is a non-consuming Exercise on a contract c, and p is a signatory of c or an actor on A.
- A is an Exercise action and p is a choice observer on A.
- A is a Fetch on a contract c, and p is a signatory of c or an actor on A.
- A is a NoSuchKey k assertion and p is a maintainer of k.

---
#### DIVULGENCE

what is it? it is when a party is not a stakeholder of a contract (i.e. it is neither a signer nor an observer) and still manages to see contracts through "ledger projections", i.e. through
the fact that the party sees all the actions (and its tree of subrsansactions) of a contract of which it is a stakeholder. in other words, since any action on a contract C is seen by all the "informees"
In other words, since any action on a C contract is seen by all the "informees" of all the parent actions (these parties are called witnesses), then it may happen that a party sees a contract because it is an informee party in a parent action.

only Exercise actions can be ancestors of other actions.

---
#### WITNESSES

are the informees of all ancestor actions of the action being requested

if the exercise of the choice was "consuming": then the "witnesses" are the set of stakeholders (signatories,observers) + actors

if the exercise was not consuming: then the "witnesses" are the set of signatories + actors (choice observers)

---
#### ACTORS

are the parties performing a given choice + choice observers

---

#### MAINTANERS

are the parties that make sure that at most one unconsumed contract exists for the key. 

The maintainers must be a subset of the signatories and depend only on the key. This dependence is captured by the function maintainers that takes a key and returns the key’s maintainers.


## DEBUG
When debugging DAML Scripts, whether inside of a trigger or repl, is there a way to capture debug output back to the script? 
At the moment no, debug output is just written to stdout. What you could do is to have your own wrapper that tracks debug mesasges in something like a State action in addition to printing it out. Then you can easily capture debug messages.

--

## OTHER
---
#### SIGN DATA (CONTRACT, CHOICE DATA) with PKI

Centralized signing (https://stackoverflow.com/questions/17607604/digitally-signing-data-in-a-web-app ) 
A better option (IMHO) is to sign centrally. This way the keys are kept in a centralized FIPS-secure server. Meanwhile, the signers just use a webapp to authorize the signing. The signers don't need to hold the private key since it is stored in the secure server. In the centralized design, the private key does not leave the central server. Rather, the document or data to be signed is sent to the server, is signed, and then the signed doc or data (e.g. XML) is returned to the webapp.




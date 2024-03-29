concepts, information and my experience and other about working with daml (around 12 months working with it)

## UTILS


#### exercise by key in script:

```
submit <party> do exerciseByKeyCmd @TemplateName (key1, key2) <ChoiceName> with param = "text"
```


#### do a query for all templates that a pary can see and take the first one

```
    a <- query @<templateName> <party>         -- this query return an list of contracts 

    let (_, contract) = head a          -- we take the first one since we know it is the one we have just created
    
    
    
    
    -- remenber to import the function of DA.List --    import DA.List
```

#### check if a result of a query is empty in a script

```
list <- query @<Template> <party>
  
if (null list) then do abort "lisst could not be empty" else do return ()

 ```
 
 #### iterate over a list and perform fetch, and after perform some checks, inside a choice
 
 ```
 
do

let fetchContracts contractKey = do
              (_, contract) <- fetchByKey @<Template> (contractKey._1, contractKey._2)      --to try also fetchByKey @<Template> contractKey
              assertMsg "error if boolValue is false" contract.someBoolValue
              return contract

mapA_ fetchContracts [(key._1, key._2), (key._1, key._2)]

 ```
 

#### meaning of $ operator:

The $ symbol in DAML is an infix operator that's used for function application.

The $ operator has lower precedence than function application using parentheses, which means that expressions to the right of the operator are evaluated first. 

This makes it useful for avoiding parentheses in nested function calls.

For example, the expression foo (bar (baz x)) can be written as foo $ bar $ baz x using the $ operator. This is equivalent to writing foo(bar(baz(x))), but it's less verbose and easier to read in some cases.

In this example:

```
(_, delRef) <- fetchByKey @Account.R $ getAccount collateral
```

In the code you provided, the $ operator is used to apply the getAccount function to the collateral argument, and then pass the result of that function call to the fetchByKey function. This is equivalent to writing fetchByKey @Account.R (getAccount collateral), but it's written using the $ operator for conciseness.

#### if with different branches

```
        let newStatus = if
              | remaining == auction.quantity.amount -> NoValidBids
              | remaining > 0.0 -> PartiallyAllocated with finalPrice; remaining
              | otherwise -> FullyAllocated with finalPrice

```

#### get value from some or give error

```
fromSomeNote "error mesage" <somevar>
```

#### execute in 'loop'

```
forA_ batchCids (`exercise` Batch.Settle with actors = singleton provider)
```

#### create a list of tuples usig zip and fetching contracts

```
bids <- zip bidCids <$> forA bidCids fetch
```

Here, forA is a DAML function that maps a list of values to a list of actions and sequences the actions. The fetch function is passed to forA to fetch the contracts associated with each bidCids identifier. The <$> operator is the infix version of fmap, which applies the zip bidCids function to the resulting list of fetched contracts. The resulting list of tuples is then bound to the bids variable.

### examples of named imports

```
import DA.Map qualified as M (empty)
import DA.Set qualified as S (fromList, singleton)
import Daml.Finance.Interface.Account.Account qualified as Account (Credit(..), Debit(..), GetCid(..), I, R, Controllers(..), View(..), createReference, disclosureUpdateReference)
import Daml.Finance.Interface.Account.Factory qualified as AccountFactory (Create(..), F, Remove(..), View(..))
import Daml.Finance.Util.Disclosure (addObserversImpl, removeObserversImpl, setObserversImpl)
```


## TIPS, SUGGESTIONS, BEST PRACTICES


- do not store large data on daml (https://discuss.daml.com/t/on-attachments-what-you-should-and-shouldnt-store-on-a-ledger/579/15 )
- when upgrade: prefix module with v2 (https://discuss.daml.com/t/whats-the-best-way-to-organize-my-project-so-that-im-easily-upgradable/1762
- Good design patterns (https://docs.daml.com/daml/patterns.html )


## CONCEPTS AND RELATED

#### PARTY
A party represents a person or legal entity. It is a unicode string. The party text can only contain alphanumeric characters, -, _ and spaces. 
Each party has a unique identity.

##### HOW TO ADD A NEW PARTY?

How to add a new party?  https://docs.daml.com/concepts/identity-and-package-management.html#provisioning-identifiers  
1. The first step in adding a new party to the ledger is to provision a new identifier for the party by using the The Ledger API  AllocateParty method (The AllocateParty call can take the desired identifier and display name as optional parameters, but these are merely hints and the ledger implementation may completely ignore them.)
2. link the identifier to a party in the real world (praticamente dobbiamo trovare un modo per farsì che dato un  identifier di un party si riesca a risalire alla identità nel mondo reale in maniera inequivocabile). The solutions for linking the identifiers to real-world identities could rely on certificate chains, verifiable credentials, or other mechanisms. The mechanisms can be implemented off-ledger, using Daml workflows (for instance, a “know your customer” workflow), or a combination of these.

---

#### TEMPLATES 

What is a template? data, bevaiour and rights/obbligations (kinf of authorization) for a daml contract.

---
#### CHOICES

choices are permissioned functions that result in an update. 

choices: business logic, which under the right permission, can mutate the ledger causing an update. For example, using choices, authorities can be swapped.

all choices must specify who can execute them. it can be executed by one or more participants. if there are more than one then it means that everyone must execute/accept it


by default, all choices consume the contract on which they are called unless they have the keyword "non-consuming"

a template can have 0 or more choices.

---
#### SQL ANALOGY

- each template is like a table and the data fields are the columns;
- each contract is like a row;
- all templates are like a database/schema;
- choices are like sproc. they are atomic;
- keys like the unique key in a table.

---
#### SCRIPTS

Scripts on the other hand do not fail atomically: while each submit is atomic, if a submit succeeded and the script fails later, the effects of that submit will still be applied to the ledger.

---
#### TRANSACTIONS

A transaction is a list of actions. 
Transactions succeed or fail atomically as a whole.

#### DAR FILE

A dar file is Daml’s equivalent of a JAR file in Java: it’s the artifact that gets deployed to a ledger to load the package and its dependencies. dar files are fully self-contained in that they contain all dependencies of the main package.

#### DAML ACTIVE CONTRACT SET (ACS)

basically it is the current state of the application (all events have been applied up to that point since daml uses an event sourcing model

#### PARTICIPANT NODE 

 “participant node” is anything that exposes the gRPC Ledger API. From a client perspective, it’s basically an IP address, a port, and a promise that we can make a gRPC connection to it and expect it to understand the messages defined by the Ledger API spec.
There are many different implementations that could be the server behind that IP & port, and “participant node” is the generic term for them.
The JSON API is a normal (gRPC) Ledger API client. It is not special in any way; anyone could have written it, it can run on a separate server, etc. It happens to be a component we provide because it seemed to fit a relatively common use-case, but ultimately it’s not part of the core ledger model and it’s not more special than any application you would write against the (gRPC) Ledger API.
You could imagine a setup where the JSON API runs on the same server (or within the same internal network) as the (gRPC) Ledger API, and where the JSON API is the only one exposed “outside” that server/internal network. That’s a valid choice, though one in which you should remember that the JSON API does not expose the full power of the Ledger API, so clients that are forced to only go through it will be somewhat restricted in what they can do.

#### CONTRACTS

* are immutable. In order to change a contract, a new one must be created and the old one is archived
* Daml contracts are created with a complete list of parties that can see them (signatories + observers + choice controllers, usually called “stakeholders” as a whole).
(https://discuss.daml.com/t/daml-contracts-and-visibility/3161/2)

---

#### SIGNATORY

what is a signatory? a participant that must have consented in the creation of the contract. 
he is practically one of those who gave permission for the contract to be created/archived. so he answers the question 'who can create the contract'?

##### How can I implement the case that the contract requires dynamic numbers of signatories?

https://discuss.daml.com/t/how-can-i-implement-the-case-that-the-contract-requires-dynamic-numbers-of-signatories/1636

---

#### OBSERVER

 it is optional. they are the ones who can see the contract. they basically have the right to see the contract. nothing else, just see.

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

## RESET LEDGER

- if you reset the ledger using ' ResetServiceGrpc.ResetServiceBlockingStub blockingStub = ResetServiceGrpc.newBlockingStub(channel);' 
you will reset parties, acive contracts but will not remove any templates uploaded to it using upload-dar. Templates will still remain

--

## OTHER
---
#### SIGN DATA (CONTRACT, CHOICE DATA) with PKI

Centralized signing (https://stackoverflow.com/questions/17607604/digitally-signing-data-in-a-web-app ) 
A better option (IMHO) is to sign centrally. This way the keys are kept in a centralized FIPS-secure server. Meanwhile, the signers just use a webapp to authorize the signing. The signers don't need to hold the private key since it is stored in the secure server. In the centralized design, the private key does not leave the central server. Rather, the document or data to be signed is sent to the server, is signed, and then the signed doc or data (e.g. XML) is returned to the webapp.


## FAQ

#### who can see the postgresql data?

in general, the owner and operator of the PostgreSQL instance where you're hosting the ledger will indeed be able to see all of its content. This means that there must be a trust relationship between the operator of the database and the party which is hosting their data there. If a party does not trust any specific operator, they can run their own node. If that's the case, only the data which that party is authorized to see will be on the participant.
Note that, even if multiple parties are hosting their data on the same, trusted node, they will not be able to access each other's data, as the Ledger API only shows to each party the data that it's authorized to access. https://stackoverflow.com/questions/70580340/how-is-daml-able-to-maintain-privacy-between-the-parties-if-they-share-the-same

### Can I know wich contracts are available on a Daml application?

Yes, using navigator (https://docs.daml.com/1.18.3/tools/navigator/index.html). Otherwise, It's not that easy to develop a script or program that do that.
Read here to know why: https://discuss.daml.com/t/query-daml-templates-using-the-json-api/4481/7

to use navigator, download the sdk and execute:

daml navigator server

## TROUBLESHOOTING

#### user error (Transitive dependencies with same unit id but conflicting package ids

 solution 1: build the .dars in the same platform, for example build all dars on windows, or all .dars on linux, etc.. (see:  https://discuss.daml.com/t/transitive-dependencies-with-same-unit-id-but-conflicting-package-ids/5267/6 )


Solution 2: have two different repos with the version in the .daml or have a branch with different daml.yaml

You have to change the version and/or the name. It is not possible to depend on two different packages with the same name and version. This is the tradeoff we chose for the compiler to avoid users having to specify package ids everywhere. See: https://discuss.daml.com/t/working-with-packages-of-different-versions/2328

### Exception in thread "main" java.lang.IllegalArgumentException: unknown module 885d5e47aa1115d34eb8d1246c3dac5aefa43a47508f236da5eb3be043a8eaf7:Scripts.InitializeLedger while looking for value 885d5e47aa1115d34eb8d1246c3dac5aefa43a47508f236da5eb3be043a8eaf7:Scripts.InitializeLedger:initialize

Basically the problem was that the name of the module was wrong.

I had the folder  daml/Scripts/InitializeScript.daml and a script called "initialize"

And when running the "daml script", I was running with the wrong module path. Iwas running

This: 

daml script --dar .\main-1.0.0.dar --ledger-host localhost --ledger-port 6865 --script-name Scripts.InitializeLedger:initialize:initialize

Instead of 

daml script --dar .\main-1.0.0.dar --ledger-host localhost --ledger-port 6865 --script-name InitializeLedger:initialize


###  I was using navigator and trying to create new contracts from the template section, but when i clicked on the submit buttin nothing happened and I was not able to create the contract.

Solution: basically I was inserting the wrong party because I was inserting into the input the party  display_name instead of the party Id. 
I was able to spot it out because I entered in the postgresq database and saw that the party id was different from the one I was inserting



## UPGRADING APPLICATIONS (for example for bugfixing or new models)0

If you make a change to the templates, before building the new dar,
you should also change the property "version" in the "daml.yaml"
in order to be able to import the new ".dar" in the "main" project.
This is because You have to change the version and/or the name. It is not possible to depend on two different packages with the same name and version.
(see https://discuss.daml.com/t/working-with-packages-of-different-versions/2328)



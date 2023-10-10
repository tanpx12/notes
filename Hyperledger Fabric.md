
# Key concept
## Hyperledger Fabric Model

**Assets**
- Assets are representative of anything with monetary value over the network.
- Assets are represented in Hyperledger Fabric as a collection of key-value pairs, with state changes recorded as transactions on a Channel ledger.
- Hyperledger Fabric provides the ability to modify assets using chaincode transaction.

**Chaincode**
- Chaincode is software defining an assets or assets and the instructions for modifying the assets. 
- Chaincode function 

**The Ledger**
- In Hyperledger Fabric, a ledger consists of of 2 distinct, though related parts - a world state and a blockchain
- World state - a database that holds current values of a set of ledger states. Ledger states are, by default, expressed as key-value pairs.
- Blockchain - a transaction log that records all the changes that have resulted in the current world state. The blockchain data once written, can't be modified => immutable.

## How Fabric networks are structured

**The sample network**

![[Pasted image 20230706102230.png]]

- 3 orgs: R0,R1,R2 establish a network with configuration CC1 which contains all the definitions of the orgs as well as the roles of each orgs in the channel.
- P1, P2, the corresponding peers of R1 and R2, joing the channel C1. Meanwhile, O - the ordering service- is owned by R0. All of these nodes will contain will contain a copy of the ledger (L1) of the channel.
- R1 and R2 interact with the channel through the application A1 and A2, which they own.
- All 3 orgs have a CA that has generated the necessary certificates for the nodes, admin, orgs definition, and applications of its orgs.

**Creating the network**

- The first step is to agree and define the network configuration.
- The channel configuration, CC1, is contained in a "configuration block" which typically, created by the `configtxgen` tool from a `configtx.yaml` file. This configuration block contains record of the orgs that can join components and interact on the channel, as well as the policies that define the structure for how decisions are made and specific outcome are reached.
- The definitions of these orgs, and the identities of their admins, must be created by a CA associated with each org. 

**Certificate Authorities (CA)**

- Different components of the blockchain network use certificates to identify themselves to each other as being from a particular org => different orgs use different CAs.
- The mapping of certificates to member orgs is achived via a structure called Membership Services Provider (MSP), which defines an org by creating an MSP which is tied to a root CA certificate to identify that components and identities were created by the root CA. The channel configuration can then assign certain rights and permissions to the org through a policy. 

**Join nodes to the channel**

- Peers are fundamental element of the network because the host ledgers and chaincode => they are one of the physical points at which org that transact on a channel connect to the channel.
- The ordering service gathers endosed transactions from applications and order them into transaction blocks, which are subsequently distributed to every peer node in the channel.
- After the ordering service has been added to the channel, it is possible to propose and commit updates to the channel configuration, but little else.

**Install, approve, and commit a chaincode**

- In Fabric, the business logic that defines how peer orgs interact with ledger is contained in chaincode.
- Chaincodes are installed on peers, and then defined and committed on a channel.
- The chaincode is installed on the relevent peers, approved by the revelent peer orgs, and committed on the channel. In the example, the chaincode, S5, is installed on every peer, even though organizations are not required to install every chaincode. 
- The most important piece of information supplied within the chaincode definition is the endorsement policy. It describes which organizations must endorse transactions before they will be accepted by other orgs onto their copy of ledger.

**Using an application on the channel**

- After a smart contract has been committed, client applications can be used to invoke transactions on a chaincode, via the Fabric Gateway service. 
- Just like peers and orderers, a client application has an identity that associates it with an org.
- Starting in Fabric v2.4, the client application makes a gRPC connection to the gateway service, which then handles the transaction proposal and endorsement process on behalf of the application

## Membership Service Provider (MSP)

**Definition**
- The implementation of the MSP requirement is a set of folders that are added to the configuration of the network and is used to define an org both inwardly and outwardly.
- The MSP identifies which Root CAs and Intermediate CAs are accepted to define the members of a trust domain by listing the identities of their members, or by identifying which CAs are authorized to issue valid identities for their members.

**MSP domains**
- MSPs occur in 2 domains in a blockchain network:
	- Locally on an actor's node (local MSP)
	- In channel configuration (channel MSP)
- Local MSPs 
	- Defined for clients and for nodes (peers and orderers)
	- Define the permission for a node (who are the peer admins, who can operate the node, etc)
	- The local MSPs of clients allow the user to authenticate itself in its transaction as a member of channel, or as the owner of a specific role into the system.
	- Every node must have a local MSP defined.
	- An orderer local MSP is also defined on the file system of the node and only applies to that node.
- Channel MSPs
	- Channel MSPs define administrative and participatory rights at the channel level.
	- Channel MSPs identify who has authorities at a channel level.
	- Every org participation in a channel must have an MSP defined for it.
	- The channel MSP includes the MSPs of all the orgs on a channel.
	- Local MSPs are only defined on the file system of the node or user to which they apply. However, a channel MSP is also instantiated on the file system of every node in the channel and kept synchronized via consensus

**MSP structure**

![[Pasted image 20230711094402.png]]

- `config.yaml`: used to configure the identity classification feature in Fabric by enable "Node OUs" and defining the accepted roles.
- `cacerts`: this folder contains a list of self-signed X.509 certificates of the Root CAs trusted by the org represented by this MSP.
- `intermediatecerts`: this folder contain a list of X.509 certificates of the intermediate CAs trusted by the org.
- `admincerts` (deprecated from Fabric v1.4.3)
- `keystore`: This folder is defined for the local MSP of a peer or orderer node, and contains the node's private key (used to sign data). This folder is mandatory for local MSPs and contain exactly 1 private key. The *channel MSP* does not include this folder.
- `signcert`: For a peer or orderer node this folder contains the node's certificate issued by the CA.
- `tlsintermediatecacerts`: this folder contains a list of intermediate CA certificates CAs trusted by the org represented by this MSP for secure communication between nodes using TLS.

The channel MSP includes the following additional folder:
- `Revoked Certificates`: if the identity of an actor has been revoked, identifying information about the identity - not the identity itself - is held in this folder.


## Policies

**What is a policy**
- Policy is a set of rules that define the structure for how decisions are made and specific outcomes are reached. 
- Fabric policies are the mechanism for infrastructure management. It represent how members come to agreement on accepting or rejecting changes to the network, a channel, or a smart contract.
- Polices are agreed to by the channel members when the channel is originally configured, but they can be modified as the channel evolves.

**Why are policies needed**
- Policies are one of the things that make Hyperledger Fabric different from other blockchains like Eth or Bit. 
	- In those system,  the policies that govern the network are fixed at any point in time and can only be changed using the same process that governs the code. Meanwhile, Fabric user have the ability to decide on the governace of the network before it is launched, and change the governace of a running network.
- Policies allow members to decide which orgs can access or update a Fabric network, and provide the mechanism to enforce those decisions.
	- Policies contain a list of orgs that have acces to a given resource, such as a user or system chaincode. They also specify how many orgs need to agree on a proposal to update resource.

**How policies are implemented**
- Policies are defined within the relevant administrative domain of a particular action defined by the policy.
- For example, the policy for adding a peer organization to a channel is defined within the administrative domain of the peer organizations (known as the `Application` group).

**Access Control Lists (ACLs)**
- ACLs provide the ability to configure access to resources by associating those resources with existing policies.

**Smart Contract endorsement policies**
- Every smart contract inside a chaincode package has an endoserment policy that specifies how many peers belonging to different channel members need to execute and validate a transaction against a given smart contract in order for the transaction to be consider valid.

**Modification policies**
- Modification policies specify the group of identities required to sign any configuration update.


## Peers

- Peers manage ledgers and smart contracts. It also manages transaction proposals and endoserments by running Fabric Gateway service. 
**Ledger and chaincode**
- The peer hosts instances of the ledger, instances of chaincode, because blockchain require consistent replicas of data and smart contracts across peers in a channel. 
- Because a peer is a host for ledgers, chaincodes and services, client applications and administrators must interact with a peer to access these resources.

**Multiple Ledgers**
- A peer is capable of hosting more than one ledger, which is useful because it allows for a flexible system design where a signle peer can belong to mutiple channels in a Fabric network.

**Multiple Chaincodes**
- A chaincode is instantiated on a single channel. Each channel can have mutiple chaincoeds that interact with it

**Fabric Gateway service**

- The gateway service manage transaction proposals and endorsements on the peer. 

**How applications and peers interact**

- Phase 1: Transaction proposal and endorsement
	- Transaction proposal - The client application (A1) submits a signed transaction proposal by connecting to the gateway service on P1. A1 must either delegate the selection of endorsing orgs to the gateway service or explicitly identify the organizations required for endorsement.
	- Transaction execution - The gateway service selects P1, or another peer in its org, to execute the transaction.The selected peer executes the chaincode (S1) specified in the proposal, generates a proposal response. The selected peer signs the proposal response and returns it to the gateway.
	- Transaction endorsement - The gateway repeats transaction execution for each orgs required by the chaincode endorsement policies. The gateway service collects the signed proposal responses and create a transaction envelop - which it returns to the client for signing.
- Phase 2: Transaction Submission and Ordering
	- Transaction submission - the client sends the signed transaction envelop to the gateway service. The gateway forward the envelop to an ordering node and returns a success message to the client.
	- Transaction ordering - The ordering node (O1) verifies the signature, and the ordering service orders the transaction, and packages it with other ordered transactions into blocks. The ordering service then distributes the block to all peers in the channel for validation and commiment to the ledger.
- Phase 3: Transaction Validation and Commitment
	- Transaction Validation - Each peer checks that the client signature on the transaction envelop matches the signature on the original transaction proposal. Each peer also check that all read-write sets and status responses are equivalent and that the endorsements satisfy the endorsement polices. Each peer then marks each transaction as valid or invalid for commitment to the ledger.
	- Transaction commitment - Each peer commit the ordered block of transactions to the channel ledger (L1). The commit is an immutable ledger update to the channel ledger. The world state of the channel is updated with result of valid transaction only. 
	- Commit event - Each peer that commits to the ledger sends the client a commit status event with proof of the ledger update.

**How peers and channels interact**

- Channel include peer nodes, orderer nodes and applications, and by joining a channel, they are agree to collaborate to collectively manage and share identical copies of the ledger. 

## Ledger 

- A ledger stores important factual information about bussiness objects; both the current value of the attributes of the objects, and the history of transactions that resulted in these current values. 

- In Hyperledger Fabric, there're 2 distinct, parts - a world state and a blockchain. 
- World state is a database that holds current values of a set of ledger states. The ledger states are, by default, expressed as key-value pairs.
- Blockchain is a transaction log that records all the changes that have resulted in the current world state. It can't be modified and immutable.
 
![[Pasted image 20230711142247.png]]

**World state**
- The world state holds the current value of the attributes of a business object as a unique ledger state.
- The world state is implemented as a database.
- Applications submit transactions which capture changes to the world state, and these transactions end up being committed to the ledger blockchain. 
- Only transactions that are signed by the required set of endorsing organizations will result in an update to the world state.
- A state has a version number which is for internal use by the Hyperledger Fabric, and is incremented every time the state change.

**Blockchain**

- The blockchain has recorded every previous version of each ledger state and how it has been changed.
- The blockchain is structured as sequential log of interlinked blocks, where each block contains a sequence of transactions, each transaction representing a query or update to the world state.
- Ordering service is in charge of sequencing the whole blockchain and the transactions within a block.
- Each block header includes a hash of the block's transactions, as well a hash of the prior block's header.

**Blocks**
- Structure of a block:
	- Block Header: This section comprises three field, written when a block is created.
		- Block number: an integer starting a 0, and increase by 1 for every new block appended to the blockchain.
		- Current Block Hash: the hash of all the transactions contained in the current block.
		- Previous Block Header Hash: the hash from the previous block header.
	- Block Data: this section contains a list of transactions arraged in order. It is written when the block is created by the ordering service.
	- Block Metadata: 

**Transactions**
- Structure of a transaction:
	- Header: contains some essential metadata about the transaction (name, relevant chaincode, version, ...)
	- Signature: contains cryptographic signature created by the client application.
	- Proposal: captures the before and after values of the world state.
	- Endorsement: is a list of signed transaction responses from each required organization sufficient to satisfy the endorsement policy.

## Smart contracts and Chaincode

- A smart contract defines the transaction logic that controls the lifecycle of a business object contained in the world state.
- A smart contract programmatically access two distinct pieces of the ledger - a blockchain and a world state.
- Smart contracts primarily *put*, *get* and *delete* states in the world state and can also query the immutable blockchain record of transactions.
	-  A get typically represents a query to retrieve information about the current state of a business object
	-  A put typically creates a new business object or modifies an existing one in the ledger world state.
	-  A delete typically represents the removal of a business object from the current state of the ledger, but not its history.
- Associated with every chaincode is an endorsement policy that applies to all of the smart contracts defined within it.
**Valid transactions**
- The contract takes a set of input parameters called the transaction proposal and uses them in combination with its program logic to read and write the ledger.
- Changes to the world state are captured as a transaction proposal response
- The world state is not updated when the smart contract is executed.

**Channels**
- While the smart contract code is installed inside a chaincode package on an org peers, channel member can only execute a smart contract after the chaincode has been defined on a channel. 
- The chaincode definition is a struct that contains the parameters that govern how a chaincode operates. This including the chaincode name, version and endorsement policy. When a sufficient number of orgs have apporved to the same chaincode definition, the definition can be committed to the channel. 

**Intercommunication**
- A smart contract can call other smart contracts bot within the same channel and across different channels. In this way, they can read and write world state data to which they would not otherwise have access due to smart contract namespaces.

**System chaincode**
- Chaincode can also define low-level program coed which corresponds to domain independent system interactions, unrelated to these smart contracts for business processes.
- Different types of system chaincodes and their associated abbreviations:
	- `_lifecycle` runs in all peers and manages the installation of chaincode on your peer, the approval of chaincode definitions for your org and the committing of chaincode definitions to channels.
	- Configuration system chaincode (CSCC) runs in all peers to handle changes to a channel configuration, such as policy update.
	- Query system chaincode (QSCC) runs in all peers to provide ledger APIs which include block query, transaction query, ...
	- Endorsement system chaincode (ESCC) runs in endorsing peers to cryptographically sign a transaction response. 
	- Validation system chaincode (VSCC) validate a transaction, including checking endorsement policy and read-write set versioning.

## Fabric chaincode lifecycle

**Deploying a chaincode**

- The Fabric chaincode lifecycle is a process that allows multiple organizations to agree on how a chaincode will be operated before it can be used on a channel. 

**Install and define a chaincode**

- Fabric chaincode lifecycle require that orgs agree to the parameters that define a chaincode, such as name, version, and the chaincode endorsement policy. Channel members come to agreement using the following 4 steps: 
	- Package the chaincode: This step can be completed by one org or by each org.
	- Install the chaincode on your peers: Every org that will use the chaincode to endorse a transaction or query the ledger needs to complete this step
	- Approve a chaincode definition for your org: Every org that will use the chaincode needs to complete this step.
	- Commit the chaincode definition to the channel: The commit transaction needs to be submitted by one org once the required number of org on the channel have approved. 

**Upgrade a chaincode**
- To upgrade chain, follow these steps: 
	- Repackage the chaincode: You only need to complete this step if you are upgrading the chaincode binaries.
	- Install the new chaincode package on your peers: Once again, you only need to complete to complete this step if you are upgrading the chaincode binaries. Installing the new chaincode package will generate a package ID, which you need to pass to the new chain code definition.
	- Approve a new chaincode definition: If you are upgrading the chaincode binaries, you need to update the chaincoed version and the package ID in the chaincode definition. 
	- Commit the definition to the channel: When a sufficient number of channel members have approved the new chaincode definition, one org can commit the new chaincode definition to upgrade the chaincode definition to the channel.
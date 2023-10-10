- [SSX-like system](https://github.com/spruceid/ssx)
- Login with identity (Ziden)
- [Social recovery integration](https://vitalik.ca/general/2021/01/11/recovery.html)
# Login with zeroknowledge identities

- Ziden maintain a claim system for users.
- JWZ can take advantage of this system and use it to authenticate and authorize users to other platform.
**Pros of JWZ**: 
- Auth without having an account
- No information required for auth
- Anonymous login
- Social recovery
- [Better security ?](https://crypto.stackexchange.com/questions/25338/why-arent-zero-knowledge-proofs-used-in-practice-for-authentication)
**Cons of JWZ**:
- Multi-devices login  
- Trusted setup for verification key
- Migration ? 
- Having trusted authority

# Social Recovery for Identity wallet

Including 2 main components:
- Key recovery, based on [Shamir secret sharing](https://github.com/WebOfTrustInfo/rwot8-barcelona/blob/master/topics-and-advance-readings/security_shamirs.md).
- Key rotation, based on claim tree architecture and smart contracts.
## Key recovery

An ideally implementation should balance between many competing conditions:
- Key recovery should require approval of many individuals to minimize the potential of theft or deliberate key compromise by a small malicious subset of users.
- Key recovery should require the smallest acceptable threshold to prevent loss of funds from destroyed/lost/inaccessible shares.
- Key recovery requiring interaction with many individuals is undesirable, as each interaction must involve re-authentication at the inconvenience of all involved.
- Each group of shares should have different "weight", depend on how close they are with the owner of the secret => n-way linear key split.

[Implementation of SSS](https://github.com/grempe/secrets.js)

**Problem** : adversaries try to submit fake shares to prevent owner restores the secret

==> Verifiable secret sharing (vss) 
==> Which scheme should i use ? 
- [ ] [Schoenmaker](https://github.com/blockades/dark-crystal-secrets) 
- [ ] [Feldman](https://crypto.stackexchange.com/questions/6637/understanding-feldmans-vss-with-a-simple-example)
- [ ] Publicity verifiable secret sharing

==> Label the encryption key when recover the secret 
# Social Recovery for Crypto wallet 

## Multisig wallet/contract 

## Transfer threshold with zero knowledge

## Account abstraction 
https://eips.ethereum.org/EIPS/eip-4337
- The key goal of account abstraction is allow users to use smart contract wallets containing arbitrary verification logic instead of EOAs as their primary. Completely remove any need at all for users to also have EOAs
- Account abstraction allow you to:
	- Define your own flexible security rules
	- Recover your account if you lose the keys
	- Share your account security across trusted devices or individuals
	- Pay someone else's gas, or have someone else pay yours
	- Batch transactions together
	- More opportunities for dapps and wallet developers to innovate
### Specification
**Definitions**:
- **UserOperation** - a structure that describe a transaction to be sent on behalf of user.
	- Like a transaction, it contains "sender", "to", "calldata", "maxFeePerGas", "maxPriorityFee", "signature", "nonce", "signature"
	- "nonce" and "signature" usages is not defined by the protocol, but rather by each account implementation
- **Sender** - the account contract sending a user operation
- **EntryPoint** - a singleton contract to execute bundles of UserOperations. Bundlers/Clients whitelist the supported entrypoint
- **Bundler** - a node that bundles multiple UserOperations and create an `EntryPoint.handleOps()` transaction. Note that not all block-builders on the network are required to be bundlers.
- **Aggregator** - a helper contract trusted by accounts to validate an aggregated signature. Bundler/Clients whitelist the supported aggregators.
- Users send `UserOperation` objects to a dedicated user operation mempool. A specialize class of actors called **bundler** listen in on the user operation mempool and create **bundle transactions**. A bundle transaction packages up multiple `UserOperation` objects into a single `handleOps` call to be pre-published global **entry point contract**.
- The account:
	- MUST validate the caller is a trusted EntryPoint
	- If the account does not support signature aggregation, it MUST validate the signature is a valid signature of the `userOpHash`, and SHOULD return SIG_VALIDATION_FAILD (not revert) on signature mismatch. Any other error should revert.
	- MUST pay the entryPoint (caller) at least the "missingAccountFunds", 
	- The account MAY pay more than this minimum, to cover future transactions
	- The return value MUST be packed of `authorizer`,`validUntil` and `validAfter` timestamps
		- `authorizer` - 0 for valid signature, 1 to mark signature fail. Otherwise, an address of an authorizer contract. This ERC defines "signature aggregator" as authorizer
		- `validUntil` is 6-byte timestamp value, or zero for "infinite". The UserOp is valid only up to this time.
		- `validAfter` is 6-byte timestamp. The UserOp is valid only after this time.
- The aggregator:
	- If an account uses an aggregator, then its address is returned by `simulateValidation()` reverting with `ValidationResultWithAggregator` instead of `ValidationResult`
	- To accept the UserOp, the bundler must call `validateUserOpSignature()` to validate the userOp's signature
	- `aggregateSignatures()` must aggregate all UserOp signature into a single value.
	- Note that the above methods are helper method for the bundler. The bundler MAY use a native library to perform the same validation and aggregation logic.
	- `validateSignatures()` MUST validate the aggregated signature matches for all UserOperations in the array, and revert otherwise. This method is called on-chain by `handleOps()`
### Required entry point contract functionality
- There are 2 separate entry point methods: `handleOps` and `handleAggregatedOps`
	- `handleOps` handle userOps of accounts that don't require any signature aggregator.
	- `handleAggregatedOps` can handle a batch that contains userOps of multiple aggregators.
	- `handleAggregatedOps` performs the same logic below as `handleOps`, but it must transfer the correct aggregator to each userOp, and also must call `validateSignatures` on each aggregator after doing all the per-account validation. The entry point's `handleOps` function must perform the following steps. It must make two loops, the **verification loop** and the **execution loop**. In the verification loop, the `handleOps` call must perform the following steps for each `UserOperation`: 
	- **Create the account if it does not yet exist**, using the initcode provided in the `UserOperation`. If the account doesn't exist, and the initcode is empty, or doesn't deploy a contract at the "sender" address, the call must fail.
	- Call `validateUserOp` on the account, passing the UserOperation, the required fee and aggregator. The account should verify the operation's signature, and pay the fee if the account considers the operation valid. If any `validateUserOp` call fails, `handleOps` must skip execution at least that operation, and may revert entirely.
- In the execution loop, the `handleOps` call must perform the following steps for each `UserOperation`:
	- Call the account with the `UserOperation`'s calldata. It's up to the account to choose how to parse the calldata; an expected workflow is for the accout to have an `execute` function that parses the remaining calldata as a series of one or more call that the account should make.
	![[Screenshot from 2023-03-20 15-37-48.png]]
### Extension: paymasters
- We extend the entry point logic support **paymasters** that can sponsor for other users. This feature can be used to allow application developers to subsidize fees for their users, allow user to pay fees with ERC-20 tokens and many other use cases. When the paymaster is not equal to zero address, the entry point implements a different flow:
 ![[Screenshot from 2023-03-20 16-02-52.png]]
 
 During the verification loop, in addition to calling `validateUserOp`, the `handleOps` execution also must check that the paymaster has enough ETH deposited with the entry point to pay for the operation, and then call `validatePaymasterUserOp` on the paymaster to verify that the paymaster is willing to pay for the operation. Note that in this case, the `validateUserOp` is called with a `missingAccountFunds` to 0 to reflect that the account's deposit is not used for payment for this userOp.

- If the paymaster's `validatePaymasterUserOp` returns a "context", then `handleOps` must call `postOp` on the paymaster after making the main execution call. It must guarantee the execution of `postOp`, by making the main execution inside an inner call context, and if the inner call context reverts attempting to call `postOp` again in an outer call context. 
### Client behavior upon receiving a UserOperation
- When a client receives `UserOperation`, it must first run some basic sanity check:
	- Either `sender` is an existing contract, or the `initCode` is not empty
	- If `initCode` is not empty, parse first 20 bytes as a factory address. Record whether the factory is staked, in case the later simulation indicates that it needs to be. If the factory accesses global state, it must be staked.
	- The `verificationGasLimit` is sufficiently low (<= MAX_VERIFICATION_GAS) and the `preVerificationGas` is sufficiently high 
	- The `paymasterAndData` is either empty, or start with the paymaster address, which is a contract that: 
		- currently has nonempty code on chain.
		- has a sufficient deposit to pay for the `UserOperation`.
		- is not currently banned.
	- The callgas is at least the cost of a `CALL` with non-zero value.
	- The `maxFeePerGas` and `maxPriorityFeePerGas` are above a configurable minimum value that the client is willing to accept. At the minimum, they are sufficiently high to be included with the current `block.basefee`
	- The sender doesn't have another `UserOperation` already present in the pool. Only one `UserOperation` per sender may be included in a single batch, A sender is exempt from this rule and may have multiple `UserOperations` in the pool and in a batch if it is staked, but this exception is of limited use to normal account.
### Simulation

**Simulation rationale**
- In order to add a UserOpearation into the mempool, we neeed to "simulate" its validation to make sure it is valid, and that it is capable of paying for its own execution. In addition, we need to verify that the same will hold true when executed-onchain. For this purpose, a UserOperation is not allowed to access any information that might change between simulation and execution, such as current blocktime, number, hash, etc. In addition, a UserOperation is only allowed to access data related to this sender address:
- Multiple UserOperation should note acces the same storage, so that it is impossible to invalidate a large number of UserOperation with a single state change. There are 3 special contract that interact with the account: the factory (initCode) that deploy contracts, the paymaster that can pay for the gas and signature aggregator. Each of these contracts is also restricted in its storage access, to make sure UserOperation validations are isolated.

**Specification**
- To simulate `UserOperation` validation, the client make a view call to `simulateValidation(userop)`
- This method always revert with `ValidationResult` as successul response. If the call reverts with other error, the client reject this `userOp`
- The simulatec call perform the full validation, by calling:
	- If `initCode` is present, create the account
	- `account.validateUserOp`
	- If specified a paymaster: `paymaster.validatePaymasterUserOp`
- Either `validateUserOp` or `validatePaymasterUserOp` may return a "validAfter" and "validUntil" timestamps, which is the time-range that this UserOperation is valid on-chain. The `simulateValidation` call return this range. A node MAY drop a `UserOperation` if it expires too soon. If the `ValidationResult` includes `sigFail`, client should drop the `UserOperation`  
- While simulating `userOp` validation, the client should make sure that:
	- May not invoke any forbidden opcodes
	- Must not use GAS opcode (unless followed immediately by one of the CALLs)
	- Storage access is limited as follow:
		- self storage (of factory/paymaster, respectively) is allowed, but only if self entity is staked.
		- account storage access is allowed
		- in any case, may not use storage used by another UserOp `sender` in the same bundle
	- Limitation on "CALL" opcodes
		- Must not use value
		- must not revert with out-of-gas
		- destination address must have code 
		- cannot call EntryPoint's method, except for `depositFor`
	- `EXTCODEHASH` of every addres access does not change between first and second simulations of the op.
	- `EXTCODEHASH`, `EXTCODELENGTH`,`EXTCODECOPY` may not access address with no code
	- If `op.initcode.length != 0`, allow only one `CREATE2` opcode call, otherwise forbid `CREATA2`

**Storage associated with an address**
- We define storage slots as "associated with an address" as all the slots that uniquely related on this address, and cannot be related with any other adddress. 
- Address `A` is associated with:
	- Slots of contract `A` address itself.
	- Slots `A` on any other address.
	- Slots of type `keccak(A || X) + n` on any other address. `n` is an offset value up to 128, to allow accessing fields in the format `mapping(address => struct)` 

### Bundling
- During bundling, the client should:
	- Exclude UserOps that access any sender address of another UserOp in the same batch.
	- Exclude UserOps that access any address created by another UserOp validation in the same batch (via factory).
	- For each paymaster used in the batch, keep track of the balance while adding UserOps. Ensure that it has sufficient deposit to pay for all the UserOps that use it.
	- Sort UserOps by aggregator, to create the lists of UserOps-per-aggregator.
	- For each aggregator, run the aggregator-specific code to create aggregated signature, and update the UserOps.
- After creating the batch, before including the transaction in a block, the client should:
	- Run `eth_estimateGas` with maximum possible gas, to verify the entire `handleOps` batch transaction, and used the estimated gas for the actual transaction execution.
	- If the call reverted, check the `FailedOp` event. A `FailedOp` during `handleOps` simulation is an unexpected event since it was supposed to be caught by the single-UserOperation simulation. Remove the failed op that caused the revert from the batch and drop from the mempool. If the error is caused by a factory or paymaster, then also drop from mempool all other UserOps of this entity. Repeat until `eth_estimateGas` succeeds.
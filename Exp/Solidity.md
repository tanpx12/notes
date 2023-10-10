# Gas optimization 
## 1. Packing of storage variable
- Pack variables in one slot by defining them in a lower data type. Packing only helps when multiple variables of the packed slot are being accessed in the same call. If not done correctly, it increases gas costs instead, due to shift required.

- Before: 
``` solidity
contract MyContract{
	uint32 x;  // Storage slot 0
	uint256 y; // Storage slot 1
	uint32 z;  // Storage slot 2
}
```

- After: 
``` solidity
contract MyContract{
	uint32 x;  // Storage slot 0
	uint32 z;  // Storage slot 0
	uint256 y; // Storage slot 1
}
```

## 2. Local variable assignment
- Catch frequently used storage variables in memory/stack, converting multiple `SLOAD` into 1 `SLOAD`.
- Before: 
``` solidity
uint256 length = 10;

function foo() public {
	for(uint256 i = 0; i < length; i++)
		\\ do smth 
}
```

- After:
``` solidity
uint256 length = 10;

function foo() public {
	uint256 l = length;
	for(uint256 i = 0; i < l; i++)
		\\ do smth 
}
```

## 3. Use fixed size bytes array rather than string or bytes[]
- If the string you are dealing with can be limited to max of 32 characters, use `bytes32` instead of dynamic `bytes` array of `string`.

- Before: 
``` solidity
string a;
function add(string str){
	a = str;
}
```

- After: 
``` solidity
bytes32 a;
function add(bytes32 str){
	a = str;
}
```

## 4. Use immutable and constant
- Use immutable if you want to assign a permanent value at construction. Use constants if you already know the permanent value. Both get directly embedded in bytecode, saving `SLOAD`.

## 5. Using unchecked
- Use unchecked for arithmetic where you are sure if the variable won't over or undeflow, saving gas costs for checks added from solidity 0.8.0

## 6. Use calldata instead of memory for funtion parameters
- It is generally cheaper to load variables directly from `calldata`, rather than copying them to the `memory`. Only use `memory` if the variable need to be modified.

## 7. Use custom errors to save deployment and runtime costs in case of reverts
- Instead of using string for error messages (e.g, `require(msg.sender == owner, "unauthorized")`), you can use custom errors to reduce both deployment and runtime gas costs. In addition, they are very convenient as you can easily pass dynamic information to them 

- Before: 
``` solidity
function add(uint256 _amount) public {
	require(msg.sender == owner, "unauthorized");
	
	total += _amount;
}
```

- After: 
``` solidity
error Unauthorized(address caller);

function add(uint256 _amount) public {
	if(msg.sender != owner) revert Unauthorized(msg.sender);
	
	total += _amount;
}
```

## 8. Refactor a modifier to call a local function instead of directly having the code in the modifier, saving bytecode size and thereby deployment cost
- Modifiers code is copied in all instances where, it's used, increasing bytecode size. By doing a refactor to the internal function, one can reduce bytecode size significantly at the cost of one `JUMP`. Consider doing this only if you are constrained by bytecode size.

## 9. Use `indexed` events as they are less costly compared to non-indexed ones
- Using keyword `indexed` for value types such as uint, bool, and address save gas costs. However, using `indexed` for arbitrary length type (like string and bytes) are more expensive than the unindexed one.

## 10. Use struct when dealing with different input arrays to enforce array length matchin 
- When the length of all input arrays needs to be the same, use a struct to combine multiple input arrays so you don't have to manually validate their lengths

- Before: 
``` solidity
function vote(uint8[] calldata v, bytes[32] calldata r, bytes[32] calldata s) public {
	require(v.length == r.length == s.length, "length not matching");
}
```

- After: 
``` solidity
struct Signature{
	uint8 v;
	bytes32 r;
	bytes32 s;
}

function vote(Signature[] calldata sigs) public {
	\\ no need to check length
}
```

## 11. Short circuit with || and &&
- Put the lower-cost expression first so the higher-cost expression may be skipped (short-circuit). 


# Security

# Note
## What is gas?
-> Gas refers to the unit that measures the amount of computational effort required to execute specific operations on the network.
![[eth-gas.png]]

`total fee = unit of gas used * (base fee + priority fee)`
`refund = max fee per gas - (base fee + priority fee)`

## What is gas limit?
-> Gas limit refers to the maximum amount of gas you are willing to consume on a transaction.

For example, if you put a gas limit of 50,000 for a simple ETH transfer, the EVM would consume 21,000 and you will get back the remaining 29,000. However, if you specify too little gas, let says 20,000, the EVM *will consume your 20,000 gas units* attempting to fulfill the transaction, but *it will not complete*. *The EVM then reverts any change*, but since the miner has already done 20,000 gas unit worth of work, *that gas is consumed*.

## Block size
-> Each block has target size of 15M gas, but it can increase or decrese in accordance with the network demand (up to 30M gas). *Blocksize increase* -> *base fee of next block increase*.

## SSTORE opcode gas cost
- if `gas_left <= 2300` 
	- Throw `OUT_OF_GAS_ERROR` (can't `sstore` with < 2300 gas for backwards compability)
- if `(context_addr, target_storage_key)` not in `touched_storage_slots` -> `gas_cost += 2100`
- if `(new_val == current_val)` -> `gas_cost += 100`
- else `new_val != current_val`:
	- if `current_val == orig_val` ("clean slot", not yet updated in current execution context)
		- if `orig_val == 0` (slot started zero, current still zero, now beign changed to nonzero) -> `gas_cost += 2000`
		- else `orig_val != 0` (slot started nonzero, currently still same nonzero value, now being changed):
			- `gas_cost += 2900` and update the refund as follow
			- if `new_val == 0` -> `gas_refund += 4800`
	- else `current_val != orig_val` ("dirty slot", already updated in current execution context)
		- `gas_cost += 100` and update the refund as follow
		- if `orig_val != 0` (execution context started with a nonzero value in slot):
			- if `current_val == 0` (slot started nonzero, currently zero, now being changed to nonzero) -> `gas_refund -= 4800`
			- else if `new_val == 0` (slot started nonzero, currently still nonzero, now being changed to zero) -> `gas_refund += 4800`
		- if `new_val == orig_val` (slot reset to the value it started with):
			- if `orig_val == 0` (slot started zero, currently nonzero, now being reset to zero) -> `gas_cost += 19900`
			- else `orig_val != 0` (slot started nonzero, currently still nonzero, now reset to orig. nonzero) -> `gas_refund += 2800`
[::ref][https://github.com/wolflo/evm-opcodes/blob/main/gas.md#a7-sstore]

## Why does `uint8` cost more gas than `uint256`?
-> The EVM works with 256bit/32byte words (debatable design decision). Every operation is based on these base units. If your data is smaller, further operations are needed to *downscale from 256 bits to 8 bits*, hence why you see increased costs.
[::ref][https://ethereum.stackexchange.com/questions/3067/why-does-uint8-cost-more-gas-than-uint256]

## The difference between function selector and function signature
-> A function selector is a 4-byte value that is used to identify a specific function in a smart contract. It is calculated by taking the first 4 bytes of the keccak-256 hash of the function's signature. The function signature is a string that represents the name of the function and the types of its input parameters.
[::ref][https://ethereum.stackexchange.com/questions/135205/what-is-a-function-signature-and-function-selector-in-solidity-and-evm-language]

## OPCODE gas costs 
 
| Operation        | Gas          | Description                       |
| ---------------- | ------------ | --------------------------------- |
| ADD/SUB          | 3            | arithmetic operation              |
| MUL/DIV          | 5            | arithmetic operation              |
| ADDMOD/MULMOD    | 8            | arithmetic operation              |
| AND/OR/XOR       | 3            | bitwise logic operation           |
| LT/GT/SLT/SGT/EQ | 3            | comparison operation              |
| POP              | 2            | stack operation                   |
| PUSH/DUP/SWAP    | 3            | stack operation                   |
| MLOAD/MSTORE     | 3            | memory operation                  |
| JUMP             | 8            | unconditional jump                |
| JUMPI            | 10           | conditional jump                  |
| SLOAD            | 200          | storage operation                 |
| SSTORE           | 5,000/20,000 | storage operation                 |
| BALANCE          | 400          | get balance of an account         |
| CREATE           | 32,000       | create a new account using CREATE |
| CALL             | 25,000       | create a new account using CALL   | 
|                  |              |                                   |

[Full table here][https://github.com/wolflo/evm-opcodes]


## Mapping vs array
- `mapping` is generally recommended. For this use case of a contract, which could have an unlimited number of documents, which could be updated, the recommendation holds.
- The main advantage of an `array` is for iteration. But the iteration needs to be limited, not only for speed, but potentially for security.

=> Mapping is very fast and cheap. If you dont have the purpose of iterating your data, use mapping. For structs mapping is very efficient. 

## Proxy pattern
- Transparent proxy: Transparent proxy pattern includes the upgrade functionality within the proxy contract itself. An admin role is assigned with privilege to interact with the proxy contract directly to update the referenced logic implementation address. Callers that do not have the admin privilege will have their call delegated to the implementation contract.
- UUPS (Universal Upgradeable Proxy Standard): includes the upgrade functionality in the logic implementation contract. Because the upgrade mechanism is in the implementation, later versions can remove related logic to disable future upgrades. All calls are forwarded from the proxy contract to the logic implementation.
- Beacon: allows multiple proxy contracts to share one logic implementation by referencing the beacon contract. The beacon contract provides the logic implementation contract address to calling proxies and only the beacon contract needs to be updated when upgrading with a new logic implementation address.
[::ref][https://www.certik.com/resources/blog/FnfYrOCsy3MG9s9gixfbJ-upgradeable-proxy-contract-security-best-practices]

## Stack
- Is a data location in EVM
- Only available in the function scope
- **Properties** :
	- Cheapest to use data location among all others.
	- The EVM use 4 basic opcodes to manipulate the stack: `PUSH`, `POP`, `SWAP`and `DUP`
	- Other opcodes arguments always take the topmost items on the stack.
	- The opcodes related to `storage`, `memory` and `calldata` load values from these data locations into the stack using the opcodes related to each data location.
- **Basic stack opcodes**:
	- `POP`: remove the first item on the top of the stack. 
	- `PUSH`: push an item on the top of the stack. `PUSH` instructions can be `PUSH1`,...,`PUSH32` indicate to push 1 byte up to 32 bytes on the stack.
	- `DUP`: instruct to duplicate the n-th stack item and to put it on top of the stack, where n can be 1 up to 16.
	- `SWAP`: instruct to swap the item on top of the stack with the n-th item where n can be 1 up to 16.
- How opcodes consume and return arguments from/to the stack
	- In general, opcodes take the **topmost items** on the stack.
- Layout of stack:
	- words on the stack (thus size of stack items) are **256 bits** long
	- limited in size = the stack can hold up to **1024 values**
	- only the **16th topmost item** on the stack can be accessed => try to access elements **deeper than 16** will throw "stack to deep" error.
- Some workarounds to access more than 16th deeper elements 
	- It is also possible to move stack elements to storage or memory to get deeper access to the stack. But it is not possible to access arbitrary elements deeper in the stack without first removing elements from the top of the stack.
- Stack vs "Call" Stack
	- There are 2 separate stacks, one is used by the contract for data manipulation, the other one is used when calling other contract. Both have 1024 entries of 32 bytes each but they are different, they are not the same stack.
	- The "normal" stack is where the opcodes take their arguments. The "call" stack related to the stack about calls between contracts.
[::ref][https://betterprogramming.pub/solidity-tutorial-all-about-stack-c1ec6070fe60#49f3]

## selfdestruct
- `selfdestruct` function deletes the instance of the smart contract from the blockchain and transfers all the remaining ETH store in it to the address passed as an argument. 
	`selfdestruct("address_to_send_ETH_before_deletion")`
-> Use a variable to store balance instead of using `address(this).balance` 
- 3 way to force a contract (without receive() or fallback() function) receive ETH: 
	- `selfdestruct` send
	- set block fee recipient address to the contract address.
	- set withdrawal address on Beacon chain to the contract address.

# Cheat Sheet 

## Function visibility specifiers

- `public`: visible externally and internally
- `private`: only visible in the current contract (its derived contract can not access `private`).
- `external`: only visible externally.
- `internal`: only visible inside that contract and its derived contracts.

## Function modifier specifiers

- `pure` for functions: Disallows modification or access of state.
- `view` for functions: Disallows modification of state.
- `payable` for functions: Allow them to receive Ether together with a call.
- `constant` for state variables: Disallows assignment (except initialisation), does not ocupy storage slot.
- `immutable` for state variables: Allow exactly one assignment at construction time and is constant afterwards. Is stored in code.
- `anonymous` for events: Does not store event signature as topic.
- `indexed` for events parameters: Store the parameter as topic.
- `virtual` for function and modifiers: Allow the function's or modifier's behavior to be changed in derived contract.
- `override`: State that this function, modifier or public state variable changes the behavior of a function or modifier in a base contract.
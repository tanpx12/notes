# Easy
## 1.What is the difference between private, internal, public, and external functions?
- `private`: only accessible inside current contract.
- `internal`: can only be accessed from within the current contract or contracts deriving from it.
- `public`: accessible from anywhere.
- `external`: accessible from anywhere except for current contract and derived one.

## 2.Approximately, how large can a smart contract be?
- Maximum of 24kb

## 3.What is the difference between create and create2?
- CREATE :
	- Hashing the 'account nonce', which is equivalent to the number of transactions completed by the account so far.  
	    `new_address = keccak256(sender, nonce);`
- CREATE2 :
	- With CREATE2, the address is determined by an arbitrary salt value and the init_code.
	- The hashed Bytecode that will be deployed on that particular address.  
		`new_address = keccak256(0xFF, sender, salt, bytecode);`

## 4.What major change happened with Solidity 0.8.0?
- Arithmetic operations revert on underflow and overflow.
- ABI coder v2 is activated by default.

## 5.What special operation is required for proxies to work?
- Delegate call and fallback function

## 6.Prior to EIP-1559, how do you calculate the dollar cost of an Ethereum transaction?
- Fee (in \$) = gas units * gas price per unit * eth price (in \$)

## 7.What are the challenges of creating a random number on the blockchain?
- Since everything is public on blockchain, anyone could simulate the random function => no more randomness

## 8.What is the different between Dutch auction and English auction?
- A Dutch auction starts with a high price and goes down, while an English auction starts with a low price and goes up.

## 9.What is the difference between transfer and transferFrom in ERC20?
- `transfer` transfer the assets from the balance the msg.sender to other address while `transferFrom` transfer the assets which msg.sender have the allowance to spend.

## 10.Gas limit per block
- 30M (x2 target gas per block).

## 11.What is the different between `assert` and `require`? 
- `assert()` function when false, uses up all the remaining gas and reverts all the changes made.
- `require()` function when false, also revert back all the changes made to the contract but does refund all the remaining gas fee we offered to pay.

## 12.Reentrancy attack
- A reentrancy attack occurs when the contracts fails to update state before sending fund, the attacker can continuously call the function to drain fund.

# Medium
## 1.What is the different between transfer and send? Why should they not be used?
- `.send()`: The send method is the low-level counterpart to transfer. It also has a gas limit of **2300** and should be used when the error must be handled in the contract without reverting all state changes.
- `.transfer()`: In most cases, this should be the preferred method of transferring ether because it automatically reverts in the event of an error. Because it only provides **2300** gas, it protects your contract from re-entry attacks.
- The whole reason `transfer()` was introduced was to address the cause of the infamous hack on The DAO. The idea behind it was that **_2300 gas is enough to emit a log entry but insufficient to make a reentrant call that then modifies storage._** This would make sense under the assumption that gas costs wouldn’t change, but that assumption turned out to be incorrect. We now recommend that transfer() be avoided as gas costs can and will change.

## 2.What is a storage collision in a proxy contract?
- In an upgradeable contract system, a proxy contract does not declare state variable but rather uses pseudo-random storage slots to store important data. The proxy contract save values from the logic implementation state variable in the relative position they were declared. If the proxy contract declares its own state variable, the storage collision will happen when both the proxy and logic contract attempt to use the same storage slot. 

## 3.What is the difference between `abi.encode` and `abi.encodePacked`
- `abi.encode` encodes its parameters using the [ABI specs](https://docs.soliditylang.org/en/v0.8.0/abi-spec.html). The ABI was designed to make calls to contracts. Parameters are padded to 32 bytes. If you are making calls to a contract you likely have to use `abi.encode`
- `abi.encodePacked` encodes its parameters using the minimal space required by the type. Encoding an uint8 it will use 1 byte. It is used when you want to save some space - **not** call a contract.

## 4.How many arguments can a solidity event have?
- Simply put Events are treated as functions so they have a limit of **17 arguments** (array count as 2).
- [::ref][https://ethereum.stackexchange.com/questions/43459/what-are-limitations-of-event-arguments]

## 5.What changed with block.timestamp before and after proof of stake?
- Under proof of work, the Ethereum block interval varied, but **miners could modify the block timestamp by +/-15 seconds**, as long as the **modified value was greater** than the parent timestamp, without the block getting rejected.
- (Post-Merge) consensus on valid blocks is pre-determined timestamps that are not modifiable. Each slot has an expected timestamp, and a block **without that exact timestamp is not valid**.

## 6.What is frontrunning?
- Frontrunning (also known as [Priority Gas Auctions](https://arxiv.org/pdf/1904.05234.pdf) (PGAs)): Transaction A is broadcasted with a **higher** gas price than an already pending transaction B so that A gets mined **before** B.
- [Backrunning](https://github.com/ethereum/go-ethereum/issues/21350): Transaction A is broadcasted with a **slightly lower** gas price than already pending transaction B so that A gets mined **right** **after** B in the same block.

## 7.What is a commit-reveal scheme and when would you use it?
- A commitment scheme is a cryptographic algorithm used to allow someone to commit to a value while keeping it hidden from others with the ability to reveal it later. We want to use it when we want to temporarily hide some value.

## 8.How does Ethereum determine the BASEFEE in EIP-1559?
- Base on the Gas use in the previous block. The base fee will increase by a maximum of 12.5% per block if the target block size is exceeded

## 9.How does an AMM price assets?
- Uniswap v1, v2: Constant product function : $x*y=k$  
- Uniswap v3: 

## 10.What is the effect on gas of making a function payable?
- There are additional opcodes that are executed while calling a **non-payable** function which ensures that the function shall only be executed if the ether (msg.value) is sent along with the transaction is exactly equal to ZERO => less gas for payable function.

## 11.What is gas griefing ?
- **A gas griefing attack happens when a user sends the amount of gas required to execute the target smart contract, but not its sub calls.** In most cases, this results in uncontrolled behavior that could have a dangerous impact on the business logic.

## 12.What is the free memory pointer and where is it stored?
- The free memory pointer is **a pointer (i.e. shows where to go) to the next available slot of memory**. Meaning that if you need to create a new uint256 for example, the free memory pointer will let the EVM know where to create the new uint256.
- Solidity reserves four 32-byte slots, with specific byte ranges (inclusive of endpoints) being used as follows: 
	- `0x00`-`0x3f` (64 bytes): scratch space for hashing method
	- `0x40`-`0x5f` (32 bytes): currently allocated memory size (aka. free memory pointer)
	- `0x60`-`0x7f` (32 bytes): zero slot
- Scratch space can be used between statements (i.e. within inline assembly). The zero slot is used as initial value for dynamic memory arrays and should never be written to (the free memory pointer points to `0x80` initially).
## 13. What is the minimum valid calldata size for `function foo(uint256[] memory a, uint256[] memory b) external view ...`

| 4 bytes           | 32 bytes   | 32 bytes   | 32 bytes   | 32 bytes   | (optional)     |
| ----------------- | ---------- | ---------- | ---------- | ---------- | -------------- |
| function selector | arg 1 addr | arg 2 addr | arg 1 size | arg 2 size | calldata value | 
=> min calldata size = 32 * 4 + 4 = 132 bytes

# Hard 
## 1.How does ABI encoding vary between calldata and memory, if at all?
- ABI encoding can vary between `calldata` and `memory` in certain scenarios, particularly when it comes to handling arrays and strings.
- Encoding an array in `calldata`:  
    - `calldata` encoding: \[pointer to array data, array length\]
    - Data is expected to be located at the specified memory address.
- Encoding an array in `memory`:
    - `memory` encoding: \[array data\]
    - The full array data is stored in the allocated memory space.

## 2.How many storage slots does this use? `uint64[] x = [1,2,3,4,5]`? Does it differ from memory?
- The storage layout for dynamically-sized arrays, such as `uint64[]`, is different from the memory layout.
- Dynamically-sized array is consider to occupy only 32 bytes and its elements are stored starting at a different storage slot that is computed using a `keccak256` hash. 
- Suppose that the storage location of the mapping of array ends up being a slot `p` => the array data is located starting at `keccak256(p)` and it is laid out in the same way as statically-sized array. 
- For `uint64[] x = [1,2,3,4,5]`. It would take up 3 slots in storage: 
	- 32 bytes (1 slot) for array size.
	- 40 bytes (2 slot) for 5 uint64 elements.
- For memory `uint64[] x = [1,2,3,4,5]`, it would take up to 6 consecutive slots (1 for array size and 5 for its elements).

## 3.Why is strict inequality comparisons more gas efficient than ≤ or ≥? What extra opcode(s) are added?
- gte of lte use an addition ISZERO op code to check whether the a - b subtract = 0 or not

## 4.How many storage slots does a string take up?
- Depend on the length of the string: 
	- If the string is short (let say "abcd") the it will use the short format which take only 1 slot: 1 byte its length along with 4 bytes for 4 character "a", "b", "c", "d" the remaining bytes will be set to 0.
	- If the string is longer than 31 bytes (let say "0123456789012345678901234567890123456789" - 40 characters) then it will use the long format. A single slot placed at the normal location between other storage variables will only contain the length shifted left by one 1 bit and set the last bit to 1 to indicate that it is the long format => the value in this slot will be $40*2+1=81$. The characters of the string will be store in 2 slot (64 bytes) since the length of the string is 40 bytes. The remaining unused bytes will be set to 0.
[::ref][https://ethereum.stackexchange.com/questions/107282/storage-and-memory-layout-of-strings]

## 5.Under what circumstances do addresses with leading zeros save gas and why?
- Following the rules of the yellow paper, it costs 4 gas for every zero bytes of transaction data (`msg.data`). Compare that to the cost of 68 gas, or 17 times as expensive for a non-zero byte. 
[::ref][https://ethereum.stackexchange.com/questions/101311/how-much-gas-do-0x00000000-addresses-and-0x00000000-methods-really-save]

## 6.What is the relationship between variable scope and stack depth?
- Escaping a variable scope will `POP` all variables inside that scope thus reduce the stack depth of the parent scope.
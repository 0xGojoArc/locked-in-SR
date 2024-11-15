##### [[Gas]]


- Gas is unit of computational measurement.
- The blockchains are run by a group of different nodes, and the economic incentives for the *node or miner or validator* is that **they get paid in the native currency of the blockchain** to process the tx, i.e. to run nodes.
- The more complicated the tx is the more gas one has to pay.
- when transaction is broken down to the lowest level, assembly opcodes, and each of those opcodes(instructions) have specific amount of gas.

	 **Transaction Fee = Gas Price * Gas Used**

 - **Gas Price** is comprised of **Base Fee** and **Priority** Fee


- In Ethereum, as per [[EIP-1559]] every tx comes with base fee.

- **Base Fee:**
	- **Base fee** is the minimum gas price to send a tx.
	- It is priced in Gwei
	- Base fee ends up getting **burnt** - with every tx a little bit of eth is removed from circulation i.e. getting burnt
	- This the cost per gas
	- Base Fee goes up and down depending on the congestion of a block. If a block is >50% full, the fee goes up, <50% and the fee goes down. 
- **Max fee:**
	- Max fee is the maximum price we are willing to pay for the tx. 
- **Max Priority:** 
	- Max Priority is the maximum tip we are willing to give to miners.

    - **1 Ether = 1000000000 Gwei <small>(Gigawei)</small> = 1000000000000000000 Wei <small>(18 zeroes) </small>**

- Transaction gets expensive the more people use the chain

##### Storage Variables/Global variables/State variables

- These variables stay permanently, they're stuck in something called Storage.
- Storage is big array or a giant list of all the variables that we create.
- storage works as the giant list associated with a contract.
- every single variable and every single value in this storage section is slotted into 32 byte slot
- [[uint256]] variable is stored as hex version representation 
- hex version is a string of 64 character, each character representing 4 bits(since 256 bits = 64 hex characters).
-
- dynamic values
	- like mappings and dynamic arrays, the elements are stored using a hash function.  
	- for array, storage spot is taken up only for the array length but not the elements
	- elements they  contain are stored starting at a different storage slot that is computed using a keccak256 hash.
	- 

- constant and immutable variables
	- they are stored in the contracts byte code itself, not in the storage slots

- variables inside functions
	- only exist the duration of function
	- do  not get added to storage, they get added to their own memory structure and gets deleted after function is run

- divide total bytes of storage by 32, we get the minimum required slots.
- however, mappings and dynamic arrays can't be stored in between the state variables, bcuz we don't quite know how many elements they would have.

- reading and writing from storage is very expensive
	- SSTORE and SLOAD cost 100, whereas MLOAD and MSTORE cost 3 (read/write from memory).
	- 
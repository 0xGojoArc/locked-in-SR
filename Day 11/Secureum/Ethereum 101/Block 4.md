##### [[Day 11/Secureum/Ethereum 101/Block 4]]
###### Memory
- Data in memory is not persistent, it is volatile.
- linear byte-array and addressable at byte-level
- zero-initialized, meaning every time a new call context is initiated in the EVM, all memory locations are set to zero.
- MLOAD - loads a word from memory and puts it on the stack 
- MSTORE - stores a word in memory from the stack
- MSTORE8 - stores a single byte in memory from the stack
###### Storage
- Non-volatile, data in storage is persistent txs across blockchain
- its key-value store, 256-bit to 256-bit
- it's also zero-initialized
- Every acc has storageRoot field, this is impl as [[MPT]] trie captures all the storage associated with that acc. 
	- This is relevant to Contract Acc that have associated storage.
	- This storageRoot within the Accs are further captured as part of the stateRoot, which is one of fields in the Block Header
- SLOAD - loads word from storage and puts it on to the stack
- SSTORE - stores a word from stack and puts it in the storage
![[EVM-Storage.jpeg]]
###### Calldata
- used for Data Parameters of Txs and Msg Calls
- It is only read-only and byte-addressable
- Three specific Instructions that operate with Calldata
	1. CALLDATASIZE - gives size of supplied calldata to the stack
	2. CALLDATALOAD - loads calldata supplied on to the stack
	3. CALLDATACOPY - copies supplied calldata to specific region of memory

###### EVM Architecture
- EVM code is stored separately in a virtual ROM and special instructions to handle the EVM code
###### EVM Ordering
- It uses Big-endian ordering
- It means, Most Significant Byte (MSB) of a word is stored at the smallest memory Address and Least Significant Byte(LSB) at Largest memory Address.
##### Instruction Set
- It can be categorised into 11 categories
	1. Stop and Arithmetic
	2. Comparison and Bitwise logic
	3. SHA3
	4. Environmental Information
	5. Block Information
	6. Stack, Memory, Storage and Flow
	7. Push
	8. Duplication
	9. Exchange
	10. Logging
	11. System
	
**1. Stop and Arithmetic : 
- `0x00 STOP 0 0
	-  Hex repr of OpCode, 
	-  STOP is mnemonic
	- numbers after the mnemonic is no. of stack items placed for this instruction and the no. of stack items removed.
	- In the case of STOP, 0 items are placed and 0 items are removed from the stack
- `0x01 ADD 2 1
	- 2 items placed onto the stack, i.e. 2 operands and computed result (Addn) 1 item is placed back onto the stack
- `0x02 MUL 2 1` , `0x03 SUB 2 1`, `0x04 DIV 2 1`...
**2. Comparison and Bitwise logic : 
- `0x10 LT 21` | `0X20 GT 2 1` and many more
**3. SHA3 : 
- ` 0x20 SHA3 2 1`
	- computes Keccak-256 Hash
**4. Environmental Information : 
- These instructions give info about env or execution context of the smart contract that is executing these instructions
- `0x30 ADDRESS 0 1` , `0X31 BALANCE 1 1` , `0X32 ORIGIN 0 1` , `0X33 CALLER 0 1`...
**5. Block Information : 
- Instructions that give info about Tx's Block
- `0x40 BLOCKHASH 1 1` , `0X41 COINBASE 0 1`...
**6. Stack, Memory, Storage and Flow : 
- `0x50 pop 1 0`, `0x51 MLOAD 1 1`, `0X52 MSTORE 2 0`...
**7. Push : 
- These are specific to the Stack
- They push/place operands/items on to the Stack
- Depending on no. items places, there are 32 instructions
- `0x60 PUSH1 0 1` => pushes single byte on to the stack and so all the way to `07F PUSH32 0 1` => pushes 32 bytes, i.e. 256 bits, i.e. full word onto the stack.
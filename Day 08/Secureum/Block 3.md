 ###### Signature purpose
 - 3 purposes
	 1. Authorization - to spend ether or execute contract that it's targeted
	 2. Non-repudiation - once sign included, later can't be denied 
	 3. Integrity - Tx data has not been modified or can not be modified after Tx has been signed

###### Contract Creation
- transaction can result in contract creation
- when a contract creation tx is sent to a special address called, zero address. it has zero in all its bits
- it contains data payload, represents bytecode of contract that is being created
- also contains optional ether amount, new contract created will have this as starting balance

###### Transactions Vs. Messages
- Tx originates Offchain, targets OnChain
- Msg originates OnChain, targets OnChain
- Tx produced by EOA
	- It can either trigger Msg to another EOA => transfer of value/ether
	- Ot it can trigger Msg to Contract acc => running contract code 

- Msg can be triggered in two ways
	- externally by Tx => destination could be  EOA or contract acc
	- Internally within the EVM => when smart contract executes the call family of Opcodes => runs code or value transfer to recipient

###### Transactions in Blockchain
- Txs grouped together and included within a Block
- Blocks are linked together, linked Blocks form the Blockchain

###### Block
- 
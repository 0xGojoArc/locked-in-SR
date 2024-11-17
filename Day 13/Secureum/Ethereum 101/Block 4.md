##### [[Day 13/Secureum/Ethereum 101/Block 4]]
##### Instructions Set

###### System instructions:
- `0xf5 CREATE2 4 1`
	- create a new Contract Acc with a code and storage but the huge difference is **it allows you to create Accounts with Predictable Address**
	- unlike CREATE instruction, where the newly created Addr is dependent on sender's nonce and all the txs is removed and generates predictable addr.
- `0xfa STATICCALL 6 1`
	- It lets callee acc read the state of caller acc without letting it modify
- `0xfd REVERT 2 0`
	- holds the execution of the current executing context, it returns the data and it returns the remaining gas that's left behind after consuming all the gas that was supplied for the txs so far
- `0xfe INVALID 0 0`
	- It consumes all the gas that's supplied as part of triggering tx and used in context on the static analysis tool.
- `0xff SELFDESTRUCT 1 0`
	- Halts execution and also destructs the account of the executing context. 


###### Gas Costs
-  Each of the instructions have different gas costs
- each of them have different requirement computational processing power of the executing ETH node/miner and also Storage requirements, memory and disk accesses on the physical hardware that is running the node.
- simple instructions like `STOP/INVALIDREVERT` gas cost is 0
- Most Arithmetic/Logic/Stack it is 3-5
- `CALL*/BALANCE/EXT*` these have greater processing reqt from the node, costs 2600
- Memory instructions, these are within context on EVM, simple instructions operate on EVM data structure cost 3
- Storage instructions like `SSTORE` costs 20000/5000* and `SLOAD` 2100* => they deal with persistent state and they have to access the disk or the persistent state within the physical machine of the ETH node these cost much more.
- `CREATE` costs 32000 => most expensive, and `SELFDESTRUCT` cost 5000
###### Transaction Reverts
- It can revert for different exceptional conditions, e.g.: Out of Gas, Invalid instructions
- When tx reverts, the state changes made in the context of the EVM so far(all the previous instructions) are discarded.
- The original state is restored. the state before tx started.
###### Transaction Data
- It relevant only if the recipient of the Tx is the Contract Account
- Tx Data contains two components and it has to specify the *Function* of that contract that is being invoked and if that function requires any *Arguments*
- Function and Arguments have to be encoded, encoded according to Application Binary Interface ([[ABI]])

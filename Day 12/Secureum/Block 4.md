##### [[Block 4]]

**8. Duplication :** 
- duplicate items that are already on the stack
- `0x80 DUP1 1 2` => duplicates 1st item in the stack
**9. Exchange : 
- swap items that are already on the stack
- `0x90 SWAP1 2 2` => exchanges 1st and 2nd items and so on...
- `0X9F SWAP16 17 17` => exchanges 1st and 17th items
**10. Logging  :
- Append Log Records from within execution context contract onto blockchain
- these instruction differ in no. of topics that are specified as part of the log.
- `0xa0 LOG0 2 0` , `0xa1 LOG1 3 0`...  til `0xa4 LOG4 6 0` 
	- Log itself is event that fired within the context of the contract
	- In the events, the diff parameters can be specified as either being indexed or non-indexed
	- indexed parameters go into the topics part of the log
	- non-indexed go into data part of the log
**11. System :
- Critical to how system functions
- They allow one to create new Contract Acc, call one form another, revert from current executing context, invalidate some of things that have happened and so on.
- `0xf0 CREATE 3 1` 
	- Used to create new Contract Acc that has associated code and storage with it
	- The Addr of newly created Acc depends on the sender's Addr and nonce of that acc.
	- The newly created contract addr dependent on the previous txs happened from sender's acc.
- `0xf1 CALL 7 1`
	- It allows current execution context to do a Msg call into another account.
	- i.e. A caller account that is doing a Msg call into callee account
	- It let's contract call each other in the executing context
- `0xf2 CALLCODE 7 1` **DEPRECATED**
	- It lets caller acc call callee account and  it allows the called contract to execute code using the calling contract's storage.
- `0xf3 RETURN 2 `
	- returns the output data
- `0xf4 DELEGATECALL 6 1`
	- Executes code from another contract (the target contract) but operates within the context of the calling contract's storage.
	- Any state changes made during the execution affect the calling contract's storage not the the target contract's storage.
	- it means `msg.sender` and `msg.value` and other context variables remain tied to the calling contract.
- `0xf5 CREATE2 4 1`
- 
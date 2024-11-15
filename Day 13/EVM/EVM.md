##### [[EVM]]

###### The Program Counter
- It encodes which instruction stored in the code should be read next by EVM.
- PS is usually incremented by one byte to point to the following instructions with some exceptions.
###### The Stack
- A list of 32-byte elements used to store smart contracts instruction inputs and outputs.
- A new stack is created for each call context, and its destroyed once the call context ends.
- stack depth is max 1024 items.
	- Call context:  In a single tx, you can have multiple function calls, but they may or may not create new call contexts depending on how they are invoked.
	- if you trigger a contract function that makes multiple internal calls directly, they all share the same call context. However, if any of those internal calls are made as separate transactions, each will have its own call context.
	- If the internal functions are called as separate transactions (using low-level calls like `call`, `delegatecall`, or `staticcall`), then new call contexts are created for each of these calls.
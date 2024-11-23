[[Day 23/Secureum/Solidity 101/Solidity 101|Solidity 101]]

###### Events
- It is an abstraction that are built on top of [[EVM]]'s logging functionality
- Emitting events cause the arguments that are supplied to them to be stored in transaction log, log is special data structure in the blockchain
- The logs are associated with the address of the specific contract that created them and stay there as long as a block is accessible
- log and its event data is not accessible from within contracts.
- Application can subscribe and listen to these events through the Remote Procedure Calls(RPC) interface of an Ethereum client.
- E.g.: `Transfer` event from the `ERC20` token
	- `event Transfer(address indexed from, address indexed to, uint256 value);`
	- `Transfer` event records three parameters: `from`，`to`, and `value`，which correspond to the address where the tokens are sent, the receiving address, and the number of tokens being transferred.
	- Parameter `from` and `to` are marked with `indexed` keywords, which will be stored at a special data structure known as `topics` and easily queried by programs.

	**Indexed Parameters**
	- Upto 3 parameters of every event can be specified as the index by using the `index` keyword 
	- these indexed parameters are stored in special data structure called **Topics** instead of the data part of the log
	- `indexed` parameters can be understood as the indexed "key" for events, which can be easily queried by programs.
	- search/filter can be used on Topics
	- The size of each `indexed` parameter is 32 bytes.
	  
	- **Non-indexed parameters** will be stored in the `data` section of the log.
	- They can be interpreted as "value" of the event and can't be retrieved directly. But they can store data with larger size.
	- Therefore, `data` section can be used to store complex data structures, such as `array` and `string`. Moreover, `data` consumes less gas compared to `topic`.

	**Topics**
	- `Topics` is used to describe events.
	- Each event contains a maximum of 4 `topics`.
	- the first `topic` is the event hash: the hash of the event signature.
	- Besides event hash, `topics` can include 3 `indexed` parameters,

	**Emit**
	- Events are triggered by `emit` keyword
	- E.g.: `emit Deposit(msg.sender, _id, msg.value);`
	- From security perspective, it's important to emit correct events and parameters
- 

###### Structs
- It is custom data structure that can group together several variables of same or different types to create something unique to the contract as required by the dev
- Struct members are accessed by dot syntax
	- E.g.: `s.user`, `s.amount`
- It is an aggregate type
###### Enums
- User-defined custom type
- Used to represent finite set of constant integer values
- Every enum can a min of 1 member and max of 256 members
- Since they represent underlying integer values, they can be explicitly converted to and from integers
- E.g.: 
```
enum ActionChoices {GoLeft, GoRight}	
ActionChoices choice;
choice = ActionChoices.GoRight;
```
- one can declare a variables of specific enum types, from the above e.g. `choice` is a variable of `ActionChoices` and it can be assigned the members of `ActionChoices`. `GoRight` has been assigned as choice in the example.
- This is used to improve readability, instead of using integer we can use the specific names that correspond to those integer values.
###### Constructor
- It is a special function that helps in initializing the contract state when the contract is created
- It is optional and can have **only one** per contract.
- It's run only once and once the construction has finished executing the final code of the contract is stored on the blockchain
- This deployed code does not include the constructor code that just finished executing or any other internal functions that are called from within the constructor functions
- From Security perspective this is important as to it lets examine what initializations are being done to the contract state, if not then the default values of the state variables would be used in the context on various contract functions

###### Receive Function
- It gets triggered automatically whenever a contract receives ether via `send/transfer` primitives
- This also gets triggered when a transaction targets a contract with empty `calldata`
- It can have only one receive function for every contract and it can not have any arguments and it cannot return anything and it must also `external` visibility and `payable` specifier state mutability.
- `send/transfer` -> 2300 Gas
- **[[Security]]:** It is important to evaluate this function since it affects contract's ether balance and any assumptions in the contract logic that depends on the contracts ether balance. 
###### Fallback Function
- It gets triggered automatically on a call to contract when none of the functions in the contract match the [[function signature]] specified in the transaction
- Also get triggered if there was no data supplied at all in the transaction and there is no `receive` ether function that we just talked about.
- Similar to receive function, only one one fallback function per contract, but this function can receive and return data if required.
- if it is to receive ether then it needs to use specified payable 
- **[[Security]]:**  Similar to receive function
###### Statically Typed
- type of variables need to specified in the code at compile time
- It performs compile time type checking 
###### Types
- Two types:
	1. Value types
	2. Reference types
- **Value Type:** 
	- Always passed by value
	- Whenever they are used as function arguments or in assignments of expressions they are always copied from one location to the other 
	- E.g.: Boolean, Integer, Address, Enums etc.
	- [[Security]]: it can be considered as safer because copy of that variable is made so the original value of the original state itself is not modified accidentally. Persistence of values should be examined.
- **Reference Type:**
	- It can be modified via multiple names all of which point to or reference the same underlying variable
	- E.g.: Arrays, Structs, Mappings
	- [[Security]]: Riskier, due to different/multiple name for the same variable
###### Default Values
- Variables that are declared but not initialized have default values
- Default values is basically zero-state of type
- E.g.: Boolean: False, Integer: 0, Address: 0, Enum: First member
- [[Security]]: especially in case of default address value, which is 0 has meaning in Ethereum and affects security properties within the contract depend on how its used
###### Scoping
- Variable visibility: Where variable can be used with respect to where it was declared
- Solidity uses C99 Standard for Scoping
- The variable are visible right after declaration to until the end of the smallest curly bracket block that contains that declaration
	- for loop variables are exception
- variables that are parameter like function parameter, modifier parameter, catch parameters are visible inside code block that follows the body of the function or modifier for a function modifier parameter and the catch block for a catch parameter
- Variables and other items declared outside of a code block such as functions, contracts, state variables and user defined types, these are visible even before they are declared.
- [[Security]]: Understanding scoping rules of solidity becomes important when we are doing data flow analysis; manual review or writing tools to do static analysis on contract.
- 


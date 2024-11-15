##### [[Solidity]]

- View and pure do not cost gas, 

- bc view reads data from the blockchain

- Pure reads the data from a function with return value

- If another func(gas cost tx) that updates state calls pure func then it costs gas

- Can create a type using struct keyword

- String are a special type in Sol, have to mention memory or calldata
    
- Mapping helps with mapping key to value pair
    
- Only special types like string, struct, and tuples we need to mention the data locations like memory or calldata
    
- Inheritance works in contracts by using is keyword. However, a parent contract’s function must have virtual keyword and child contract must have override key word in the new function that wants to override. 
    
- Payable keyword in the func is what makes contract receive $
    
- To view the value of the contract use msg.value, i.e. no of wei sent with msg
    
- Use require keyword to force a user to meet the condition, eg; require(msg.value > 1e18) => min 1eth should be sent to this contract
    
- Nonce is tx count for the account
    
- Oracle and chainlink helps in getting the real world data into the chains
    
- Smart contracts are unable to connect with external data, API calls, payment systems, or off-chain resources on their own
    
- Smart contract connectivity problem or Oracle problem: Smart contracts  are deterministic in nature, to reach the consensus among the nodes they cant have random function or api call, bc diff nodes can get diff results and will never be able to reach consensus
    
- Blockchain Oracle, a device that provides external world data to smart contracts
    
- Chainlink is a decentralized Oracle Network
    
- Solidity contract interface: list of all the function definitions without any implementation. When compiled, it gives the ABI. It enforces a set of defined set of properties and functions on a contract for it to operate.
    
- There are no decimals in Solidity, only whole numbers.
    
- Whenever we send tx, we actually make a HTTP post req to RPC URL(node provider.
    
- You can send funds to only payable address.
    
- An address can be typecasted into payable address by payable(msg.sender)
    
- There are three methods to transfer/send the funds; 1. transfer, 2. send, and 3. call
    
- transfer and send methods have a gas limit of 2300, bc to prevent reentrancy attack. It is not recommended to use transfer and send methods anymore
    
- transfer returns an error and automatically reverts if method is not successful while just send returns a boolean and will need to use require to revert
    
- call method is one of the lower-level command
    
- call command is quite powerful, it can be used to call any function in all of ethereum without having the abi
    
- call returns bool and a bytes object, since it can call any function.
    
- The following code shows how the call is, left-hand side is returned, and right hand we are getting balance of this current contract since not calling any func we have empty quotes  
    (bool callSuccess, bytes dataReturned) = payable(msg.sender).call{value: address(this).balance}(“”)
    
- to assign contract owner so that only the owner can call withdraw func
    
-  constructor is called immediately once the contract is deployed and owner gets assigned to this contract
    
- constructor call will be in the same transaction as the contract deployment, not a separate tx like function calls
    
- Modifier, used to create a keyword that can used in functions to add functionality,
    
- the variables that we set one time outside of function and never change it, we can make them constant or immutable variables
    
- They can help in saving the gas
    
- Constant variables are written in all caps
    
- The immutable variable has a prefix of i_variableName, this can declared in a constructor function
    
- Why? bc these variables are stored directly in the bytecode not the storage slot.
    
- require statement error needs to be saved as string array and every charac in the error log gets stored individually costing gas
    
- Now we can do custom error for reverts, declare error at the top outside contract and use if statement to revert
    
- receive and fallback: when ether is sent to a contract without calling any func or sending any data, these two functions help in determining what to do 
    
- When there’s  ether sent to a contract
    
- with no msg.data (i.e. no calldata) then receive func will get triggered
    
- with calldata but it’s not in the contract then fallback will get triggered
    
- If there’s receive func then goes to that receive func else fallback
-  Functions get transformed into function selector

---
#### Events
- Any time you update a storage, you want to emit an event
Two main reasons
- makes migration easier - redeploy a SC, difficult to move all storage to new contract, emitting events makes it easier.
- makes front end indexing easier

- EVM has logging functionality, logs and events are used synonymously.
- events allows you to print stuff to this log data structure in a way gas efficient than storing it in a variable
- smart contracts. can't access logs.
- events are tied to contract or account that emitted this event
- listening to these events is where it's handy
	- tx happened and event emitted and we can listen for these
	- this is how off-chain infrastructure works
	- it's important for frontend 
	- chainlink and graph uses these, to index them and make sense of it and stores it as a graph so it's easy to query them later.

- there are two kinds of parameters in events
	1. Indexed parameter: 
		- can have 3 of these in an event
		- Also called as topics
		- easier to search and query
	2. Non indexed parameter
		- these are ABI encoded and need ABI to decode
		- 
	
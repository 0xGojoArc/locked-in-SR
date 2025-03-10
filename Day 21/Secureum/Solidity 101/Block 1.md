###### Experimental
- used to specify features that are considered as experimental
- SMTChecker is one such feature(as of now 2021), 
- SMT - Satisfiability Modulo Theory, an approach to formal verification.
- In case of solidity, it is used to implement safety checks by querying an SMT Solver
- Various Security/Safety checks performed by SMT Checker
	- *require/assert:* Checker considers all the required statements are assumptions made by developers, and it tries to prove the conditions inside the statement are true, if failures can be proved then it provides counter example to show the user how this assertion can fail.
	- *Arithmetic overflow/underflow*, *Division by 0*, *Unreachable code* and so on.
- This SMT checker affects the security and optimizations of the smart contract.
###### NatSpec
- Ethereum Natural Language Specification Format
- Written with `/// `or `/ ** ... */` and it is specialized comments specific to SolidityEthereum 
- written directly above functions declarations or statements
- there are many different types/tags; `@title`, `@author`, `@notice`, `@dev`, `@param`, `@return` etc.
- They automatically generate JSON Docs for Devs and Users.
###### Contracts
- It is similar to Classes in OOP
- They encapsulate state in the form of variable and logic that allow one to modify that state in the form of functions.
	- state => variables
	- modification => functions
- They inherit from other contracts and interact with other contracts.
- They contain different components; Struct/Enum, State variables. Events, Errors, Modifiers, Constructor, Functions.
- They can be simple vanilla contracts or libraries or Interfaces.
##### Variables
###### State Variables
- These are accessed by all the functions of the contract
- These are stored in a data location called **Contract Storage** 
	- In EVM, there's non-volatile storage, this is where state variables are stored
	- because it needs to persist across transaction that affect contract state.

	**State Visibility**
	 - These visibility specifiers indicate who can see thee state variable and who can access them
	 - There 3 visibility specifiers:
		 1. **public :** 
			 - part of contract interface, and they can be accessed either internally or from outside the contract vis messages
			 - automatic getter functions are generated by the compiler
		 2. **internal :**
			 -  these can be accessed only internally, i.e. from within the current contract or contracts deriving from thos contract
		 3. **private :**
			 - accessed only from within the contract they are defined in not even from contracts that are derived from it.
	- The private visibility specifier really makes these variables private to the contracts and prevents only other contracts from reading these variables on chain but all these variables can be looked at and queried via different interfaces.

	**State Mutability**
	 - Indicates when the state variables can be modified and rules for the modifications
	 - Two types of specifiers
		 1. **Constant:**  These are fixed at compile-time and cannot change throughout the life of the contract.
		 - When you declare a variable as `constant`, its value must be known at compile-time. This means that the value is fixed and cannot change after it has been set, allowing for optimizations in gas usage since the compiler can replace occurrences of this constant with its value directly in the bytecode.
		 2. **Immutable:** These can be assigned a value only once during contract construction and cannot be modified afterward.
	- Compile-time: The compiler checks for errors and optimize the smart contract's code before it is deployed on the blockchain.
	- This allows solidity compiler to prevent reserving any storage slot for these variables.
	  
	**Mutability and Gas**
	-  The gas cost for constant and immutable variable is lower
	- When the compiler encounters a **constant variable** in the code, it recognizes that this value will not change. Therefore, instead of storing a reference to the variable in the bytecode, it can directly replace every instance of that variable with its actual value.
	 e.g.: `uint constant MAX_VALUE = 100`;
	- For instance, if `MAX_VALUE` is used in multiple places, the compiler will substitute it with `100` wherever it appears.
	- For **Immutable variable**, evaluated only once at construction time then the evaluated value is copied to all the places in the code where they are used. 32 bytes are reserved even if they require fewer bytes, so constant variables can be sometimes cheaper than immutable.
###### Functions
- It can be defined inside contract and sometimes outside.
- The outside ones are referred as free functions, they are specified at a File-Level
- Helps in state modifications and transitions
	**Parameters**
	- the function specifies the parameter
	- caller sends in arguments that get assigned to these parameters
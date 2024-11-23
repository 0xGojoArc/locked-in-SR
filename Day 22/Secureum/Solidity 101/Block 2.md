[[Solidity 101]]
##### Functions
###### Return Variables
- in case of unnamed return variables an explicit `return` statement needs to used.
###### Modifiers
- ```modifier modName() {checks;  _;}```
- underscore in the modifier function serves a purpose, it's how modifier is used along with the function
- The popular use case of `modifier` is restrict the access of functions.
	e.g. : `function foo() mod {}` the control flow in the function foo first goes the modifier an depending on check and logic implemented in the modifier, the function `foo` logic gets called at the point where the underscore is used within that modifier.
	- if there are few checks in the modifier and after which the underscore follows then those checks really implement some preconditions that are evaluated before the function's logic is executed.
	- the underscore could also be specified before the checks in the modifier, in this case functions logic gets executed first and then modifier executed its checks.
	- preconditions checks like access control
	- post conditions checks like accounting 
###### Function Visibility
- `public / external / internal / private`
- **public:** 
	- part of contract interface( it means it can be called from other contracts)
	- it can be called internally within the the contract or via msgs
- **external:**
	- these are also part of contract interface via transactions but they can not be called internally
- **internal:**
	- these can accessed within the contract where they are defined or contracts derived from it
- **private:**
	- these are accessed only within the contracts but not from derived contracts
###### Function Mutability
- `pure / view`
- **view:**
	- allowed only to read the state but modify
	- not modify part is enforced at [[EVM]] level using `STATICCALL` opcode
- **pure:**
	- they are neither allowed to read not-modify the contract state
	- not-modify part is enforced at the [[EVM]] level but **not-read part is not enforced at [[EVM]]** - opcodes for that
###### Function Overloading
- **It supports multiple functions with same name but with different parameters types**
- overloaded functions are selected by matching the function declarations within the current scope to the arguments supplied in the function call
###### Free Functions
- functions defined outside contracts
- thye always have implicit internal visibility, their code is included in the all contracts that call them similar to internal library functions
###### Events
- 
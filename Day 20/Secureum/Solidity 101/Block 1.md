##### Block 1

- High level language and targets EVM.

###### Characteristics
- Curly bracket language - curly brackets are used to group together statements
- Object oriented language
- statically typed, types of variables are static and compiled at compile time
- Because if object oriented programming, there's use of inheritance, code reuse modularity in form of libraries and also there are user defined types
###### Layout 
- It can contain arbitrary number of primitives and directives
	- Pragmas/Import Directives
	- Struct/Enum
	- Contract Definitions
- Every Contract themselves can contain:
	- Struct/Enum
	- State Variables
	- Events
	- Errors
	- Modifiers
	- Constructors
	- Functions
- This order is considered as best practice
###### SPDX
- SPDX license Identifier - Software Package Data Exchange
- Is included by the compiler in the bytecode metadata so this becomes machine redable
###### Pragma
- Pragma keyword enables certain features/checks, e.g. `pragma solidity ^0.8.0;`
- Two types of pragma directives
	1. Specifies the Version: There are two types
		1. Version of compiler, Solidity compiler itself
		2. Version of ABI Coder: v1 or v2 
	2. Experimental: helps dev to specify any experimental features, as of 2021 there only one - SMTChecker
- Pragma directives are local to the file where they are specified.
###### Version
- It instructs the compiler at compilation time to check whether it's version matches the one specified by the developer
- usually in the format of `pragma solidity x.y.z`
	- difference in `y` and `z` indictates
	- `y` => breaking changes
	- `z` => bug fixes
- floating pragma, it has caret symbol `^`, indicates contract can be compiled with versions starting with `x.y.z` until `x.y+1.z` 
	- e.g. : `pragma solidity ^0.8.3` can be compiled with any compiler version starting from `0.8.3` going to `0.8.4`, `0.8.5`... but not `0.9.`.
	- transition from `0.8` to `0.9` is what is allowed or prevented by this floating pragma
###### ABI Coder
- two version v1 and v2
- v2 allows encoding decoding of nested arrays and structs
- v2 superset of v1
- 
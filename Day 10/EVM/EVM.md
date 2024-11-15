##### [[EVM]]

- Ethereum Virtual Machine ([[Day 04/EVM]]) is stack-based computer, responsible for execution of smart contact instructions.
- All instruction take their parameter from the stack, **except for PUSHx**, it takes their parameters from the code
###### Smart Contract
- It is a set instructions, each instruction is an opcode.
- EVM executes a  smart contract by reading and executing each instruction sequentially, except for JUMP and JUMPI instructions.
- If there's insufficient gas or not enough values on the stack, the execution reverts.
- Tx reversion can also be triggered with **REVERT** opcode.
##### Execution Environment
- When EVM executes a contract, a context is created for it.
- This context is made of several data regions, each with a distinct purpose, as well as variables like program counter, the current caller, the callee and addr of the current code.
###### Code
- This is a region where instructions are stored.
- Instruction data stored in the code is persistent as a contract account state field.
- [[EOA]] have empty code regions.
- Code is immutable, but it can be read with **CODESIZE** and **CODECOPY**.
- Code of one contract can be ready by other contracts, with instructions **EXTCODESIZE** and **EXTCODECOPY**
###### The Program Counter
- 
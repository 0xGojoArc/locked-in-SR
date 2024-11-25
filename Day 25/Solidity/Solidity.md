[[Day 25/Solidity/Solidity]]
##### Inheritance
- There are two important keywords for inheritance in Solidity:

	- `virtual`: If the functions in the parent contract are expected to be overridden in its child contracts, they should be declared as `virtual`.
	- `override`：If the functions in the child contract override the functions in its parent contract, they should be declared as `override`.
	- **Note**: If a function both overrides and is expected to be overridden, it should be labeled as `virtual override`.


###### Simple inheritance
- Let's start by writing a simple `Grandfather` contract, which contains 1 `Log` event and 3 functions: `hip()`, `pop()`, `grandfather()`, which outputs a string `"Grandfather"`.
```
contract Grandfather {
    event Log(string msg);

    // Apply inheritance to the following 3 functions: hip(), pop(), man()，then log "Grandfather".
    function hip() public virtual{
        emit Log("Grandfather");
    }

    function pop() public virtual{
        emit Log("Grandfather");
    }

    function Grandfather() public virtual {
        emit Log("Grandfather");
    }
}	
```

- Let's define another contract called `Father`, which inherits the `Grandfather` contract. The syntax for inheritance is `contract Father is Grandfather`, which is very intuitive. In the `Father` contract, we rewrote the functions `hip()` and `pop()` with the `override` keyword, changing their output to `"Father"`. We also added a new function called `father`, which output a string `"Father"`.
```
contract Father is Grandfather{
    // Apply inheritance to the following 2 functions: hip() and pop()，then change the log value to "Father".
    function hip() public virtual override{
        emit Log("Father");
    }

    function pop() public virtual override{
        emit Log("Father");
    }

    function father() public virtual{
        emit Log("Father");
    }
}
```
- After deploying the contract, we can see that `Father` contract contains 4 functions. The outputs of `hip()` and `pop()` are successfully rewritten with output `"Father"`, while the output of the inherited `grandfather()` function is still `"Gatherfather"`.
  
###### Multiple inheritance
A solidity contract can inherit multiple contracts. The rules are:

1. For multiple inheritance, parent contracts should be ordered by seniority, from the highest to the lowest. For example: `contract Son is Gatherfather, Father`. A error will be thrown if the order is not correct.
    
2. If a function exists in multiple parent contracts, it must be overridden in the child contract, otherwise an error will occur.
    
3. When a function exists in multiple parent contracts, you need to put all parent contract names after the `override` keyword. For example: `override(Grandfather, Father)`.
```
contract Son is Grandfather, Father{
    // Apply inheritance to the following 2 functions: hip() and pop()，then change the log value to "Son".
    function hip() public virtual override(Grandfather, Father){
        emit Log("Son");
    }

    function pop() public virtual override(Grandfather, Father) {
        emit Log("Son");
    }
```
After deploying the contract, we can see that we successfully rewrote the `hip()` and `pop()` functions in `Son` contract, changing the output to `"Son"`. While the `grandfather()` and `father()` functions inherited from its parent contracts remain unchanged.

###### Inheritance of modifiers
- Likewise, modifiers in Solidity can be inherited as well. Rules for modifier inheritance are similar as the function inheritance, using the `virtual` and `override` keywords.

###### Inheritance of constructors
- Constructors can also be inherited.

###### Calling the functions from the parent contracts
- There are two ways for a child contract to call the functions of the parent contract:

1. **Direct calling**：The child contract can directly call the parent's function with `parentContractName.functionName()`. For example:

```
    function callParent() public{    
        Grandfather.pop();    
        }
```

2. **`super` keyword**：The child contract can use the `super.functionName()` to call the function in the nearest parent contract in the inheritance hierarchy. Solidity inheritance are declared in a right-to-left order: for `contract Son is Grandfather, Father`, `Father` contract is closer than the `Grandfather` contract. Thus, `super.pop()` in the `Son` contract will call `Father.pop()` but not `Grandfather.pop()`.

```
function callParentSuper() public{   
	// call the function one level higher up in the inheritance hierarchy        
	super.pop();    
	}
```
###### Diamond inheritance
- In Object-Oriented Programming, the diamond inheritance refers the scenario that a derived class has two or more base classes.
- When using the `super` keyword on a diamond inheritance chain, it should be noted that it will call the relevant function of each contract in the inheritance chain, not just the nearest parent contract.
- First, we write a base contract called `God`. Then we write two contracts `Adam` and `Eve` inheriting from `God` contract. Lastly, we write another contract `people` inheriting from `Adam` and `Eve`. Each contract has two functions, `foo()` and `bar()`:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

/* Inheritance tree visualized：
  God
 /  \
Adam Eve
 \  /
people
*/
contract God {
    event Log(string message);
    function foo() public virtual {
        emit Log("God.foo called");
    }
    function bar() public virtual {
        emit Log("God.bar called");
    }
}
contract Adam is God {
    function foo() public virtual override {
        emit Log("Adam.foo called");
        Adam.foo();
    }
    function bar() public virtual override {
        emit Log("Adam.bar called");
        super.bar();
    }
}
contract Eve is God {
    function foo() public virtual override {
        emit Log("Eve.foo called");
        Eve.foo();
    }
    function bar() public virtual override {
        emit Log("Eve.bar called");
        super.bar();
    }
}
contract people is Adam, Eve {
    function foo() public override(Adam, Eve) {
        super.foo();
    }
    function bar() public override(Adam, Eve) {
        super.bar();
    }
}
```
- In this example, calling the `super.bar()` function in the `people` contract will call the `Eve`, `Adam`, and `God` contract's `bar()` function, which is different from ordinary multiple inheritance.
- Although `Eve` and `Adam` are both child contracts of the `God` parent contract, the `God` contract will only be called once in the whole process. This is because Solidity borrows the paradigm from Python, forcing a DAG (directed acyclic graph) composed of base classes to guarantee a specific order based on C3 Linearization.

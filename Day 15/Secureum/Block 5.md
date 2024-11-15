##### [[Block 5]]
 #81
###### Function Selector
- It specifies the exact function within the destination contract that needs to be invoked
- The *Function Signature* of the function that needs to be invoked and hashed using the Keccak-256, The **first four bytes**  of this hash are taken as the function selector.
- A function signature is formed by concatenating the function name with its parameter types.(with no space).
	- Fn Sig => `transfer(address,uint256)` 
- Function arguments are encoded and starts from 5th byte onwards.

**Clarification between function parameter types and arguments**
```
//example 
function transfer(address recipient, uint256 amount);

transfer(0x1234567890abcdef1234567890abcdef12345678, 100);
```

1. **Function parameter types :**  `address` and `uint256`
2. **Arguments :** `0x123456....` and `100`

- `recipient` and `amount` are just parameter names
- Its the arguments that will be passed to the function are encoded.


---
---

#### Higher level concepts / Application concepts

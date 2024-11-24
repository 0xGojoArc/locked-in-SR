###### Integers
- There are signed(`int`) and unsigned integers(`uint`)
- They are in sizes from 8 bits to 256 bits
- Operators: Arithmetic/Comparative/Bit/Shift
- [[Security]]: Since the data flow encompas integers, it is crucial. Specifically it becomes security critical because of underflow and overflow

	**Integer Arithmetic**
	- Integers are restricted to a range of values
		- E.g.: `uint256` -> `0` to $2^{256-1}$
	- Any operation that leads to go over this range is called an **[[overflow]]** or an **[[underflow]]**
	- In case of overflows, if eg. variable value is at its maximum value of its range i.e. $2^{256-1}$ the value would wrap to the other side and becomes *zero*.
	- In case of  underflow, if variable value is 0, and contracted logic decremented it by one more then again it causes wrapping to other end and value would be $2^{256-1}$
	- in Solidity version less than `0.8.0`, SafeMath library must be used for underflow and underflow checks
	- Version above `0.8.0`,  By default the compiler checks for these issues and a developer can switch between default checked arithmetic and unchecked arithmetic.
###### Fixed Point
- Fixed-point numbers are a way to use whole numbers to represent numbers that have decimals.
	- E.g.: If you want to represent $1.50, you multiply it by 100 (because there are two decimal places). 
	- So: $1.50 becomes 150 (because 1.50 × 100 = 150).
	- A common choice is 18 digits  in Ethereum, which means we can represent very precise values.
	- So $1.50 becomes 1500000000000000000 (which is 1.5 × $10^{18}$.
	- Do Math with Whole number and we can convert back by dividing by the same scale factor
	- 1500000000000000000 ÷ $10^{18}$ = $1.50.
- [[Ethereum]] uses a smallest unit called **wei**, where 1 Ether (ETH) equals $10^{18}$ wei. This means that when dealing with Ether, all calculations can be done using whole numbers, which helps avoid precision issues associated with floating-point arithmetic. By aligning token decimals with the wei system, it ensures that tokens can easily interact with Ether without conversion complications
- There is no real support for Fixed point number and have to depend on libraries
###### Address
- 
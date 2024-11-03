- a data type that can store whole numbers ranging from 0 to (2^{256} - 1).
- unsigned integer of 256 bits.

- a bit is smallest unit of data in computing. It is binary, holding either 0 or 1.
- 1 byte = 8 bits
- An 8-bit binary number can represent 256 different combinations of 0s and 1s. i.e. 2 ** 8 = 256
- no. of bytes = no. of bits / 8
- therefore, uint256 occupies 32 bytes of memory.
- 
---
#### Let's see how 

A) 1 bit = 0 or 1
	the numbers 1 bit can represent = 2 numbers

B) how many numbers can 2 bits represent?
	- 00
	- 01
	- 10
	- 11
			so 4 numbers.

C) how many numbers can 3 bits represent?
	here we know either it starts with 0 or 1 and followed by two bits or 0s and 1s like above
	- 0XX = num of 2 bit repr.
	- 1XX = num of 2 bit repr.
	      = 2 * num of 2 bit repr
	      = 2 * 4(from above)
	      = 8 numbers.

D)  how many numbers can 4 bits represent?
		we can apply the same reasoning to calculate here also
		- 0XXX = num of 8 bit repr.
		- 1XXX = num of 8 bit repr.
		= 2 * num of 8 bit repr.
		= 2 * 8
		= 16 numbers.

1 bit   = 2 ** 1 = 2 nums
2 bits = 2 ** 2 = 4 nums
3 bits = 2 ** 3 = 8 nums
4 bits = 2 ** 4 = 16 nums
8 bits = 2 ** 8 = 256 nums
....
N bits = 2 ** N nums

How many numbers can 1 byte represent?
	= 2 ** 8 
	= 256 numbers


---


- 1 hex character = 4 bits
	- 1 hex is a number between 0 - 15, i.e. 16 numbers or 2 ** 4
	- = 2 ** 4 numbers
	- = can be represented by 4 bits
- 1 byte = 8 bits = 2 hex
- we know a byte can be represented by 2 hex values
- so uint256 = 32bytes 
		    = 32 * (2 hex) 
		    = 64 hex
- so 32 bytes can be represented in 64 hex values
- and each hex can be represented with 4 bits
	- = 64 * 4 bits 
	- = 256 bits
	- = uin256